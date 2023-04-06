<h3>Get Entities (Advanced)</h3>
This API endpoint provides powerful search capabilities using an OpenSearch-like query search option, allowing you to quickly and efficiently search through a large number of entities. Additionally, we offer aggregation capabilities, allowing you to perform complex data analysis on your search results.

With our API, you can easily specify search parameters such as the search terms, filters, and sort criteria to refine your search results. You can also specify aggregation criteria, including min, max, sum, average, and many others, to group and analyze your search results based on specific fields or terms.


**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entity><br><br>

> <h4> Parameters</h4>
  
* **Autentication**: Authentication can be via Authorization header or Key-Pair. See the [Authentication page](#) for more details.<br><br> 
  
   * **Authorization** (_string_): The authorization header of the session if it is your authentication method. _e.g.:_

    ```
    fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    ```

  * **api-key** (_string_): It needs to be combined with the api-secret. You can get it from the [Key Pair Endpoints](#).

  * **api-secret** (_string_): It needs to be combined with the api-key. You can get it from the [Key Pair Endpoints](#).
 * **limit** (_int_): This parameter specifies the maximum number of results you wish to retrieve per page. (_Default is 10_)
  
* **page**(_int_): This parameter specifies the page of results you want to retrieve, where each page contains a specified number of objects based on the value of the "limit" parameter. (_Default is 0_)

* **sort**(_string_): The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field. _e.g.: reputation_ <br>
* **order**(_"asc" or "desc"_): The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order.(_Default is asc_)

* <h3>Message:</h3>
    The body parameter that is going to contain the OpenSearch-like query search and aggregations.<br><br>

    For Example:

    ```json
    {
  "aggs": {
    "top-malware-types": {
      "terms": {
        "field": "attributes.malware-type.keyword",
        "size": 50
      }
    },
    "accuracy-stats": {
      "stats": {
        "field": "accuracy"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }}}]}}}
    ```

In the previous example we are filtering all malware-type entities and obtaining the number of registered malware by type.

We get a response like:
```json
{
 "pages": 48,
  "items": 475,
  "results": [
    {
      "@timestamp": "2023-03-28T09:05:09.669101722Z",
      "accuracy": 2,
      "attributes": {
        "malware": "win trojan sdbot",
        "malware-family": "win",
        "malware-type": "trojan"
      },
      "id": "malware-a208c4b33a1763284ce9a4584efa296372082ba82ee174597092f972767de163",
      "reputation": -3,
      "type": "malware"
    },
    ...
    ],
      "aggregations": {
    "accuracy-stats": {
      "avg": 1.9873684210526317,
      "count": 475,
      "max": 3,
      "min": 1,
      "sum": 944
    },
    "top-malware-types": {
      "buckets": [
        {
          "doc_count": 348,
          "key": "trojan"
        },
    ...
      ],
       "doc_count_error_upper_bound": 0,
      "sum_other_doc_count": 0
    }
  }
}
```
If you have not worked with Opensearch or elastic search before you can go to <a id="./Search_and_Agg/QUERY.md" >Search</a> and <a id="./Search_and_Agg/AGGREGATIONS.md">Agregations Page</a> to learn more about performing advanced searches, if you did, you can do a fast review about the queries we have available.


To perform a search, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities?sort=reputation' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "aggs": {
    "top-malware-types": {
      "terms": {
        "field": "attributes.malware-type.keyword",
        "size": 50
      }
    },
    "accuracy-stats": {
      "stats": {
        "field": "accuracy"
      }
    }
  },
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "type": {
              "value": "malware"
            }
          }
        }
      ]
    }
  }
}
'
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Retrieving a Specified Page of Results and Aggregations in the Response</h5>

When a search query is executed in the API, the response object contains a list of hits that correspond to the entities that matched the search criteria, along with the corresponding aggregation. 

The following fields are included in the response:

* **pages** (_string_):  The total number of pages in the result set.<br>
* **items** (_string_) The total number of entities in the result set.
* **result** (_entity list_):  A list of hits that correspond to the entities that matched the search criteria.
  * **aggregations**(_JSON_):  A list of metrics based on the criteria specified in the search request.


><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "60791adb-e0f3-4df0-a45a-02a1718d75f5",
  "error": "... error: {\"error\":{\"root_cause\":[{\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"}],\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"},\"status\":400}"
}
```
> <h5> Code 401 - Authentication required
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request. See <a> Authentication API Page</a> for more details.

For example:

```json
{
  "uuid": "4b2561eb-2b4b-43ee-a272-643df36e2c5b",
  "error": "authentication required"
}
```

> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if you do not have enough permissions to make this operation, it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

For example:
```json
{
  "uuid": "75827622-bb05-46b5-80a9-344a6df0e001",
  "error": "unauthorized"
}
```


> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error, occurs when the search could not find a match for the given parameters.

For example:

```json
{
  "uuid": "e23611e9-2974-4cd7-a08b-47fe7d35fa9a",
  "error": "entity not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.