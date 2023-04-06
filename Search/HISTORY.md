<h3>Get Entities Records History</h3>
This software interface offers robust search functionality for entity report history by providing an OpenSearch-like query search option. This enables you to easily and effectively search through a vast number of reports. Furthermore, it provides aggregation features that enable you to conduct intricate data analysis on your search results.

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entities/history><br><br>

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
    "most-active-malwares": {
      "terms": {
        "field": "attributes.malware.keyword",
        "size": 50
      }
    },
    "malware_per_minute": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "minute"
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
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-5m",
              "lte": "now"
            }}}
      ]}}}
    ```

In the previous example, we obtained a list of reports for malware-type entities within the last five minutes. We also retrieved a histogram displaying the frequency of reports in one-minute intervals, as well as a ranking of the most active malware and the corresponding number of reports for each.

We get a response like:
```json
{
  "pages": 264,
  "items": 2632,
  "results": [
    {
      "@timestamp": "2023-03-30T14:37:39.806070224Z",
      "attributes": {
        "malware": "pdf dropper agent",
        "malware-family": "pdf",
        "malware-type": "dropper"
      },
      "entityID": "malware-20eae8ae8ec23315a7d9f07c0cbcd3651657b8604ef02e9dfcbfd6304cb824b8",
      "id": "f87b13f3-cf04-4d6a-83b2-90c477e1e6e7",
      "reputation": -3,
      "type": "malware",
      "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
    },...],
    ],
  "aggregations": {
    "malware_per_minute": {
      "buckets": [
        {
          "doc_count": 111,
          "key": 1680186720000,
          "key_as_string": "2023-03-30T14:32:00.000Z"
        },...]},
    "most-active-malwares": {
      "buckets": [
        {
          "doc_count": 2592,
          "key": "pdf dropper agent"
        },..]}}}   
```
If you have not worked with Opensearch or elastic search before you can go to <a>Search and Agregations Page</a> to learn more about performing advanced searches, if you did, you can do a fast review about the queries we have available.

To perform a history search, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entities/history' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"aggs": {
    "most-active-malwares": {
      "terms": {
        "field": "attributes.malware.keyword",
        "size": 50
      }
    },
    "malware_per_minute": {
      "date_histogram": {
        "field": "@timestamp",
        "interval": "minute"
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
        },
        {
          "range": {
            "@timestamp": {
              "gte": "now-5m",
              "lte": "now"
            }
          }
        }
      ]
    }
  }
}'
'
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Retrieving a Specified Page of Results and Aggregations in the Response</h5>

When a search query is executed in the API, the response object contains a list of hits that correspond to the entity records that matched the search criteria, along with the corresponding aggregation.  

The following fields are included in the response:

* **pages** (_string_):  The total number of pages in the result set.<br>
* **items** (_string_) The total number of entities in the result set.
* **result** (_entity list_):  A list of hits that correspond to the records that matched the search criteria.
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
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
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
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.