# Get Associations by id

Get entity associations API endpoint retrieves all the entities associated with a given entity based on their relationship. This API can provide valuable information for identifying relationships and patterns between entities, such as malware, domains, IP addresses, etc.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/search/v1/associations/{id}](https://intelligence.threatwinds.com/api/search/v1/associations/%7Bid%7D)

> **Parameters**

*   **Autentication**: Authentication can be via Authorization header or Key-Pair. See the [Authentication page](association.md) for more details.\
    \


    * **Authorization** (_string_): The authorization header of the session if it is your authentication method. _e.g.:_

    ```
    fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    ```

    * **api-key** (_string_): It needs to be combined with the api-secret. You can get it from the [Key Pair Endpoints](association.md).
    * **api-secret** (_string_): It needs to be combined with the api-key. You can get it from the [Key Pair Endpoints](association.md).
* **limit** (_int_): This parameter specifies the maximum number of results you wish to retrieve per page. (_Default is 10_)
* **page**(_int_): This parameter specifies the page of results you want to retrieve, where each page contains a specified number of objects based on the value of the "limit" parameter. (_Default is 0_)
* **sort**(_string_): The sort parameter is a query parameter that allows you to sort the results of your search based on a specific field. _e.g.: reputation_\

* **order**(_"asc" or "desc"_): The order parameter specifies the order in which the results should be sorted based on the specified "sort" parameter. It can take two values: "asc" for ascending order and "desc" for descending order.(_Default is asc_)
* **entityID** (_string_): The ID of the entity for which you want to retrieve the associations.

To create a session, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/search/v1/associations/domain-59f5e994104053f590acdd1d2d72071669c839a60fc68291110facfa2fd16396' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' 
```

> **Returns**

> **Code 200**

### Return a list of Entities as Response

It returns a list of associated entities or an empty list if no match is found.

> Fields of the response:

* **id** (_string_): The ID of the returned entity.\

* **type** (_string_) The type of the entity.
* **reputation** (_int_): A number that represents the reputation of the entity on a scale of -3 to 3. The values are:

> 3 excellent  2 good 1 normal  0 neutral -1 poor  -2 bad  -3 extremely bad

* **accuracy** (_string_): The accuracy of the match.\

* **attributes** (_JSON_): A list of attributes of the entity. It depends on the type of entity. To see the list of attributes, check the [Definnition Page](../Search/AUTENTICATIONAPI.md).\


_e.g.:_

```json
{
  "pages": 1,
  "items": 1,
  "results": [
    {
      "@timestamp": "2023-04-04T21:02:42.717161656Z",
      "accuracy": 1,
      "attributes": {
        "threat": "expansion on 123@123.com"
      },
      "id": "threat-d8257cec79ed8b20c32decd305c1692391c185ab13d52b3495ea486df5385b27",
      "reputation": -2,
      "type": "threat"
    }
  ]
}
```

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "cf096e86-347f-4ddf-af1c-0c673a004211",
  "error": "value cannot be empty"
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
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not session related.
