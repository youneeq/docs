---
title: Youneeq API Reference

language_tabs:
  - javascript: JavaScript

toc_footers:
  - <a target="_blank" href='http://www.youneeq.ca'>What is Youneeq?</a>
  - <a target="_blank" href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

<aside class="success">We're in the process of migrating our documentation here. In the meantime, you can find our
latest API guide <a target="_blank" href="http://www.youneeq.ca/wp-content/uploads/2016/10/Youneeq-Integration-Setup-Guide-Mar-23-2016.pdf">here</a>.
</aside>

# Get Started

Youneeq provides a recommendation service to online publishers that targets content at individual users. To find out
more, check out our <a target="_blank" href="http://www.youneeq.ca/">website</a>.

On this page you will find documentation regarding how to integrate Youneeq services with your site.

## Recommendations

```javascript

jQuery(document).ready(function ($) {

	// This function handles displaying recommendations after the request has returned a response.
	function on_yq_suggest(response)
	{
		if (response && response.suggest && response.suggest.node)
		{
			recommendations = response.suggest.node;
			var html = "<li>";

			// Iterate through each recommended story.
			for (var i = 0; i < recommendations.length; i++)
			{
				// Access the properties of this recommendation.
				var title = recommendations[i].title;
				var url = recommendations[i].url;

				// Build HTML to display link to this recommendation.
				html += "<li><a href='" + url + "'>" + title + "</a></li>";
			}
			html += "</li>";

			// Populate some element with the HTML to display the recommendations.
			$("#youneeq-div").append(html);
		}
	}

	// This function sends a request to Youneeq that indexes the current page (article) and requests recommendations.
	function my_yq_init()
	{
		if ($('meta[property="og:type"]').attr('content') === "article") {
			Yq.observe(
				{
					'content_id': content_id,
					'observe': [{
								'type': 'node',
								'title': title,
								'create_date': create_date,
								'description': description,
								'image': image_url,
								'categories': categories,
					}],
					'suggest': [{
							'type': 'node',
							'count': 6,
					}]
				},
				on_yq_suggest
			);
		}
	}

	Yq.onready(my_yq_init);
}
```

A "Recommendation" request typically does the following:

* Records a page hit.
* Indexes the current page, if it is an article page, so that we have the information we need to use it within recommendations.
* Requests recommendations from the Youneeq recommendation service.

### 1. Include Youneeq Scripts
The following scripts are required:

* jquery.js
* json2.js
* detect_timezone.js
* yqmin.js

This can be done with the following code:

`<script type="text/javascript" src="http://cdn.youneeq.ca/yq-static-scripts.js"></script>`<br>
`<script type="text/javascript" src="http://api.youneeq.ca/app/yqmin"></script>`<br>

Note: yq-static-scripts.js contains jquery.js, json2.js and detect_timezone.js.

### 2. Integration Code

### 3. Activate with Youneeq Staff


## Click Tracking



# Complete Reference

We will continue to support ***Deprecated*** parameters, but discourage their use because a better alternative exists.

## Recommendations

