---
title: "Python tutorial: Search integration highlights"
titleSuffix: Azure Cognitive Search
description: Understand the Python SDK Search integration queries used in the Search-enabled website with this cheat sheet. 
manager: nitinme
author: diberry
ms.author: diberry
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 05/25/2021
ms.custom: devx-track-python
ms.devlang: python
---

# 4 - Python Search integration cheat sheet

In the previous lessons, you added search to a Static Web App. This lesson highlights the essential steps that establish integration. If you are looking for a cheat sheet on how to integrate search into your Python app, this article explains what you need to know.

The application is available: 
* [Sample](https://github.com/Azure-Samples/azure-search-python-samples/tree/master/search-website)
* [Demo website - aka.ms/azs-good-books](https://aka.ms/azs-good-books)

## Azure SDK azure-search-documents

The Function app uses the Azure SDK for Cognitive Search:

* [PYPI package azure-search-documents](https://pypi.org/project/azure-search-documents/)
* [Reference Documentation](/python/api/azure-search-documents)

The Function app authenticates through the SDK to the cloud-based Cognitive Search API using your resource name, API key, and index name. The secrets are stored in the Static Web App settings and pulled in to the Function as environment variables. 

## Configure secrets in a configuration file

The Azure Function app settings environment variables are pulled in from a file, `__init__.py`, shared between the three API functions. 

:::code language="python" source="~/azure-search-python-samples/search-website/api/shared_code/__init__.py" highlight="6-9" :::

## Azure Function: Search the catalog

The Search [API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Search/__init__.py) takes a search term and searches across the documents in the Search Index, returning a list of matches. 

Routing for the Search API is contained in the [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Search/function.json) bindings.

The Azure Function pulls in the search configuration information, and fulfills the query.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Search/__init__.py" highlight="8-18, 122" :::

## Client: Search from the catalog

Call the Azure Function in the React client with the following code. 

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/pages/Search/Search.js" highlight="42-55" :::

## Azure Function: Suggestions from the catalog

The `Suggest` [API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Suggest/__init__.py) takes a search term while a user is typing and suggests search terms such as book titles and authors across the documents in the search index, returning a small list of matches. 

The search suggester, `sg`, is defined in the [schema file](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/bulk-upload/good-books-index.json) used during bulk upload.

Routing for the Suggest API is contained in the [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Suggest/function.json) bindings.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Suggest/__init__.py" highlight="8-23, 35" :::

## Client: Suggestions from the catalog

The Suggest function API is called in the React app at `\src\components\SearchBar\SearchBar.js` as part of component initialization:

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/components/SearchBar/SearchBar.js" highlight="52-60" :::

## Azure Function: Get specific document 

The `Lookup` [API](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Lookup/__init__.py) takes a ID and returns the document object from the Search Index. 

Routing for the Lookup API is contained in the [function.json](https://github.com/Azure-Samples/azure-search-python-samples/blob/master/search-website/api/Lookup/function.json) bindings.

:::code language="python" source="~/azure-search-python-samples/search-website/api/Lookup/__init__.py" highlight="8-18, 27" :::

## Client: Get specific document 

This function API is called in the React app at `\src\pages\Details\Detail.js` as part of component initialization:

:::code language="javascript" source="~/azure-search-python-samples/search-website/src/pages/Details/Details.js" highlight="19-29" :::

## Next steps

* [Index Azure SQL data](search-indexer-tutorial.md)
