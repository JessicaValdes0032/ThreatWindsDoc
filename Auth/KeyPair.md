<h2>KeyPair</h2>

<nav>
Table of Content:
  <ul>
    <li><a href="#createKeyPair">Create KeyPair</a></li>
    <li><a href="#deleteKeyPair">Delete KeyPair</a></li>
    <li><a href="#getKeyPairs">Get KeyPairs</a></li>
    <li><a href="#verifyKeyPair">Verify KeyPair</a></li>
    <li><a href="#newCodeKeyPair">New Verification Code</a></li>
    <li><a href="#checkKeyPair">Check KeyPair</a></li>
   </ul>
</nav>
<h3 id="createKeyPair">Create KeyPair</h3>
This API endpoint creates an unverified KeyPair.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/keypair>

> <h4> Parameters</h4>
  
* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:
* **Message** (_JSON_):
  * **days**	(_integer_): The 'days' parameter specifies the length of time, in days, that the key pair will remain valid. _e.g.: 365_
  * **name**	(_string_): "The 'name' parameter is used to specify a name or identifier for the key pair. _e.g.: Example_
<br>

To create a KeyPair, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "days": 365,
  "name": "Example"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Return a KeyPair as Response</h5>

We get this code when the KeyPair was created successfully. It returns a KeyPair(apiKey and apiSecret). Remember that you need to verify this keypair before you can use it.

>Fields of the response:

* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.
  
* **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. Ensure to save the apiSecret in a safe place, this will not be shown again.

* **verificationCodeID** (_string_): The verification code id that is used in the <a href="./KeyPair.md">KeyPair Verification Endpoint</a> to ensure that the KeyPair is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_<br>

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when the authorization header parameter provided in the request is not valid.

e.g.:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```
> <h5> Code 401 - Authentication required
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or the session that is being used for authentication no longer being active. It's important to double-check the authorization header before attempting to use it.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="deleteKeyPair">Delete KeyPair</h3>

This API endpoint closes a KeyPair.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/keypair>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **KeyPair ID** (_string_): The id of the KeyPair that you want to delete. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
To delete a KeyPair, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns the message "acknowledged" </h5>

When a KeyPair is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the KeyPair or the session, cannot be found.

There are several reasons why this may occur, such as:
* the session or the keypair has expired.
* the session has not been verified.
*  the keypair does not exist in the system.
<br><br>
  
It's important to double-check the parameters before attempting to delete the KeyPair.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="getKeyPairs">Get KeyPairs</h3>

This API endpoint to get the user KeyPairs.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/keypairs>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to use the authorization header._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To get the currents KeyPairs, use a **GET** request, for example:

```
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypairs' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns a list of KeyPairs</h5>

It returns the list of the current user <a href="./KeyPair.md">KeyPairs</a>.

>Fields of the response:

* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_<br>

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.


><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_<br><br>


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

The 401 error code indicates that you need authentication to do this request.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the session has expired or has not been verified. It's important to double-check the parameters before attempting to make the request.


For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

<h3 id="verifyKeyPair">Verify KeyPair</h3>

This API endpoint verifies the KeyPair using code sent by email.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification>

> <h4> Parameters</h4>

* **verificationCodeID** (_string_): You can obtain it from the KeyPair that you wish to verify at the time of its creation or with the <a href="">New Verification Code</a> endpoint. _e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"_<br>

* **code** (_string_): A code sent to your preferred email when a KeyPair is created. _e.g.: "757564"_<br>
  
To close a KeyPair, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "code": "757564",
  "verificationCodeID": "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns the message "acknowledged" </h5>

When a KeyPair is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the verificationCodeID, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is not longer active.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "52dbba7a-99fd-4e87-999f-d9e1df38b4f4",
  "error": "incorrect authorization header"
}
```
> <h5> Code 401 - Unauthorized</h5>

We get this code when the code sent is incorrect.

For example:

```json
{
  "uuid": "39908d33-7953-440f-895c-69423c6f7c71",
  "error": "verification code not found"
}
```
> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="newCodeKeyPair">New Verification Code</h3>

This API endpoint sends a new verification code.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/keypair/verification>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```
* **Message** (_Json_):
   * **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. You can get it at the time of its creation or when you return the list of yours keyPairs.
  * **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. This is only obtained at the time of its creation.<br><br>

To get a new verification code, use a **POST** request, for example:

```
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/verification' \
  -H 'accept: application/json' \
  -H 'Authorization: Jlore138gST9TnQKRWZZzLW4NfxCo0q8' \
  -H 'Content-Type: application/json' \
  -d '{
  "apiKey": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB",
  "apiSecret": "fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns a verificationCodeID. </h5>

Returns a new verificationCodeID and **a code is sent to your preferred email**. Then you can use both in the <a href="#verifyKeyPair">Verify Endpoint</a> to verify your KeyPair.

>Fields of the response:

* **verificationCodeID** (_string_): The verification code id that is used in the <a href="./KeyPair.md">KeyPair Verification Endpoint</a> to ensure that the KeyPair is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br><br>

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

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

The 401 error code indicates that you need authentication to do this request.

> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the KeyPair, cannot be found.

There are several reasons why this may occur, such as the KeyPair not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

<h3 id="checkKeyPair">Check KeyPair</h3>

This API endpoint check a KeyPair and returns privileges<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/KeyPair>

> <h4> Parameters</h4>

   * **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. You can get it at the time of its creation or when you return the list of yours keyPairs.
  * **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. This is only obtained at the time of its creation.<br><br>

To get the currents KeyPairs, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'api-key: 5i3w1FwOAGehi5Ce' \
  -H 'api-secret: HAciZAv0IODXAd2fXYEdZMAZHiOKb3En'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns the message "acknowledged" </h5>

When a KeyPair is successfully retrieved, the code returns a JSON object with info about the KeyPair and the user.

>Fields of the response:
* **userID** (_string_): The user id owner of the KeyPair.  _e.g.:"5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"_
  
* **userAlias** (_string_) The user's alias on the platform. _e.g.:johny<br>_
  
* **userName** (_string_): user's full name.
  
* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_<br>

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.<br><br>


><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect apyKey"
}
```
> <h5> Code 401 - Authentication required
Authentication required</h5>

The 401 error code indicates that you need authentication to do this request.

> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the KeyPair, cannot be found.

There are several reasons why this may occur, such as the KeyPair not existing in the system or this has expired. It's important to double-check the parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.
