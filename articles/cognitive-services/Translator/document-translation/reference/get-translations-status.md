---
title: Get translations status
titleSuffix: Azure Cognitive Services
description: The get translations status method returns a list of batch requests submitted and the status for each request.
services: cognitive-services
author: jann-skotdal
manager: nitinme

ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/21/2021
ms.author: v-jansk
---

# Get translations status

The Get translations status method returns  a list of batch requests submitted and the status for each request. This list only contains batch requests submitted by the user (based on the resource).

If the number of requests exceeds our paging limit, server-side paging is used. Paginated responses indicate a partial result and include a continuation token in the response. The absence of a continuation token means that no other pages are available.

$top, $skip and $maxpagesize query parameters can be used to specify a number of results to return and an offset for the collection.

$top indicates the total number of records the user wants to be returned across all pages. $skip indicates the number of records to skip from the list of batches based on the sorting method specified. By default, we sort by descending start time. $maxpagesize is the maximum items returned in a page. If more items are requested via $top (or $top is not specified and there are more items to be returned), @nextLink will contain the link to the next page.

$orderBy query parameter can be used to sort the returned list (ex "$orderBy=createdDateTimeUtc asc" or "$orderBy=createdDateTimeUtc desc"). The default sorting is descending by createdDateTimeUtc. Some query parameters can be used to filter the returned list (ex: "status=Succeeded,Cancelled") will only return succeeded and canceled operations. createdDateTimeUtcStart and createdDateTimeUtcEnd can be used combined or separately to specify a range of datetime to filter the returned list by. The supported filtering query parameters are (status, IDs, createdDateTimeUtcStart, createdDateTimeUtcEnd).

The server honors the values specified by the client. However, clients must be prepared to handle responses that contain a different page size or contain a continuation token.

When both $top and $skip are included, the server should first apply $skip and then $top on the collection. 

> [!NOTE]
> If the server can't honor $top and/or $skip, the server must return an error to the client informing about it instead of just ignoring the query options. This reduces the risk of the client making assumptions about the data returned.

## Request URL

