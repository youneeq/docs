---
title: Youneeq API Reference

language_tabs:
  - Javascript

toc_footers:
  - <a target="_blank" href='http://www.youneeq.ca'>What is Youneeq?</a>
  - <a target="_blank" href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# API Reference

Something something. Dun dun na.

# Search

```Javascript
var searchUrl = "http://search.youneeq.ca/api/search?callback=?";

var searchArgs = {
    domain: "mydomain.com",
    search: "zika virus",
    userId: "2vcwxogpr4oxvbotfbdbrl0o"
}

var jsonArgs = {
    'json': JSON.stringify(searchArgs)
};

var ajaxArgs = {
    url: searchUrl,
    crossDomain: true,
    dataType: 'jsonp',
    data: jsonArgs
};

$.ajax(ajaxArgs).done(function(response) {
	// Do whatever you need to with the response.
}
```

Returns personalized search results.

### HTTP Request
`GET http://search.youneeq.ca/api/search?callback=<callback>&json=<json>`

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
callback | string | ✔ | The JQuery callback ID. This should be added automatically via a JQuery Ajax request.
json | json | ✔ | See JSON Parameters.

### JSON Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
domain | string | ✔ | The domain to get search results from and use user history from to personalize those results.
search | string | ✔ | The search string inputted by the user. Can include multiple words, spaces, etc. Will be parsed server-side.
userId | string | | The user's ID. Known as bof_profile in the recommendation service. If not supplied, search results will not be personalized.
contentInfo | boolean | | Whether to include the content information (title, description, url, image) with the search results. Defaults to true.
maxArticleAge | integer | | The maximum age (in days) of articles that should be included in the search results.
pageNumber | integer | |

### Sample HTTP Request
`GET http://search.youneeq.ca/api/search?callback=jQuery17109329779187683022_1454095328422&json={"domain":"mydomain.com","search":"zika virus","userId":"2vcwxogpr4oxvbotfbdbrl0o","contentInfo":"false"}`

### Response

Parameter | Type | Description
--------- | ---- | -----------
stories | array | An array of objects, where each object represents one article in the search results. The array is sorted by score, with the articles at the beginning of the array being more relevant than those at the end of the array.
numResults | integer |


Parameter | Type | Description
--------- | ---- | -----------
domain | string |
contentId | string |
score | float |
solrScore | float |
read | boolean |
title | string |
desciption | string |
publishDate | string |
url | string |
imageUrl | string |


### Sample Response
`jQuery17109329779187683022_1454095328422({"stories":[{"domain":"mydomain.com","contentId":"20160823268","score":17.077688,"read":false,"solrScore":15.168544},{"domain":"mydomain.com","contentId":"20160801170","score":13.230506,"read":false,"solrScore":11.460202}],"numResults":2})`
