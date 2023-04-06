<h3>Get Association Records</h3>

Our API enables efficient analysis of two entities' associations over time through robust search functionality and an OpenSearch-like query search option. Additionally, we offer powerful aggregation capabilities, allowing users to perform complex data analysis on their search results.

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/relation/{entityID}/{relatedEntityID}/history><br><br>

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
* **entityID** (_string_): The ID of one entity for which you want to obtain the relation records.
* **relatedEntityID**(_string_): The ID of the other entity that is a member of the relation.

To perform a history search, use a **POST**</span> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/relation/ip-5a9c7d3523825175f04352a12d73c5750fbc82a41cd0c199f84602af5b43281c/threat-427598fdfe0aacdd2c6f51af110a5ff38b06d5db9e422bc22e7a9b5dac06c5c2/history' \
  -H 'accept: application/json'
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Retrieving a Specified Page of Results and Aggregations in the Response</h5>

When a search query is executed in the API, the response object contains a list of hits that correspond to the entity records that matched the search criteria, along with the corresponding aggregation.  

The following fields are included in the response:

* **pages** (_string_):  The total number of pages in the result set.<br>
* **items** (_string_) The total number of entities in the result set.
* **result** (_entity list_):  The search results will provide a list of hits that correspond to the records where the two entities were reported as associated.
    * **@timestamp** (_string_): A timestamp in the **ISO 8601** format representing the date of record creation. _e.g.:"2023-03-30T15:27:50.824681449Z"_,
    *  **entityID**: This response parameter contains the same value as the corresponding request parameter.
    *  **relatedEntityID**: This response parameter contains the same value as the corresponding request parameter.
    *  **id"**: ID of the record. _e.g.:"09f55e9e-bc5a-4967-9c66-510f0ab894f9"_,
    *  **mode**: The type of relation of this two entities._e.g.:"association"_
    *  **userID**: The user who inserted this record _e.g.: "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"_


For Example:
```json
{
  "pages": 1,
  "items": 4,
  "results": [
    {
      "@timestamp": "2023-03-30T15:27:50.83519765Z",
      "entityID": "ip-5a9c7d3523825175f04352a12d73c5750fbc82a41cd0c199f84602af5b43281c",
      "id": "6d4b3315-a102-4399-9374-c364469b8c07",
      "mode": "association",
      "relatedEntityID": "threat-427598fdfe0aacdd2c6f51af110a5ff38b06d5db9e422bc22e7a9b5dac06c5c2",
      "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
    },
    {
      "@timestamp": "2023-03-30T15:27:50.824681449Z",
      "entityID": "ip-5a9c7d3523825175f04352a12d73c5750fbc82a41cd0c199f84602af5b43281c",
      "id": "09f55e9e-bc5a-4967-9c66-510f0ab894f9",
      "mode": "association",
      "relatedEntityID": "threat-427598fdfe0aacdd2c6f51af110a5ff38b06d5db9e422bc22e7a9b5dac06c5c2",
      "userID": "2e4ad29c-bf32-47b0-ae0d-67ec870b1677"
    },...]
}
```

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