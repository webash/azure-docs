---
title: Working with Azure Cosmos DB Cassandra API from Spark 
description: This article is the main page for Cosmos DB Cassandra API integration from Spark.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/01/2019

---

# Connect to Azure Cosmos DB Cassandra API from Spark
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

This article is one among a series of articles on Azure Cosmos DB Cassandra API integration from Spark. The articles cover connectivity, Data Definition Language(DDL) operations, basic Data Manipulation Language(DML) operations, and advanced Azure Cosmos DB Cassandra API integration from Spark. 

## Prerequisites
* [Provision an Azure Cosmos DB Cassandra API account.](create-cassandra-dotnet.md#create-a-database-account)

* Provision your choice of Spark environment [[Azure Databricks](/azure/databricks/scenarios/quickstart-create-databricks-workspace-portal) | [Azure HDInsight-Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) | Others].

## Dependencies for connectivity
* **Spark connector for Cassandra:**
  Spark connector is used to connect to Azure Cosmos DB Cassandra API.  Identify and use the version of the connector located in [Maven central]( https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) that is compatible with the Spark and Scala versions of your Spark environment. We recommend an environment which supports Spark 3.0 or higher, and the spark connector available at maven coordinates `com.datastax.spark:spark-cassandra-connector-assembly_2.12:3.0.0`. If using Spark 2.x, we recommend an environment with Spark version 2.4.5, using spark connector at maven coordinates `com.datastax.spark:spark-cassandra-connector_2.11:2.4.3`.


* **Azure Cosmos DB helper library for Cassandra API:**
  If you are using a version Spark 2.x then in addition to the Spark connector, you need another library called [azure-cosmos-cassandra-spark-helper]( https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.2.0/jar) with maven coordinates `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.2.0` from Azure Cosmos DB in order to handle [rate limiting](./manage-scale-cassandra.md#handling-rate-limiting-429-errors). This library contains custom connection factory and retry policy classes.

  The retry policy in Azure Cosmos DB is configured to handle HTTP status code 429("Request Rate Large") exceptions. The Azure Cosmos DB Cassandra API translates these exceptions into overloaded errors on the Cassandra native protocol, and you can retry with back-offs. Because Azure Cosmos DB uses provisioned throughput model, request rate limiting exceptions occur when the ingress/egress rates increase. The retry policy protects your spark jobs against data spikes that momentarily exceed the throughput allocated for your container. If using the Spark 3.x connector, implementing this library is not required. 

  > [!NOTE] 
  > The retry policy can protect your spark jobs against momentary spikes only. If you have not configured enough RUs required to run your workload, then the retry policy is not applicable and the retry policy class rethrows the exception.

* **Azure Cosmos DB account connection details:** Your Azure Cassandra API account name, account endpoint, and key.

## Optimizing Spark connector throughput configuration 

Listed in the next section are all the relevant parameters for controlling throughput using the Spark Connector for Cassandra. In order to optimize parameters to maximize throughput for spark jobs, the `spark.cassandra.output.concurrent.writes`, `spark.cassandra.concurrent.reads`, and `spark.cassandra.input.reads_per_sec` configs needs to be correctly configured, in order to avoid too much throttling and back-off (which in turn can lead to lower throughput).

The optimal value of these configurations depends on 4 factors:

-	The amount of throughput (Request Units) configured for the table that data is being ingested into.
- The number of workers in your Spark cluster.
-	The number of executors configured for your spark job (which can be controlled using `spark.cassandra.connection.connections_per_executor_max` or `spark.cassandra.connection.remoteConnectionsPerExecutor` depending on Spark version)
-	The average latency of each request to cosmos DB, if you are collocated in the same Data Center. Assume this value to be 10 ms for writes and 3 ms for reads.

As an example, if we have 5 workers and a value of `spark.cassandra.output.concurrent.writes`= 1, and a value of `spark.cassandra.connection.remoteConnectionsPerExecutor` = 1, then we have 5 workers that are concurrently writing into the table, each with 1 thread. If it takes 10 ms to perform a single write, then we can send 100 requests (1000 milliseconds divided by 10) per second, per thread. With 5 workers, this would be 500 writes per second. At an average cost of 5 request units (RUs) per write, the target table would need a minimum 2500 request units provisioned (5 RUs x 500 writes per second).

Increasing the number of executors can increase the number of threads in a given job, which can in turn increase throughput. However, the exact impact of this can be variable depending on the job, while controlling throughput with number of workers is more deterministic. You can also determine the exact cost of a given request by profiling it to get the Request Unit (RU) charge. This will help you to be more accurate when provisioning throughput for your table or keyspace. Have a look at our article [here](./find-request-unit-charge-cassandra.md) to understand how to get request unit charges at a per request level. 

> [!NOTE]
> The guidance above assumes a reasonably uniform distribution of data. If you have a significant skew in the data (that is, an inordinately large number of reads/writes to the same partition key value), then you might still experience bottlenecks, even if you have a large number of [request units](./request-units.md) provisioned in your table. Request units are divided equally among physical partitions, and heavy data skew can cause a bottleneck of requests to a single partition.
    
## Spark connector throughput configuration parameters

The following table lists Azure Cosmos DB Cassandra API-specific throughput configuration parameters provided by the connector. For a detailed list of all configuration parameters, see [configuration reference](https://github.com/datastax/spark-cassandra-connector/blob/master/doc/reference.md) page of the Spark Cassandra Connector GitHub repository.

| **Property Name** | **Default value** | **Description** |
|---------|---------|---------|
| spark.cassandra.output.batch.size.rows |  1 |Number of rows per single batch. Set this parameter to 1. This parameter is used to achieve higher throughput for heavy workloads. |
| spark.cassandra.connection.connections_per_executor_max (Spark 2.x) spark.cassandra.connection.remoteConnectionsPerExecutor (Spark 3.x)  | None | Maximum number of connections per node per executor. 10*n is equivalent to 10 connections per node in an n-node Cassandra cluster. So, if you require 5 connections per node per executor for a 5 node Cassandra cluster, then you should set this configuration to 25. Modify this value based on the degree of parallelism or the number of executors that your spark jobs are configured for.   |
| spark.cassandra.output.concurrent.writes  |  100 | Defines the number of parallel writes that can occur per executor. Because you set "batch.size.rows" to 1, make sure to scale up this value accordingly. Modify this value based on the degree of parallelism or the throughput that you want to achieve for your workload. |
| spark.cassandra.concurrent.reads |  512 | Defines the number of parallel reads that can occur per executor. Modify this value based on the degree of parallelism or the throughput that you want to achieve for your workload  |
| spark.cassandra.output.throughput_mb_per_sec  | None | Defines the total write throughput per executor. This parameter can be used as an upper limit for your spark job throughput, and base it on the provisioned throughput of your Cosmos container.   |
| spark.cassandra.input.reads_per_sec| None   | Defines the total read throughput per executor. This parameter can be used as an upper limit for your spark job throughput, and base it on the provisioned throughput of your Cosmos container.  |
| spark.cassandra.output.batch.grouping.buffer.size |  1000  | Defines the number of batches per single spark task that can be stored in memory before sending to Cassandra API |
| spark.cassandra.connection.keep_alive_ms | 60000 | Defines the period of time until which unused connections are available. | 

Adjust the throughput and degree of parallelism of these parameters based on the workload you expect for your spark jobs, and the throughput you have provisioned for your Cosmos DB account.


## Connecting to Azure Cosmos DB Cassandra API from Spark

### cqlsh
The following commands detail how to connect to Azure CosmosDB Cassandra API from cqlsh.  This is useful for validation as you run through the samples in Spark.<br>
**From Linux/Unix/Mac:**

```bash
export SSL_VERSION=TLSv1_2
export SSL_VALIDATE=false
cqlsh.py YOUR-COSMOSDB-ACCOUNT-NAME.cassandra.cosmosdb.azure.com 10350 -u YOUR-COSMOSDB-ACCOUNT-NAME -p YOUR-COSMOSDB-ACCOUNT-KEY --ssl
```

### 1.  Azure Databricks
The article below covers Azure Databricks cluster provisioning, cluster configuration for connecting to Azure Cosmos DB Cassandra API, and several sample notebooks that cover DDL operations, DML operations and more.<BR>
[Work with Azure Cosmos DB Cassandra API from Azure Databricks](cassandra-spark-databricks.md)<BR>
  
### 2.  Azure HDInsight-Spark
The article below covers HDinsight-Spark service, provisioning, cluster configuration for connecting to Azure Cosmos DB Cassandra API, and several sample notebooks that cover DDL operations, DML operations and more.<BR>
[Work with Azure Cosmos DB Cassandra API from Azure HDInsight-Spark](cassandra-spark-hdinsight.md)
 
### 3.  Spark environment in general
While the sections above were specific to Azure Spark-based PaaS services, this section covers any general Spark environment.  Connector dependencies, imports, and Spark session configuration are detailed below. The "Next steps" section covers code samples for DDL operations, DML operations and more.  

#### Connector dependencies:

1. Add the maven coordinates to get the [Cassandra connector for Spark](cassandra-spark-generic.md#dependencies-for-connectivity)
2. Add the maven coordinates for the [Azure Cosmos DB helper library](cassandra-spark-generic.md#dependencies-for-connectivity) for Cassandra API

#### Imports:

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra
```

#### Spark session configuration:

```scala
//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related. You can adjust the values as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
//spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10") // Spark 2.x
spark.conf.set("spark.cassandra.connection.remoteConnectionsPerExecutor", "10") // Spark 3.x
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## Next steps

The following articles demonstrate Spark integration with Azure Cosmos DB Cassandra API. 
 
* [DDL operations](cassandra-spark-ddl-ops.md)
* [Create/insert operations](cassandra-spark-create-ops.md)
* [Read operations](cassandra-spark-read-ops.md)
* [Upsert operations](cassandra-spark-upsert-ops.md)
* [Delete operations](cassandra-spark-delete-ops.md)
* [Aggregation operations](cassandra-spark-aggregation-ops.md)
* [Table copy operations](cassandra-spark-table-copy-ops.md)