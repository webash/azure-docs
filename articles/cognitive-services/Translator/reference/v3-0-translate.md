---
title: Translator Translate Method
titleSuffix: Azure Cognitive Services
description: Understand the parameters, headers, and body messages for the Translate method of Azure Cognitive Services Translator to translate text.
services: cognitive-services
author: laujan
manager: nitinme

ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/12/2021
ms.author: lajanuar
---

# Translator 3.0: Translate

Translates text.

## Request URL

Send a `POST` request to:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## Request parameters

Request parameters passed on the query string are:

### Required parameters

| Query parameter | Description |
| --- | --- |
| api-version | _Required parameter_.  <br>Version of the API requested by the client. Value must be `3.0`. |
| to  | _Required parameter_.  <br>Specifies the language of the output text. The target language must be one of the [supported languages](v3-0-languages.md) included in the `translation` scope. For example, use `to=de` to translate to German.  <br>It's possible to translate to multiple languages simultaneously by repeating the parameter in the query string. For example, use `to=de&to=it` to translate to German and Italian. |

### Optional parameters



| Query parameter | Description |
| --- | --- |


| Query parameter | Description |
| --- | --- |
| from | _Optional parameter_.  <br>Specifies the language of the input text. Find which languages are available to translate from by looking up [supported languages](../reference/v3-0-languages.md) using the `translation` scope. If the `from` parameter is not specified, automatic language detection is applied to determine the source language.  <br>  <br>You must use the `from` parameter rather than autodetection when using the [dynamic dictionary](../dynamic-dictionary.md) feature. |
| textType | _Optional parameter_.  <br>Defines whether the text being translated is plain text or HTML text. Any HTML needs to be a well-formed, complete element. Possible values are: `plain` (default) or `html`. |
| category | _Optional parameter_.  <br>A string specifying the category (domain) of the translation. This parameter is used to get translations from a customized system built with [Custom Translator](../customization.md). Add the Category ID from your Custom Translator [project details](../custom-translator/how-to-create-project.md#view-project-details) to this parameter to use your deployed customized system. Default value is: `general`. |
| profanityAction | _Optional parameter_.  <br>Specifies how profanities should be treated in translations. Possible values are: `NoAction` (default), `Marked` or `Deleted`. To understand ways to treat profanity, see [Profanity handling](#handle-profanity). |
| profanityMarker | _Optional parameter_.  <br>Specifies how profanities should be marked in translations. Possible values are: `Asterisk` (default) or `Tag`. To understand ways to treat profanity, see [Profanity handling](#handle-profanity). |
| includeAlignment | _Optional parameter_.  <br>Specifies whether to include alignment projection from source text to translated text. Possible values are: `true` or `false` (default). |
| includeSentenceLength | _Optional parameter_.  <br>Specifies whether to include sentence boundaries for the input text and the translated text. Possible values are: `true` or `false` (default). |
| suggestedFrom | _Optional parameter_.  <br>Specifies a fallback language if the language of the input text can't be identified. Language autodetection is applied when the `from` parameter is omitted. If detection fails, the `suggestedFrom` language will be assumed. |
| fromScript | _Optional parameter_.  <br>Specifies the script of the input text. |
| toScript | _Optional parameter_.  <br>Specifies the script of the translated text. |
| allowFallback | _Optional parameter_.  <br>Specifies that the service is allowed to fall back to a general system when a custom system does not exist. Possible values are: `true` (default) or `false`.  <br>  <br>`allowFallback=false` specifies that the translation should only use systems trained for the `category` specified by the request. If a translation for language X to language Y requires chaining through a pivot language E, then all the systems in the chain (X->E and E->Y) will need to be custom and have the same category. If no system is found with the specific category, the request will return a 400 status code. `allowFallback=true` specifies that the service is allowed to fall back to a general system when a custom system does not exist. |

Request headers include:

| Headers | Description |
| --- | --- |
| Authentication header(s) | _Required request header_.  <br>See [available options for authentication](./v3-0-reference.md#authentication). |
| Content-Type | _Required request header_.  <br>Specifies the content type of the payload.  <br>Accepted value is `application/json; charset=UTF-8`. |
| Content-Length | _Required request header_.  <br>The length of the request body. |
| X-ClientTraceId | _Optional_.  <br>A client-generated GUID to uniquely identify the request. You can omit this header if you include the trace ID in the query string using a query parameter named `ClientTraceId`. |

## Request body

The body of the request is a JSON array. Each array element is a JSON object with a string property named `Text`, which represents the string to translate.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

The following limitations apply:

* The array can have at most 100 elements.
* The entire text included in the request cannot exceed 10,000 characters including spaces.

## Response body

A successful response is a JSON array with one result for each string in the input array. A result object includes the following properties:

  * `detectedLanguage`: An object describing the detected language through the following properties:

      * `language`: A string representing the code of the detected language.

      * `score`: A float value indicating the confidence in the result. The score is between zero and one and a low score indicates a low confidence.

    The `detectedLanguage` property is only present in the result object when language autodetection is requested.

  * `translations`: An array of translation results. The size of the array matches the number of target languages specified through the `to` query parameter. Each element in the array includes:

    * `to`: A string representing the language code of the target language.

    * `text`: A string giving the translated text.

    * `transliteration`: An object giving the translated text in the script specified by the `toScript` parameter.

      * `script`: A string specifying the target script.

      * `text`: A string giving the translated text in the target script.

    The `transliteration` object is not included if transliteration does not take place.

    * `alignment`: An object with a single string property named `proj`, which maps input text to translated text. The alignment information is only provided when the request parameter `includeAlignment` is `true`. Alignment is returned as a string value of the following format: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  The colon separates start and end index, the dash separates the languages, and space separates the words. One word may align with zero, one, or multiple words in the other language, and the aligned words may be non-contiguous. When no alignment information is available, the alignment element will be empty. See [Obtain alignment information](#obtain-alignment-information) for an example and restrictions.

    * `sentLen`: An object returning sentence boundaries in the input and output texts.

      * `srcSentLen`: An integer array representing the lengths of the sentences in the input text. The length of the array is the number of sentences, and the values are the length of each sentence.

      * `transSentLen`:  An integer array representing the lengths of the sentences in the translated text. The length of the array is the number of sentences, and the values are the length of each sentence.

    Sentence boundaries are only included when the request parameter `includeSentenceLength` is `true`.

  * `sourceText`: An object with a single string property named `text`, which gives the input text in the default script of the source language. `sourceText` property is present only when the input is expressed in a script that's not the usual script for the language. For example, if the input were Arabic written in Latin script, then `sourceText.text` would be the same Arabic text converted into Arab script.

Examples of JSON responses are provided in the [examples](#examples) section.

## Response headers

| Headers | Description |
| --- | --- |
| X-RequestId | Value generated by the service to identify the request. It is used for troubleshooting purposes. |
| X-MT-System | Specifies the system type that was used for translation for each ‘to’ language requested for translation. The value is a comma-separated list of strings. Each string indicates a type:  <br><br>* Custom - Request includes a custom system and at least one custom system was used during translation.<br>* Team - All other requests |

## Response status codes

The following are the possible HTTP status codes that a request returns.

| ProfanityAction | Action |
| --- | --- |
| `NoAction` |NoAction is the default behavior. Profanity will pass from source to target.  <br>  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a jackass. |
| `Deleted` | Profane words will be removed from the output without replacement.  <br>  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is  |
| `Marked` | Profane words are replaced by a marker in the output. The marker depends on the `ProfanityMarker` parameter.  <br>  <br>For `ProfanityMarker=Asterisk`, profane words are replaced with `***`:  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a \\*\\*\\*.  <br>  <br>For `ProfanityMarker=Tag`, profane words are surrounded by XML tags &lt;profanity&gt; and &lt;/profanity&gt;:  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a &lt;profanity&gt;jackass&lt;/profanity&gt;. |

If an error occurs, the request will also return a JSON error response. The error code is a 6-digit number combining the 3-digit HTTP status code followed by a 3-digit number to further categorize the error. Common error codes can be found on the [v3 Translator reference page](./v3-0-reference.md#errors).

## Examples

### Translate a single input

This example shows how to translate a single sentence from English to Simplified Chinese.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

The response body is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

The `translations` array includes one element, which provides the translation of the single piece of text in the input.

### Translate a single input with language autodetection

This example shows how to translate a single sentence from English to Simplified Chinese. The request does not specify the input language. Autodetection of the source language is used instead.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

The response body is:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
The response is similar to the response from the previous example. Since language autodetection was requested, the response also includes information about the language detected for the input text. The language autodetection works better with longer input text.

### Translate with transliteration

Let's extend the previous example by adding transliteration. The following request asks for a Chinese translation written in Latin script.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans&toScript=Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

The response body is:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"script":"Latn", "text":"nǐ hǎo , nǐ jiào shén me míng zì ？"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

The translation result now includes a `transliteration` property, which gives the translated text using Latin characters.

### Translate multiple pieces of text

Translating multiple strings at once is simply a matter of specifying an array of strings in the request body.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

The response contains the translation of all pieces of text in the exact same order as in the request.
The response body is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### Translate to multiple languages

This example shows how to translate the same input to several languages in one request.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'Hello, what is your name?'}]"
```

The response body is:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### Handle profanity

Normally the Translator service will retain profanity that is present in the source in the translation. The degree of profanity and the context that makes words profane differ between cultures, and as a result the degree of profanity in the target language may be amplified or reduced.

If you want to avoid getting profanity in the translation, regardless of the presence of profanity in the source text, you can use the profanity filtering option. The option allows you to choose whether you want to see profanity deleted, whether you want to mark profanities with appropriate tags (giving you the option to add your own post-processing), or you want no action taken. The accepted values of `ProfanityAction` are `Deleted`, `Marked` and `NoAction` (default).


| ProfanityAction | Action |
| --- | --- |
| `NoAction` | NoAction is the default behavior. Profanity will pass from source to target.  <br>  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a jackass. |
| `Deleted` | Profane words will be removed from the output without replacement.  <br>  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a. |
| `Marked` | Profane words are replaced by a marker in the output. The marker depends on the `ProfanityMarker` parameter.  <br>  <br>For `ProfanityMarker=Asterisk`, profane words are replaced with `***`:  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a \\*\\*\\*.  <br>  <br>For `ProfanityMarker=Tag`, profane words are surrounded by XML tags &lt;profanity&gt; and &lt;/profanity&gt;:  <br>**Example Source (Japanese)**: 彼はジャッカスです。  <br>**Example Translation (English)**: He is a &lt;profanity&gt;jackass&lt;/profanity&gt;. |

For example:

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'This is a freaking good idea.'}]"
```
This request returns:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Compare with:

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'This is a freaking good idea.'}]"
```

That last request returns:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### Translate content with markup and decide what's translated

It's common to translate content that includes markup such as content from an HTML page or content from an XML document. Include query parameter `textType=html` when translating content with tags. In addition, it's sometimes useful to exclude specific content from translation. You can use the attribute `class=notranslate` to specify content that should remain in its original language. In the following example, the content inside the first `div` element will not be translated, while the content in the second `div` element will be translated.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Here is a sample request to illustrate.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

The response is:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### Obtain alignment information

Alignment is returned as a string value of the following format for every word of the source. The information for each word is separated by a space, including for non-space-separated languages (scripts) like Chinese:

[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]] *

Example alignment string: "0:0-7:10 1:2-11:20 3:4-0:3 3:4-4:6 5:5-21:21".

In other words, the colon separates start and end index, the dash separates the languages, and space separates the words. One word may align with zero, one, or multiple words in the other language, and the aligned words may be non-contiguous. When no alignment information is available, the Alignment element will be empty. The method returns no error in that case.

To receive alignment information, specify `includeAlignment=true` on the query string.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The answer lies in machine translation.'}]"
```

The response is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

The alignment information starts with `0:2-0:1`, which means that the first three characters in the source text (`The`) map to the first two characters in the translated text (`La`).

#### Limitations
Obtaining alignment information is an experimental feature that we have enabled for prototyping research and experiences with potential phrase mappings. We may choose to stop supporting this in the future. Here are some of the notable restrictions where alignments are not supported:

* Alignment is not available for text in HTML format i.e., textType=html
* Alignment is only returned for a subset of the language pairs:
  - English to/from any other language except Chinese Traditional, Cantonese (Traditional) or Serbian (Cyrillic).
  - from Japanese to Korean or from Korean to Japanese.
  - from Japanese to Chinese Simplified and Chinese Simplified to Japanese.
  - from Chinese Simplified to Chinese Traditional and Chinese Traditional to Chinese Simplified.
* You will not receive alignment if the sentence is a canned translation. Example of a canned translation is "This is a test", "I love you" and other high frequency sentences.
* Alignment is not available when you apply any of the approaches to prevent translation as described [here](../prevent-translation.md)

### Obtain sentence boundaries

To receive information about sentence length in the source text and translated text, specify `includeSentenceLength=true` on the query string.

```curl
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

The response is:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### Translate with dynamic dictionary

If you already know the translation you want to apply to a word or a phrase, you can supply it as markup within the request. The dynamic dictionary is only safe for proper nouns such as personal names and product names.

The markup to supply uses the following syntax.

```
<mstrans:dictionary translation="translation of phrase">phrase</mstrans:dictionary>
```

For example, consider the English sentence "The word wordomatic is a dictionary entry." To preserve the word _wordomatic_ in the translation, send the request:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json; charset=UTF-8" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

The result is:

```
[
    {
        "translations":[
            {"text":"Das Wort \"wordomatic\" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

This feature works the same way with `textType=text` or with `textType=html`. The feature should be used sparingly. The appropriate and far better way of customizing translation is by using Custom Translator. Custom Translator makes full use of context and statistical probabilities. If you have or can afford to create training data that shows your work or phrase in context, you get much better results. [Learn more about Custom Translator](../customization.md).