<h3 id="createSession">Get Comments</h3>
This API endpoint is designed to retrieve the comments associated with a specific entity.
<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/comments/{id}>

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

* **sort**(_string_): The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field. _e.g.: NEED EXAMPLE_ <br>
* **order**(_"asc" or "desc"_): The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order.(_Default is asc_)

* **id**(_string_): The "id" parameter is used to specify the ID of the entity for which you want to retrieve comments through this API endpoint.



To get a comment, use a **GET**</span> request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/comments/malware-a208c4b33a1763284ce9a4584efa296372082ba82ee174597092f972767de163' \
  -H 'accept: application/json'
}'
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Return a list of comments as Response</h5>

This API endpoint returns a list of comments that are associated with the specified entity.

>Fields of the response:

* **pages** (_string_):  The total number of pages in the result set.<br>
* **items** (_string_) The total number of comments in the result set.
* **result** (_entity list_):  A list of hits that correspond to the comments that are associated with the specified entity.

_e.g.:_
```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
    "commentID": "NEED A REAL EXAMPLE"
    "comment": "Test",
    "entityID": "malware-a208c4b33a1763284ce9a4584efa296372082ba82ee174597092f972767de163"
}
  ]
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
  "uuid": "cf096e86-347f-4ddf-af1c-0c673a004211",
  "error": "value cannot be empty"
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