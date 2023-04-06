<h2>Session</h2>

<nav>
Table of Content:
  <ul>
    <li><a href="#createSession">Create Session</a></li>
    <li><a href="#closeSession">Close Session</a></li>
    <li><a href="#getSessions">Get Sessions</a></li>
    <li><a href="#verifySession">Verify Session</a></li>
    <li><a href="#extendSession">Extend Session</a></li>
    <li><a href="#checkSession">Check Session</a></li>
  </ul>
</nav>
<h3 id="createSession">Create Session</h3>
This API endpoint creates an unverified session.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session>

> <h4> Parameters</h4>
  
* **email** (_string_) _**unique**_ The email parameter represents the session's email address that is associated with the account. _e.g.: john@doe.net_<br><br>

To create a session, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "john@doe.net",
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Return a Session as Response</h5>

We get this code when the session was created successfully. It returns a <a href="./SESSION.md">Session</a>. Remember that you need to verify the seesion berfore you can use it.

>Fields of the response:

* **bearer** (_string_): The bearer (Refers from now on as Autentication Header) is used as a means of authentication for the APIs, and must be included in the header of all requests that require authentication (See <a href="./AUTENTICATIONAPI.md">Auth Page</a> for more details). The authentication header must be verified using the verificationCodeID in the <a href="./SESSION.md">Session Verification Endpoint</a> before it can be used to ensure that it is valid. <br>

```
e.g.: fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **verificationCodeID** (_string_): The verification code id that is used in the <a href="./SESSION.md">Session Verification Endpoint</a> to ensure that the session is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the session. If you want to extend it, use the <a href="./AUTENTICATIONAPI.md">Extend Session Endpoint</a>. _e.g.:1674492894_<br>

* **ip** (_string_): The ip from the account and session was created. _e.g.: 1.1.1.1_<br>

* **sessionID** (_string_): The id of the session that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
* **sessionAgent** (string): The sessionAgent used to create the account and session.
_e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36_<br>

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "duplicated key not allowed"_

> <h5> Code 400 - Bad request</h5>

The HTTP status code 400, also known as a "Bad Request" error, is typically returned when the server is unable to process the client's request due to issues with the request parameters. This could happen when the email parameter provided in the request is not valid.

e.g.:

```
{
  "uuid": "6049f96e-4b32-4e1c-a6a3-7967f463d66d",
  "error": "mail: missing '@' or angle-addr"
}
```

> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the user, cannot be found.

There are several reasons why this may occur, such as the user not existing in the system or the session is not longer active. It's important to double-check the authorization header before attempting to delete them.

For example:

```
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
}
```

> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.

For example:

```
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="closeSession">Close session</h3>

This API endpoint closes a session.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **Session ID** (_string_): The id of the session that you want to close. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
To close a session, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns the message "acknowledged" </h5>

When a session is successfully closed, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is no longer active. It's important to double-check the parameters before attempting to delete the session.

For example:

```json
{
  "uuid": "7ae253bc-b1ca-4e8b-a5ca-268645e76f60",
  "error": "not found"
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
We get this code when an internal server error occurs. This is not session related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="getSessions">Get sessions</h3>

This API endpoint to get the user sessions.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/sessions>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To get the currents sessions, use a **GET** request, for example:

```
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/sessions' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns a list of sessions</h5>

It returns the list of the current user <a href="./SESSION.md">Sessions</a>.

>Fields of the response:

* **sessionID** (_string_): The id of the session that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

* **ip** (_string_): The ip from the account and session was created. _e.g.: 1.1.1.1_<br>
  
* **userAgent** (string): The userAgent used to create the account and session.
_e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36_<br>
* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the session. If you want to extend it, use the <a href="./AUTENTICATIONAPI.md">Extend Session Endpoint</a>. _e.g.:1674492894_<br>
* **current** (_boolean_): A boolean value that represents the current session in which you are performing the operations.<br>


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

> <h5> Code 403 - Forbidden</h5>
A 403 error code, also known as a Forbidden error, indicates that the server understood the client's request, but it is refusing to fulfill it. The server can return a 403 error if it suspects that the client is making too many requests, or if the client is using an IP address that is banned or blocked by the server. If the error persists you should contact support for further assistance.

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.

<h3 id="verifySession">Verify Session</h3>

This API endpoint verifies the Sign-in of a session using code sent by email.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session/verification>

> <h4> Parameters</h4>

* **verificationCodeID** (_string_): You can obtain it from the session that you wish to verify at the time of its creation. _e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"_<br>

* **code** (_string_): A code sent to your preferred email when a session is created. _e.g.: "757564"_<br>
  
To close a session, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/verification' \
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

When a session is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the verificationCodeID , cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or is no longer active.

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
  "uuid": "4b990113-7215-4b4c-940a-c31ddbc4ccba",
  "error": "invalid UUID length: 35"
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
We get this code when an internal server error occurs. This is not session related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="extendSession">Extend session</h3>

This API endpoint extends the life of the current user sessions.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session/extend>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To get the currents sessions, use a **PUT** request, for example:

```
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/session/extend' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns the message "acknowledged" </h5>

When a session is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.

<h3 id="checkSession">Check session</h3>

This API endpoint check user session and returns privileges<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/session>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To get the currents sessions, use a **GET** request, for example:

```bash
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/session' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns the message "acknowledged" </h5>

When a session is successfully retrieved, the code returns a JSON object with info about the session.

>Fields of the response:
* **alias** (_string_) The user's alias on the platform. _e.g.:johny<br>_
* **fullName** (_string_): user's full name.
* **sessionID** (_string_): The id of the session that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
* **verify** (_boolean_): A boolean that represents if the user is verified or not.
* **roles** (_list [string]_): A list of the user's roles.  _e.g.: ["user", "admin"]_
* **userID** (_string_): The user id.  _e.g.:"5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"_
* **userAgent** (string): The userAgent used to create the account and session.
_e.g.: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36_<br>
* **ip** (_string_): The ip from the account and session was created. _e.g.: 1.1.1.1_<br>
* **expireAt** (_integer_): A timestamp that represents the expiration datetime of the session. If you want to extend it, use the <a href="./AUTENTICATIONAPI.md">Extend Session Endpoint</a>. _e.g.:1674492894_<br>

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the session, cannot be found.

There are several reasons why this may occur, such as the session not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not session related.