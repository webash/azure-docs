---
title: Get supported document formats method
titleSuffix: Azure Cognitive Services
description: The get supported document formats method returns a list of supported document formats.
services: cognitive-services
author: jann-skotdal
manager: nitinme

ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 04/21/2021
ms.author: v-jansk
---

# Get supported document formats

The Get supported document formats method returns a list of document formats supported by the Document Translation service. The list includes the common file extension, and the content-type if using the upload API.

## Request URL

Send a `GET` request to:
```HTTP
GET https://<NAME-OF-YOUR-RESOURCE>.cognitiveservices.azure.com/translator/text/batch/v1.0/documents/formats
```

Learn how to find your [custom domain name](../get-started-with-document-translation.md#find-your-custom-domain-name).

> [!IMPORTANT]
>
> * **All API requests to the Document Translation service require a custom domain endpoint**.
> * You can't use the endpoint found on your Azure portal resource _Keys and Endpoint_ page nor the global translator endpoint—`api.cognitive.microsofttranslator.com`—to make HTTP requests to Document Translation.

## Request headers

Request headers are:

|Headers|Description|
|-----|-----|
|Ocp-Apim-Subscription-Key|Required request header|

## Response status codes

The following are the possible HTTP status codes that a request returns.

|Status Code|Description|
|-----|-----|
|200|OK. Returns the list of supported document file formats.|
|500|Internal Server Error.|
|Other Status Codes|<ul><li>Too many requests</li><li>Server temporary unavailable</li></ul>|

## File format response

### Successful fileFormatListResult response

The following information is returned in a successful response.

|Name|Type|Description|
|--- |--- |--- |
|value|FileFormat []|FileFormat[] contains the details listed below.|
|value.contentTypes|string[]|Supported Content-Types for this format.|
|value.defaultVersion|string|Default version if none is specified.|
|value.fileExtensions|string[]|Supported file extension for this format.|
|value.format|string|Name of the format.|
|value.versions|string [] | Supported version.|

### Error response

|Name|Type|Description|
|--- |--- |--- |
 |code|string|Enums containing high-level error codes. Possible values:<ul><li>InternalServerError</li><li>InvalidArgument</li><li>InvalidRequest</li><li>RequestRateTooHigh</li><li>ResourceNotFound</li><li>ServiceUnavailable</li><li>Unauthorized</li></ul>|
|message|string|Gets high-level error message.|
|innerError|InnerTranslationError|New Inner Error format which conforms to Cognitive Services API Guidelines. This contains required properties ErrorCode, message and optional properties target, details(key value pair), inner error(this can be nested).|
|innerError.code|string|Gets code error string.|
|innerError.message|string|Gets high-level error message.|
|innerError.target|string|Gets the source of the error. For example it would be "documents" or "document id" in case of invalid document.|

## Examples

### Example successful response
The following is an example of a successful response.

Status code: 200

```JSON
{
    "value": [
        {
            "format": "PlainText",
            "fileExtensions": [
                ".txt"
            ],
            "contentTypes": [
                "text/plain"
            ],
            "versions": []
        },
        {
            "format": "OpenXmlWord",
            "fileExtensions": [
                ".docx"
            ],
            "contentTypes": [
                "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
            ],
            "versions": []
        },
        {
            "format": "OpenXmlPresentation",
            "fileExtensions": [
                ".pptx"
            ],
            "contentTypes": [
                "application/vnd.openxmlformats-officedocument.presentationml.presentation"
            ],
            "versions": []
        },
        {
            "format": "OpenXmlSpreadsheet",
            "fileExtensions": [
                ".xlsx"
            ],
            "contentTypes": [
                "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
            ],
            "versions": []
        },
        {
            "format": "OutlookMailMessage",
            "fileExtensions": [
                ".msg"
            ],
            "contentTypes": [
                "application/vnd.ms-outlook"
            ],
            "versions": []
        },
        {
            "format": "HtmlFile",
            "fileExtensions": [
                ".html",
                ".htm"
            ],
            "contentTypes": [
                "text/html"
            ],
            "versions": []
        },
        {
            "format": "PortableDocumentFormat",
            "fileExtensions": [
                ".pdf"
            ],
            "contentTypes": [
                "application/pdf"
            ],
            "versions": []
        },
        {
            "format": "XLIFF",
            "fileExtensions": [
                ".xlf"
            ],
            "contentTypes": [
                "application/xliff+xml"
            ],
            "versions": [
                "1.0",
                "1.1",
                "1.2"
            ]
        },
        {
            "format": "TSV",
            "fileExtensions": [
                ".tsv",
                ".tab"
            ],
            "contentTypes": [
                "text/tab-separated-values"
            ],
            "versions": []
        },
        {
            "format": "CSV",
            "fileExtensions": [
                ".csv"
            ],
            "contentTypes": [
                "text/csv"
            ],
            "versions": []
        },
        {
            "format": "RichTextFormat",
            "fileExtensions": [
                ".rtf"
            ],
            "contentTypes": [
                "application/rtf"
            ],
            "versions": []
        },
        {
            "format": "WordDocument",
            "fileExtensions": [
                ".doc"
            ],
            "contentTypes": [
                "application/msword"
            ],
            "versions": []
        },
        {
            "format": "PowerpointPresentation",
            "fileExtensions": [
                ".ppt"
            ],
            "contentTypes": [
                "application/vnd.ms-powerpoint"
            ],
            "versions": []
        },
        {
            "format": "ExcelSpreadsheet",
            "fileExtensions": [
                ".xls"
            ],
            "contentTypes": [
                "application/vnd.ms-excel"
            ],
            "versions": []
        },
        {
            "format": "OpenDocumentText",
            "fileExtensions": [
                ".odt"
            ],
            "contentTypes": [
                "application/vnd.oasis.opendocument.text"
            ],
            "versions": []
        },
        {
            "format": "OpenDocumentPresentation",
            "fileExtensions": [
                ".odp"
            ],
            "contentTypes": [
                "application/vnd.oasis.opendocument.presentation"
            ],
            "versions": []
        },
        {
            "format": "OpenDocumentSpreadsheet",
            "fileExtensions": [
                ".ods"
            ],
            "contentTypes": [
                "application/vnd.oasis.opendocument.spreadsheet"
            ],
            "versions": []
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
