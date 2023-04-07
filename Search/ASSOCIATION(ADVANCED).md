# Associations (Advanced)

This API endpoint offers advanced search capabilities for entity associations, utilizing an OpenSearch-like query search feature that enables fast and efficient searching of a vast number of entities. Additionally, it provides aggregation capabilities that allow you to conduct intricate data analysis on your search results, enabling you to gain deep insights into entity associations.

**EndPoint:** [https://intelligence.threatwinds.com/api/search/v1/associations/{id}/advanced](https://intelligence.threatwinds.com/api/search/v1/associations/%7Bid%7D/advanced)\
\


> **Parameters**

*   **Autentication**: Authentication can be via Authorization header or Key-Pair. See the [Authentication page](<ASSOCIATION(ADVANCED) copy.md>) for more details.\
    \


    * **Authorization** (_string_): The authorization header of the session if it is your authentication method. _e.g.:_

    ```
    fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    ```

    * **api-key** (_string_): It needs to be combined with the api-secret. You can get it from the [Key Pair Endpoints](<ASSOCIATION(ADVANCED) copy.md>).
    * **api-secret** (_string_): It needs to be combined with the api-key. You can get it from the [Key Pair Endpoints](<ASSOCIATION(ADVANCED) copy.md>).
* **limit** (_int_): This parameter specifies the maximum number of results you wish to retrieve per page. (_Default is 10_)
* **page**(_int_): This parameter specifies the page of results you want to retrieve, where each page contains a specified number of objects based on the value of the "limit" parameter. (_Default is 0_)
* **sort**(_string_): The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field. _e.g.: reputation_\

* **order**(_"asc" or "desc"_): The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order.(_Default is asc_)
* **entityID** (_string_): The ID of the entity for which you want to retrieve the associations.
*   #### Message:

    The body parameter that is going to contain the OpenSearch-like query search and aggregations.\
    \


    For Example:

    ```json
    {
    "aggs": {
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
              "value": "md5"
            }
          }
        }
      ]
    }
    }
    }
    ```

"In the previous example, we filtered all entities with the attribute "md5-type" that are related to a specific entity, and obtained statistics on the accuracy of the results."

We get a response like:

```json
{
  "pages": 20,
  "items": 276,
  "results": [
    {
      "@timestamp": "2023-03-31T13:08:08.220244275Z",
      "accuracy": 1,
      "attributes": {
        "descriptor": "common-file",
        "md5": "2ed56a78d957dcde2db76e830f5f0741"
      },
      "id": "md5-2bef618643db4b0f1b8631b1567abf2262dc4161e94f671c5a9197d3b22a2c52",
      "reputation": -3,
      "type": "md5"
    },...
  ],
  "aggregations": {
    "accuracy-stats": {
      "avg": 1,
      "count": 276,
      "max": 1,
      "min": 1,
      "sum": 256
    }
  }
}
```

If you have not worked with Opensearch or elastic search before you can go to Search and Agregations Page to learn more about performing advanced searches, if you did, you can do a fast review about the queries we have available.

To perform a search, use a **POST** request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/associations/malware-efefabf74562def7d4fba5e384896ef59a08ba45b54c98fb98a3772f96a6d3cb/advanced' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "aggs": {
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
              "value": "md5"
            }
          }
        }
      ]
    }
  }
  }'
```

> **Returns**

> **Code 200**

### Retrieving a Specified Page of Results and Aggregations in the Response

When a search query is executed in the API, the response object contains a list of hits that correspond to the entities associated with the given entity and that matched the search criteria, along with the corresponding aggregations.

The following fields are included in the response:

* **pages** (_string_): The total number of pages in the result set.\

* **items** (_string_) The total number of entities in the result set.
* **result** (_entity list_): A list of hits that correspond to the entities that matched the search criteria.
  * **aggregations**(_JSON_): A list of metrics based on the criteria specified in the search request.

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "60791adb-e0f3-4df0-a45a-02a1718d75f5",
  "error": "... error: {\"error\":{\"root_cause\":[{\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"}],\"type\":\"illegal_argument_exception\",\"reason\":\"query malformed, empty clause found at [1:131]\"},\"status\":400}"
}
```

> **Code 401 - Authentication required**

Authentication required

The 401 error code indicates that you need authentication to do this request. See Authentication API Page for more details.

For example:

```json
{
  "uuid": "4b2561eb-2b4b-43ee-a272-643df36e2c5b",
  "error": "authentication required"
}
```

> **Code 403 - Forbidden**

A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if you do not have enough permissions to make this operation, it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

For example:

```json
{
  "uuid": "75827622-bb05-46b5-80a9-344a6df0e001",
  "error": "unauthorized"
}
```

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error, occurs when the search could not find a match for the given parameters.

For example:

```json
{
  "uuid": "e23611e9-2974-4cd7-a08b-47fe7d35fa9a",
  "error": "entity not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not session related.