### Request JSON Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
bof_profile | string | ✔ | The user’s ID. Not strictly required, but highly recommended. If not supplied, a session ID will be generated server-side in replacement. This is set automatically by yq.js.
content_id | string | ✔ | The content ID of the current page. If the current page is non-content, this can either be omitted or set to null (ok, so it's not *strictly* required).
href | string | ✔ | URL of the current page. This is set automatically by yq.js.
alt_href | string | | This will override 'href'. Provides a way to manually set the URL of the current page.
content_type | string | | Observed content is handled differently depending on this. If not supplied, defaults to regular articles ("content"). Supported options: ["content", "classified", "sponsored"].
report_domain | string | | Reporting data will be recorded under this domain. If not supplied, the domain parsed from 'href' or 'alt_href' will be used. This should probably only be used if you're trying to override 'href' or 'alt_href'. Should be in the form "mydomain.com".
gigya | object | | Object: [Gigya](#gigya-object-parameters).
observe | array[object] | | Object: [Observe](#observe-object-parameters). Only the first object in the array is used. This object is required to index the current article.
page_hit | object | | Object: [Page Hit](#page-hit-object-parameters). This object is required to record a page hit for reporting purposes.
suggest | array[object] | | Object: [Suggest](#suggest-object-parameters). Only the first object in the array is used. This object is required to request recommendations.
| | |
*create_date* | *string* | | ***Deprecated:*** *Use the [Observe](#observe-object-parameters)'s 'create_date' instead.*
*image* | *string* | | ***Deprecated:*** *Use the [Observe](#observe-object-parameters)'s 'image' instead.*
*title* | *string* | | ***Deprecated:*** *Use the [Observe](#observe-object-parameters)'s 'title' instead.*


### Observe object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
type | string | ✔ | Always set this to "node".
categories | array[string]<br>string<br>array[array[string]] | | The article's categories. The following formats are supported: <ul><li>["sports", "news"]</li><li>"sports, news"</li><li>[["sports"], ["news"]]</li><li>[["basketball", "sports"], ["local", "news"]] – Pairs only</li></ul>
classified_data | object | | Object: [Classified](#classified-data-parameters).
create_date | string | | The publish date (and preferably time) of the article. The format must be supported by [.NET's DateTime.TryParse](https://msdn.microsoft.com/en-us/library/ch92fbc1(v=vs.110).aspx).
description | string | | The article's description.
image | string | | URL of the article's primary image.
is_content | boolean | | Article will only be indexed if this is true. If not supplied, defaults to true.
tags | array[string] | | The article's tags.
title | string | | The article's title.
| | |
expiry_date | string | | ***Deprecated:*** We accept this parameter, but it is never used.
name | string | | ***Deprecated:*** Use 'content_id' at the [top level](#request-json-parameters) instead.

### Page Hit object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
referrer | string | | The HTTP Referrer. This is set automatically by yq.js, which assigns the value of document.referrer.
tz_name | string | | The user's timezone name. This is set automatically by yq.js, which assigns the value of timezone.olson_tz;
tz_off | string | | The user's timezone offset. This is set automatically by yq.js, which assigns the value of timezone.utc_offset.
| | |
bof_profile | string | | ***Deprecated:*** Use 'bof_profile' at the [top level](#request-json-parameters) instead.
href | string | | ***Deprecated:*** Use 'href' at the [top level](#request-json-parameters) instead. This is set automatically by yq.js, but will be overridden by the top level href, if supplied.
name | string | | ***Deprecated:*** Use 'content_id' at the [top level](#request-json-parameters) instead.

### Suggest object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
count | integer | ✔ | Number of recommendations to return.
type | string | ✔ | Always set this to "node".
categories | array[string] | | If 'strict_categories' is set to true in [Suggest Options](#suggest-options-object-parameters), this list of categories will be used to restrict the content returned in the recommendations.
date_start | string | | Oldest publish date of content to include in recommendations.
date_end | string | | Newest publish date of content to include in recommendations.
domains | array[string] | | List of domains to draw content from for recommendations. By default, only content from the current domain is used.
exclusions | array[object] | | Object: [Exclusions](#exclusions-object-parameters). List(s) of content to exclude from recommendations.
is_domain_included | boolean | | If false, the current domain will not be included in the list of domains to draw content from.
is_panel_builder | boolean | | If true, recommendations will contain content ID, title and URL. Overridden by 'panel_custom' and 'is_panel_detailed'.
is_panel_detailed | boolean | | If true, recommendations will contain content ID, title, URL, image URL and description. Overridden by 'panel_custom'.
isAllClientDomains | boolean | | If true, content will be drawn for recommendations from all domains (including the current domain) belonging to the client. Overridden by 'domains'.
isUrlReturned | boolean | | If true, recommendations will contain URL only. Overridden by 'panel_custom', 'is_panel_detailed' and 'is_panel_builder'.
options | object | | Object: [Suggest Options](#suggest-options-object-parameters). Additional options for suggest request.
panel_custom | | | Object: [Panel Custom](#panel-custom-object-parameters). Use to customize the properties returned for recommendations (i.e. content ID, title, URL, etc.).
panel_type | string | | Identifies the type/location of panel that the recommendations will be served in. Supported options: ["home_panel", "category_panel", "article_panel", "article_scroll"].
sponsored_count | integer | | Number of sponsored content recommendations to return. This is a subset of the 'count', so if 'count' is 6 and 'sponsored_count' is 2, it will return 4 "regular" recommendations and 2 sponsored content recommendations.
variant | string | | Specifies which recommender variant will generate recommendations. Defaults to YQ recommendations. Supported options: ["yq", "trending", "mostread"].
| | |
name | string | | ***Deprecated:*** Use 'content_id' at the [top level](#request-json-parameters) instead.

### Gigya object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
UID | string | ✔ |

### Classified Data object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
type | | |
price | | |
year | | |
is_rental | | |
make | | |
model | | |
condition | | |
vehicle_class | | |
address | | |
city | | |
state | | |
country | | |
zip_code | | |
property_type | | |
lot_size | number | | The lot size, in square-feet.
floor_size | number | | The floor size, in square-feet.
bedrooms | integer | | The number of bedrooms.
full_bathrooms | integer | | The number of full bathrooms.
half_bathrooms | integer | | The number of half bathrooms.
job_type | | |

### Exclusions object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
content_ids | array[string] | ✔ | The list of content IDs to exclude from recommendations.
domain | string | ✔ | The domain that the content IDs belong to (all content IDs in this object have to be from the same domain).

### Suggest Options object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
disable_history | boolean | | If true, excludes the articles viewed by the user in the last 30 days instead of the default last 10 days.
paging_enabled | boolean | | If true, enables the paging mechanism that allows for multiple suggest requests to be made from the same page without duplicating recommendations between them.
show_history | boolean | | By default and if false, articles that the user has already viewed are filtered out of recommendations. If this is true, the user's history will not be filtered out.
strict_categories | boolean | | If true, content will be restricted to categories specified by 'categories' in [Suggest](#suggest-object-parameters).

### Panel Custom object Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
categories | boolean | | If true, the categories for each article in the recommendations is returned in the results.
date | boolean | | If true, the publish date of each article in the recommendations is returned in the results.
description | boolean | | If true, the description for each article in the recommendations is returned in the results.
domain | boolean | | If true, the domain of each article in the recommendations is returned in the results.
domain_name | boolean | | If true, the friendly domain name for each article in the recommendations is returned in the results.
image | boolean | | If true, the image URL for each article in the recommendations is returned in the results.
like | boolean | |
read | boolean | | If true, a boolean indicating if the user has read that article is returned in the results.
title | boolean | | If true, the title of each article in the recommendations is returned in the results.
url | boolean | | If true, the URL of each article in the recommendations is returned in the results.
| | |
event | boolean | | ***Deprecated:*** Has no effect.

## Click Tracking




# Search

Returns personalized search results.

## Search Request

Request search results for the given domain and search terms.


```javascript
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
endDate | string | | The most recent publish date of articles to include in the search results. The format must be supported by [.NET's DateTime.TryParse](https://msdn.microsoft.com/en-us/library/ch92fbc1(v=vs.110).aspx).
maxArticleAge | integer | | The maximum age (in days) of articles that should be included in the search results.
personalized | boolean | | If true, search results will be personalized for the user.
pageNumber | integer | |
startDate | string | | The oldest publish date of articles to include in the search results. The format must be supported by [.NET's DateTime.TryParse](https://msdn.microsoft.com/en-us/library/ch92fbc1(v=vs.110).aspx).
killPromoteInfo | boolean | | Whether to include the kill/promote status of the individual search results. Defaults to false. Currently dependent on contentInfo, i.e. if contentInfo is false, this will always be false.

### Sample Request
`GET http://search.youneeq.ca/api/search?callback=jQuery17109329779187683022_1454095328422&json={"domain":"mydomain.com","search":"zika virus","userId":"2vcwxogpr4oxvbotfbdbrl0o","contentInfo":"false"}`

## Search Response

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
`jQuery17109329779187683022_1454095328422({"numResults":2,"stories":[{"domain":"mydomain.com","contentId":"20160823268","score":17.077688,"read":false,"solrScore":15.168544},{"domain":"mydomain.com","contentId":"20160801170","score":13.230506,"read":false,"solrScore":11.460202}]})`


## Event Request

Record a search-related event, such as clicking on a search result.

### HTTP Request
`GET http://search.youneeq.ca/api/event?callback=<callback>&json=<json>`

### Query Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
callback | string | ✔ | The JQuery callback ID. This should be added automatically via a JQuery Ajax request.
json | json | ✔ | See JSON Parameters.

### JSON Parameters
Parameter | Type | Required | Description
--------- | ---- | :--------: | -----------
contentId | string | ✔ | The content ID of the article that was clicked.
domain | string | ✔ | The domain of the site where the event occurred.
eventType | string | ✔ | The type of event to record. Currently, "click" is the only supported event type.
userId | string | ✔ | The user's ID. Known as bof_profile in the recommendation service.
contentDomain | string |  | ***Not implemented yet:*** The domain of the article that was clicked. If not specified, we assume that the clicked article is from the same domain ('domain' parameter) where the event occurred.

### Sample Request
 `GET http://search.youneeq.ca/api/event?callback=jQuery17109329779187683022_1454095328422&json={"contentId":"20160923195","domain":"mydomain.com","eventType":"click","userId":"2vcwxogpr4oxvbotfbdbrl0o"}&_=1454095330136`

## Event Response

The response just returns true if the request was successful, false otherwise.

### Sample Response
`jQuery17109329779187683022_1454095328422(true)`
