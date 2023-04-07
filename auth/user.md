# User

## Create User

This API endpoint creates a new user and an unverified session.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/user](https://intelligence.threatwinds.com/api/auth/v2/user)

> **Parameters**

* **alias** (_string_) _**unique**_ The alias parameter represents the user's alias on the platform. The alias can contain letters, numbers, and special characters._e.g.:johny_\
  \

* **email** (_string_) _**unique**_ The email parameter represents the user's email address that is going to be associated with their account. This email address is used for communication with the user, and may also be used as a way to reset their password or verify their account. _e.g.: john@doe.net_\
  \

* **fullName** (_string_): user's full name. This name may be used as a way to verify the user. _e.g.: John Doe_

To create a user, use a **POST** request, for example:

```
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/user' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "alias": "johny",
  "email": "john@doe.net",
  "fullName": "John Doe"
}'
```

> **Returns**

> **Code 202**

#### Return a Session as Response

We get this code when the user was created successfully. It returns a [Session](session.md).

> Fields of the response:

* **bearer** (_string_): The bearer (Refers from now on as Autentication Header) is used as a means of authentication for the APIs, and must be included in the header of all requests that require authentication (See [Auth Page](../Auth/AUTENTICATIONAPI.md) for more details). The authentication header must be verified using the verificationCodeID in the the [Session Verification Endpoint](session.md) before it can be used to ensure that it is valid.\


```
e.g.: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **verificationCodeID** (_string_): The verification code id that is used in the [Session Verification Endpoint](session.md) to ensure that the session is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the session. If you want to extend it, use the [Extend Session Endpoint](../Auth/AUTENTICATIONAPI.md). _e.g.:1674492894_\

* **ip** (_string_): The ip from the account and session was created. _e.g.: 1.1.1.1_\

* **sessionID** (_string_): The id of the session that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_\

* **userAgent** (string): The userAgent used to create the account and session. _e.g.: Mozilla/5.0 (X11; Linux x86\_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36_\


> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when a parameter that must be unique, such as an email or an alias, already exists in the system. Another reason for this error could be that the email or alias provided in the request are not valid.

e.g.:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "duplicated key not allowed"
}
```

> **Code 409 - Conflict**

A 409 error, also known as a "Conflict" error, is an HTTP status code that indicates that the client's request conflicts with the current state of the server. In general, a 409 error occurs when there is a conflict between the current state of the resource and the changes that the client is attempting to make.

> **Code 500 - Internal server error**

We get this code when an internal server error occurs. This is not user related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

## Delete user

This API endpoint delete a user.\
\


**EndPoint:** [https://intelligence.threatwinds.com/api/auth/v2/user](https://intelligence.threatwinds.com/api/auth/v2/user)

> **Parameters**

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion.\_e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To create an user, use a **DELETE** request, for example:

```
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/user' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

> **Returns**

> **Code 202**

#### Returns the message "acknowledged"

When a user is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

> Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

> **Errors**

> Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> **Code 404 - Not found**

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or a the session is not longer active. It's important to double-check the authorization header before attempting to delete them.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
}
```

> **Code 400 - Bad request**

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters.

For example:

```
{
  "uuid": "85f0c8a9-d361-49e9-a9d1-826c770fcffb",
  "error": "incorrect authorization header"
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
