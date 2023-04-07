# KeyPair

Table of Content:

* [Create KeyPair](keypair.md#createKeyPair)
* [Delete KeyPair](keypair.md#deleteKeyPair)
* [Get KeyPairs](keypair.md#getKeyPairs)
* [Verify KeyPair](keypair.md#verifyKeyPair)
* [New Verification Code](keypair.md#newCodeKeyPair)
* [Check KeyPair](keypair.md#checkKeyPair)

## Create KeyPair <a href="#createkeypair" id="createkeypair"></a>

This API endpoint creates an unverified KeyPair.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/keypair](https://intelligence.threatwinds.com/api/auth/v2/keypair)

> **Parameters**

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion.\_e.g.:
* **Message** (_JSON_):
  * **days** (_integer_): The 'days' parameter specifies the length of time, in days, that the key pair will remain valid. _e.g.: 365_
  * **name** (_string_): "The 'name' parameter is used to specify a name or identifier for the key pair. _e.g.: Example_\


To create a KeyPair, use a **POST** request, for example:

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

> **Returns**

> **Code 202**

#### Return a KeyPair as Response

We get this code when the KeyPair was created successfully. It returns a KeyPair(apiKey and apiSecret). Remember that you need to verify this keypair before you can use it.

> Fields of the response:

* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.
* **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. Ensure to save the apiSecret in a safe place, this will not be shown again.
* **verificationCodeID** (_string_): The verification code id that is used in the [KeyPair Verification Endpoint](keypair.md) to ensure that the KeyPair is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_\

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when the authorization header parameter provided in the request is not valid.

e.g.:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

> **Code 401 - Authentication required**

Authentication required

The 401 error code indicates that you need authentication to do this request.

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or the session that is being used for authentication no longer being active. It's important to double-check the authorization header before attempting to use it.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Delete KeyPair <a href="#deletekeypair" id="deletekeypair"></a>

This API endpoint closes a KeyPair.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/keypair](https://intelligence.threatwinds.com/api/auth/v2/keypair)

> **Parameters**

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion.\_e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **KeyPair ID** (_string_): The id of the KeyPair that you want to delete. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\


To delete a KeyPair, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

> **Returns**

> **Code 202**

#### Returns the message "acknowledged"

When a KeyPair is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

> Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the KeyPair or the session, cannot be found.

There are several reasons why this may occur, such as:

* the session or the keypair has expired.
* the session has not been verified.
* the keypair does not exist in the system.\
  \


It's important to double-check the parameters before attempting to delete the KeyPair.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Get KeyPairs <a href="#getkeypairs" id="getkeypairs"></a>

This API endpoint to get the user KeyPairs.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/keypairs](https://intelligence.threatwinds.com/api/auth/v2/keypairs)

> **Parameters**

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to use the authorization header.\_e.g.:

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

> **Returns**

> **Code 202**

#### Returns a list of KeyPairs

It returns the list of the current user [KeyPairs](keypair.md).

> Fields of the response:

* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.
* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_\

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_\
  \


> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

> **Code 401 - Authentication required**

Authentication required

The 401 error code indicates that you need authentication to do this request.

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the session has expired or has not been verified. It's important to double-check the parameters before attempting to make the request.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

## Verify KeyPair <a href="#verifykeypair" id="verifykeypair"></a>

This API endpoint verifies the KeyPair using code sent by email.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification](https://intelligence.threatwinds.com/api/auth/v2/KeyPair/verification)

> **Parameters**

* **verificationCodeID** (_string_): You can obtain it from the KeyPair that you wish to verify at the time of its creation or with the [New Verification Code](keypair.md) endpoint. _e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"_\

* **code** (_string_): A code sent to your preferred email when a KeyPair is created. _e.g.: "757564"_\


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

> **Returns**

> **Code 202**

#### Returns the message "acknowledged"

When a KeyPair is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

> Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the verificationCodeID, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is not longer active.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "record not found"
}
```

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "52dbba7a-99fd-4e87-999f-d9e1df38b4f4",
  "error": "incorrect authorization header"
}
```

> **Code 401 - Unauthorized**

We get this code when the code sent is incorrect.

For example:

```json
{
  "uuid": "39908d33-7953-440f-895c-69423c6f7c71",
  "error": "verification code not found"
}
```

> **Code 403 - Forbidden**

A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## New Verification Code <a href="#newcodekeypair" id="newcodekeypair"></a>

This API endpoint sends a new verification code.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/keypair/verification](https://intelligence.threatwinds.com/api/auth/v2/keypair/verification)

> **Parameters**

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request.\_e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **Message** (_Json_):
  * **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. You can get it at the time of its creation or when you return the list of yours keyPairs.
  * **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. This is only obtained at the time of its creation.\
    \


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

> **Returns**

> **Code 202**

#### Returns a verificationCodeID.

Returns a new verificationCodeID and **a code is sent to your preferred email**. Then you can use both in the [Verify Endpoint](keypair.md#verifyKeyPair) to verify your KeyPair.

> Fields of the response:

* **verificationCodeID** (_string_): The verification code id that is used in the [KeyPair Verification Endpoint](keypair.md) to ensure that the KeyPair is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\
  \


> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
}
```

> **Code 401 - Authentication required**

Authentication required

The 401 error code indicates that you need authentication to do this request.

> **Code 403 - Forbidden**

A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the KeyPair, cannot be found.

There are several reasons why this may occur, such as the KeyPair not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

## Check KeyPair <a href="#checkkeypair" id="checkkeypair"></a>

This API endpoint check a KeyPair and returns privileges\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/KeyPair](https://intelligence.threatwinds.com/api/auth/v2/KeyPair)

> **Parameters**

* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. You can get it at the time of its creation or when you return the list of yours keyPairs.
* **apiSecret** (_string_): The apiSecret is a piece of confidential information that, when combined with the apiKey, serves as a method of authentication for accessing to the endpoints that require it. This is only obtained at the time of its creation.\
  \


To get the currents KeyPairs, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/keypair' \
  -H 'accept: application/json' \
  -H 'api-key: 5i3w1FwOAGehi5Ce' \
  -H 'api-secret: HAciZAv0IODXAd2fXYEdZMAZHiOKb3En'
```

> **Returns**

> **Code 202**

#### Returns the message "acknowledged"

When a KeyPair is successfully retrieved, the code returns a JSON object with info about the KeyPair and the user.

> Fields of the response:

* **userID** (_string_): The user id owner of the KeyPair. _e.g.:"5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"_
* **userAlias** (_string_) The user's alias on the platform. _e.g.:johny_\

* **userName** (_string_): user's full name.
* **apiKey** (_string_): The apiKey is a unique identifier that, when combined with the apiSecret, serves as a method of authentication for accessing to the endpoints that require authentication. It acts as a public key that identifies the authorized user or system making the API request.
* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the KeyPair. _e.g.:1674492894_\

* **keyID** (_string_): The id of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **keyName** (_string_): The name of the KeyPair that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **verified** (_boolean_): A boolean that represents if the KeyPair is verified or not.\
  \


> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```json
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect apyKey"
}
```

> **Code 401 - Authentication required**

Authentication required

The 401 error code indicates that you need authentication to do this request.

> **Code 403 - Forbidden**

A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the KeyPair, cannot be found.

There are several reasons why this may occur, such as the KeyPair not existing in the system or this has expired. It's important to double-check the parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.
