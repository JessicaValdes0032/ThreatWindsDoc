<h3 id="entityTypeValue">Get Entity by type and value</h3>
This API endpoint retrieves the last entity that matches the given type and value. It's useful for fast searching when you need to determine if an entity exists or not.
<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entity>

> <h4> Parameters</h4>
  
* **Autentication**: Authentication can be via Authorization header or Key-Pair. See the [Authentication page](#) for more details.<br><br> 
  
   * **Authorization** (_string_): The authorization header of the session if it is your authentication method. _e.g.:_

    ```
    fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    ```

  * **api-key** (_string_): It needs to be combined with the api-secret. You can get it from the [Key Pair Endpoints](#).

  * **api-secret** (_string_): It needs to be combined with the api-key. You can get it from the [Key Pair Endpoints](#).

* **Message** (_JSON_):

  * **type** (_string_): The type of entity that you are looking for. _e.g.: ip, malware, file, md5_. (To see all the types of entities, check the [Definition endpoint](#)).

  * **value** (_string_): The value of the entity that you are looking for. _e.g.:_ "138.219.198.146", "Doc Dropper Agent", "a7ffc6f8bf1ed76651c14756a061d662f580ff4de43b49fa82d80a4b80f8434a", "161f923323b9c24f8ffc4db9b2f0d4d3".



To create a session, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entity' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "type": "malware",
  "value": "Doc Dropper Agent"
}'
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Return an Entity as Response</h5>

The API returns this code when an entity is found successfully. It returns the entire entity.

>Fields of the response:

* **id** (_string_): The ID of the returned entity.<br>
* **type** (_string_) The type of the entity.
* **reputation** (_int_): A number that represents the reputation of the entity on a scale of -3 to 3. The values are:
  
> 3 excellent 
2 good
1 normal 
0 neutral
-1 poor 
-2 bad 
-3 extremely bad

* **accuracy** (_string_): The accuracy of the match.<br>

* **attributes** (_JSON_):  A list of attributes of the entity. It depends on the type of entity. To see the list of attributes, check the <a href="./AUTENTICATIONAPI.md">Definnition Page</a>.<br>

_e.g.:_
```json
{
  "@timestamp": "2023-03-28T14:54:04.03077028Z",
  "accuracy": 3,
  "attributes": {
    "malware": "pdf dropper agent",
    "malware-family": "pdf",
    "malware-type": "dropper"
  },
  "id": "malware-20eae8ae8ec23315a7d9f07c0cbcd3651657b8604ef02e9dfcbfd6304cb824b8",
  "reputation": -3,
  "type": "malware"
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

<h3 id="entityById">Get Entity by id</h3>
This API endpoint retrieves the last entity that matches the given type and value. It's useful for fast searching when you need to determine if an entity exists or not.
<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/search/v1/entity/{id}>

> <h4> Parameters</h4>
  
* **Autentication**: Authentication can be via Authorization header or Key-Pair. See the [Authentication page](#) for more details.<br><br> 
  
   * **Authorization** (_string_): The authorization header of the session if it is your authentication method. _e.g.:_

    ```
    fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
    ```

  * **api-key** (_string_): It needs to be combined with the api-secret. You can get it from the [Key Pair Endpoints](#).

  * **api-secret** (_string_): It needs to be combined with the api-key. You can get it from the [Key Pair Endpoints](#).

* **entityID** (_string_): The ID of the entity for which you want to retrieve information.



To create a session, use a **GET**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/search/v1/entity/malware-784136848fb31fe5fadf1f5ed8af40737720be2a85124583c18495d6bcf188e3' \
  -H 'accept: application/json' \
  -H 'api-key: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' \
  -H 'api-secret: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB' 
```

><h4>Returns</h4>

> <h5>Code 200</h5>

<h5>Return an Entity as Response</h5>

The API returns this code when an entity is found successfully. It returns the entire entity.

>Fields of the response:

* **id** (_string_): The ID of the returned entity.<br>
* **type** (_string_) The type of the entity.
* **reputation** (_int_): A number that represents the reputation of the entity on a scale of -3 to 3. The values are:
  
> 3 excellent 
2 good
1 normal 
0 neutral
-1 poor 
-2 bad 
-3 extremely bad

* **accuracy** (_string_): The accuracy of the match.<br>

* **attributes** (_JSON_):  A list of attributes of the entity. It depends on the type of entity. To see the list of attributes, check the <a href="./AUTENTICATIONAPI.md">Definnition Page</a>.<br>

_e.g.:_
```json
{
  "@timestamp": "2023-03-28T14:54:04.03077028Z",
  "accuracy": 3,
  "attributes": {
    "malware": "pdf dropper agent",
    "malware-family": "pdf",
    "malware-type": "dropper"
  },
  "id": "malware-20eae8ae8ec23315a7d9f07c0cbcd3651657b8604ef02e9dfcbfd6304cb824b8",
  "reputation": -3,
  "type": "malware"
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
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.