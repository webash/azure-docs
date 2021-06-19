---
title: Troubleshoot migration of Hive from 3.6 to 4.0 - Azure HDInsight
description: Troubleshooting guide for migration of Hive workloads from HDInsight 3.6 to 4.0
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 05/24/2021
---

# Troubleshooting guide for migration of Hive workloads from HDInsight 3.6 to HDInsight 4.0

This article provides answers to some of the most common issues that customers face when migrating Hive workloads from HDInsight 3.6 to HDInsight 4.0.

## Change parquet files to store timestamp in UTC time zone

Workaround:
1. ssh both head nodes and backup below jar files in case you need to roll back. Don't keep backup under the same path.
    `/usr/hdp/4.1.2.5/hive/lib/hive-common-3.1.2.4.1.2.5.jar` and `hive-exec-3.1.2.4.1.2.5.jar`

2. Download these two jars files
    - [https://wenjmshare.blob.core.windows.net/public/hive-common-3.1.2.4.1.2.5.jar](https://wenjmshare.blob.core.windows.net/public/hive-common-3.1.2.4.1.2.5.jar) 
    - [https://wenjmshare.blob.core.windows.net/public/hive-exec-3.1.2.4.1.2.5.jar](https://wenjmshare.blob.core.windows.net/public/hive-exec-3.1.2.4.1.2.5.jar)

3. Move the downloaded jars to `/usr/hdp/4.1.2.5/hive/lib` location on both head nodes.

4. Restart HS2 on the Ambari UI.


## Reduce latency when running `DESCRIBE TABLE_NAME`

Workaround:
* Increase maximum number of objects (tables/partitions) that can be retrieved from metastore in one batch. Set it to a large number (default is 300) till satisfactory latency levels are reached. The higher the number, the fewer round trips are needed to the Hive metastore server, but it may also cause higher memory requirement at the client side.

    ```hive.metastore.batch.retrieve.max=2000```
* Restart Hive and all stale services

## Unable to query Gzipped text file if skip.header.line.count and skip.footer.line.count are set for table

Issue has been fixed in Interactive Query 4.0 but still not in Interactive Query 3.1.0

Workaround:
* Create table without using ```"skip.header.line.count"="1"``` and ```"skip.footer.line.count"="1"```, then create view from the original table that excludes the header/footer row in the query.

## Unable to use Unicode characters

Workaround:
1. Connect to the hive metastore database for your cluster.

2. Backup the `TBLS` and `TABLE_PARAMS` table using the following command:
    ```sql
        select * into tbls_bak from tbls;
        select * into table_params_bak from table_params;
    ```

3. Manually change the affected column types to `nvarchar(max)`.
    ```sql 
        alter table TABLE_PARAMS alter column PARAM_VALUE nvarchar(max);
        alter table TBLS alter column VIEW_EXPANDED_TEXT nvarchar(max) null;    
        alter table TBLS alter column VIEW_ORIGINAL_TEXT nvarchar(max) null;
    ```

## Create table as select (CTAS) creates a new table with same UUID

Hive 3.1 (HDInsight 4.0) offers a built-in UDF to generate unique UUIDs. Hive UUID() method generates unique IDs even with CTAS. You can leverage it as follows.
```hql
create table rhive as
select uuid() as UUID
from uuid_test
```

## Hive job output format differs from HDInsight 3.6

It is caused by the difference of WebHCat(Templeton) between HDInsight 3.6 and HDInsight 4.0.

* Hive Rest API - add ```arg=--showHeader=false -d arg=--outputformat=tsv2 -d```

* .NET SDK - initialize args of ```HiveJobSubmissionParameters```
    ```csharp
    List<string> args = new List<string> { { "--showHeader=false" }, { "--outputformat=tsv2" } };
                var parameters = new HiveJobSubmissionParameters
                {
                    Query = "SELECT clientid,market from hivesampletable LIMIT 10",
                    Defines = defines,
                    Arguments = args
                };
    ```

## Reduce Hive internal table creation latency

1. From Advanced hive-site and Advanced hivemetastore-site, delete the value ```org.apache.hive.hcatalog.listener.DbNotificationListener``` for ```hive.metastore.transactional.event.listeners```. 

2. If ```hive.metastore.event.listeners``` has a value, remove it.

3. DbNotificationListener is needed only if you use REPL commands and if not, it is safe to remove it.

    :::image type="content" source="./media/apache-hive-40-migration-guide/hive-reduce-internal-table-creation-latency.png" alt-text="Reduce internal table latency in HDInsight 4.0" border="true":::

## Change Hive default table location

This is a by-design behavior change on HDInsight 4.0 (Hive 3.1). The major reason of this change is for file permission control purposes. 

To create external tables under a custom location, specify the location in the create table statement.

## Disable ACID in HDInsight 4.0

We recommend enabling ACID in HDInsight 4.0 as most of the recent enhancements (both functional and performance) in Hive are made available only for ACID tables.

Steps to disable ACID on HDInsight 4.0:
1. Change the following hive configurations in Ambari:

    ```text
    hive.strict.managed.tables=false
    hive.support.concurrency=false; 
    hive.txn.manager=org.apache.hadoop.hive.ql.lockmgr.DummyTxnManager;
    hive.enforce.bucketing=false;
    hive.compactor.initiator.on=false;
    hive.compactor.worker.threads=0;
    hive.create.as.insert.only=false;
    metastore.create.as.acid=false;
    ```

2. Restart hive service.

> [!IMPORTANT]
> Microsoft recommends against sharing the same data/storage with HDInsight 3.6 and HDInsight 4.0 Hive-managed tables.It is an unsupported scenario.

* Normally, above configurations should be set even before creating any Hive tables on HDInsight 4.0 cluster. We shouldn't disable ACID once managed tables are created. It would potentially cause data loss or inconsistent results. So, it is recommended to set it once when you create a new cluster and don’t change it later.

* Disabling ACID after creating tables is risky, however in case you want to do it, please follow the below steps to avoid potential data loss or inconsistency:

    1. Create an external table with same schema and copy the data from original managed table using CTAS command ```create external table e_t1 select * from m_t1```.    
    2. Drop managed table using ```drop table m_t1```.
    3. Disable ACID using the configs suggested.        
    4. Create m_t1 again and copy data from external table using CTAS command ```create table m_t1 select * from e_t1```.        
    5. Drop external table using ```drop table e_t1```.

Make sure all managed tables are converted to external tables and dropped before disabling ACID. Also, compare the schema and data after each step to avoid any discrepancy.

## Create Hive external table with 755 permission

This issue can be resolved by either of the following two options:

1. Manually set the folder permission to 757 or 777, to allow hive user to write to the directory.

2. Change the “Hive Authorization Manager” from ```org.apache.hadoop.hive.ql.security.authorization.StorageBasedAuthorizationProvider``` to ```org.apache.hadoop.hive.ql.security.authorization.MetaStoreAuthzAPIAuthorizerEmbedOnly```.

MetaStoreAuthzAPIAuthorizerEmbedOnly effectively disables security checks because the Hive metastore is not embedded in HDInsight 4.0. However, this may bring other potential issues. Please exercise caution when using this option.

## Permission errors in Hive job after upgrading to HDInsight 4.0

* In HDInsight 4.0, all cluster shapes with Hive components are configured with a new authorization provider:
```org.apache.hadoop.hive.ql.security.authorization.StorageBasedAuthorizationProvider```

* HDFS file permissions should be assigned to the hive user for the file being accessed. The error message provides the details needed to resolve the issue.

* You can also switch to ```MetaStoreAuthzAPIAuthorizerEmbedOnly``` provider used in HDInsight 3.6 Hive clusters.
```org.apache.hadoop.hive.ql.security.authorization.MetaStoreAuthzAPIAuthorizerEmbedOnly```

    :::image type="content" source="./media/apache-hive-40-migration-guide/hive-job-permission-errors.png" alt-text="Set authorization to MetaStoreAuthzAPIAuthorizerEmbedOnly" border="true":::

## Next steps

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]