Send a `GET` request to:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0/batches
```

Learn how to find your [custom domain name](../get-started-with-document-translation.md#find-your-custom-domain-name).

> [!IMPORTANT]
>
> * **All API requests to the Document Translation service require a custom domain endpoint**.
> * You can't use the endpoint found on your Azure portal resource _Keys and Endpoint_ page nor the global translator endpoint—`api.cognitive.microsofttranslator.com`—to make HTTP requests to Document Translation.

## Request parameters

Request parameters passed on the query string are:

|Query parameter|In|Required|Type|Description|
|--- |--- |--- |---|---|
|$maxpagesize|query|False|integer int32|$maxpagesize is the maximum items returned in a page. If more items are requested via $top (or $top is not specified and there are more items to be returned), @nextLink will contain the link to the next page. Clients MAY request server-driven paging with a specific page size by specifying a $maxpagesize preference. The server SHOULD honor this preference if the specified page size is smaller than the server's default page size.|
|$orderBy|query|False|array|The sorting query for the collection (ex: 'CreatedDateTimeUtc asc', 'CreatedDateTimeUtc desc')|
|$skip|query|False|integer int32|$skip indicates the number of records to skip from the list of records held by the server based on the sorting method specified. By default, we sort by descending start time. Clients MAY use $top and $skip query parameters to specify a number of results to return and an offset into the collection. When both $top and $skip are given by a client, the server SHOULD first apply $skip and then $top on the collection.Note: If the server can't honor $top and/or $skip, the server MUST return an error to the client informing about it instead of just ignoring the query options.|
|$top|query|False|integer int32|$top indicates the total number of records the user wants to be returned across all pages. Clients MAY use $top and $skip query parameters to specify a number of results to return and an offset into the collection. When both $top and $skip are given by a client, the server SHOULD first apply $skip and then $top on the collection.Note: If the server can't honor $top and/or $skip, the server MUST return an error to the client informing about it instead of just ignoring the query options.|
|createdDateTimeUtcEnd|query|False|string date-time|The end datetime to get items before.|
|createdDateTimeUtcStart|query|False|string date-time|The start datetime to get items after.|
|ids|query|False|array|IDs to use in filtering.|
|statuses|query|False|array|Statuses to use in filtering.|

## Request headers

Request headers are:

|Headers|Description|
|--- |--- |
|Ocp-Apim-Subscription-Key|Required request header|

## Response status codes

The following are the possible HTTP status codes that a request returns.

|Status Code|Description|
|--- |--- |
|200|OK. Successful request and returns the status of the all the operations. HeadersRetry-After: integerETag: string|
|400|Bad Request. Invalid request. Check input parameters.|
|401|Unauthorized. Check your credentials.|
|500|Internal Server Error.|
|Other Status Codes|<ul><li>Too many requests</li><li>Server temporary unavailable</li></ul>|

## Get translations status response

### Successful get translations status response

The following information is returned in a successful response.

|Name|Type|Description|
|--- |--- |--- |
|@nextLink|string|Url for the next page. Null if no more pages available.|
|value|TranslationStatus[]|TranslationStatus[] array listed below|
|value.id|string|ID of the operation.|
|value.createdDateTimeUtc|string|Operation created date time.|
|value.lastActionDateTimeUtc|string|Date time in which the operation's status has been updated.|
|value.status|String|List of possible statuses for job or document: <ul><li>Canceled</li><li>Cancelling</li><li>Failed</li><li>NotStarted</li><li>Running</li><li>Succeeded</li><li>ValidationFailed</li></ul>|
|value.summary|StatusSummary[]|Summary containing the details listed below.|
|value.summary.total|integer|Count of total documents.|
|value.summary.failed|integer|Count of documents failed.|
|value.summary.success|integer|Count of documents successfully translated.|
|value.summary.inProgress|integer|Count of documents in progress.|
|value.summary.notYetStarted|integer|Count of documents not yet started processing.|
|value.summary.cancelled|integer|Count of documents canceled.|
|value.summary.totalCharacterCharged|integer|Total count of characters charged.|

### Error response

|Name|Type|Description|
|--- |--- |--- |
|code|string|Enums containing high-level error codes. Possible values:<br/><ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>Unauthorized</li></ul>|
|message|string|Gets high-level error message.|
|target|string|Gets the source of the error. For example, it would be "documents" or "document ID" if there was an invalid document.|
|innerError|InnerTranslationError|New Inner Error format thath conforms to Cognitive Services API Guidelines. This contains required properties ErrorCode, message, and optional properties target, details (key value pair), inner error (this can be nested).|
|innerError.code|string|Gets code error string.|
|innerError.message|string|Gets high-level error message.|
|innerError.target|string|Gets the source of the error. For example, it would be "documents" or "document ID" if there was an invalid document.|

## Examples

### Example successful response

The following is an example of a successful response.

```JSON
{
  "value": [
    {
      "id": "273622bd-835c-4946-9798-fd8f19f6bbf2",
      "createdDateTimeUtc": "2021-03-23T07:03:30.013631Z",
      "lastActionDateTimeUtc": "2021-03-26T01:00:00Z",
      "status": "Succeeded",
      "summary": {
        "total": 10,
        "failed": 1,
        "success": 9,
        "inProgress": 0,
        "notYetStarted": 0,
        "cancelled": 0,
        "totalCharacterCharged": 1000
      }
    }
  ]
}
```

### Example error response

The following is an example of an error response. The schema for other error codes is the same.

Status code: 500

```JSON
{
  "error": {
    "code": "InternalServerError",
    "message": "Internal Server Error",
    "target": "Operation",
    "innerError": {
      "code": "InternalServerError",
      "message": "Unexpected internal server error has occurred"
    }
  }
}
```

## Next steps

Follow our quickstart to learn more about using Document Translation and the client library.

> [!div class="nextstepaction"]
> [Get started with Document Translation](../get-started-with-document-translation.md)
