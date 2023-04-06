<h2>Email</h2>
<nav>
Table of Content:
  <ul>
    <li><a href="#createEmail">Create Email</a></li>
    <li><a href="#deleteEmail">Close Email</a></li>
    <li><a href="#getEmails">Get Emails</a></li>
    <li><a href="#verifyEmail">Verify Email</a></li>
    <li><a href="#newCodeEmail">New Verification Code</a></li>
    <li><a href="#setPreferredkEmail">Set Preferred Email</a></li>
</ul>
</nav>
  
<h3 id="createEmail">Create Email</h3>
This API endpoint creates an unverified Email and send a verification code.<br><br>
EndPoint: https://intelligence.threatwinds.com/api/auth/v2/email

<h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the Email is active before attempting to retrieve the authorization header for account deletion._e.g.:
* **Message** (_JSON_):
  * **address**	(string): The email parameter represents a user's email address that is going to be associated with their account. This email address is used for communication with the user, and may also be used as a way to reset their password or verify their account. _e.g.: john@doe.net_<br><br>
<br>

To create a email, use a **POST**</span> request, for example:

```bash
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/email' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer fq6JoEFTsxiXAl1cVdPDnK4emIQCwaUBfq9JoEFTsxhXAl1cVxPDnK4emIQCwaUB' \
  -H 'Content-Type: application/json' \
  -d '{
  "address": "john@doe.com"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Return a verification code as Response</h5>

We get this code when the email was created successfully. It returns a verificationCodeID. Remember that you need to verify this email before you can use it.

>Fields of the response:

* **verificationCodeID** (_string_): The verification code id that is used in the Email Verification Endpoint to ensure that the email is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

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

<h3 id="deleteEmail">Delete Email</h3>

This API endpoint delete an Email.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/email>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for account deletion._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

* **Email ID** (_string_):  The id of the Email that you want to delete.  _e.g.: john@doe.net_<br>
  
To delete an email, use a **DELETE** request, for example:

```bash
curl -X 'DELETE' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 5f35d2c4-5633-4b16-bxf0-5ca32ef8ea2e'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns the message "acknowledged" </h5>

When a email is successfully deleted, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

>Fields of the response:

* **message** (_string_): A message that describes the result of the operation. _e.g.:_"acknowledged"

><h4>Errors</h4>

>Fields of the response:

* **uuid** (_string_): The id of the error. _e.g.: 6070686d-917d-44bb-ad11-02345a7f1939_
* **error** (_string_): The description of the error. _e.g.: "not found"_

> <h5> Code 404 - Not found</h5>

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case, the email or the session, cannot be found.

There are several reasons why this may occur, such as:
* the session or the email has expired.
* the session has not been verified.
*  the email does not exist in the system.
<br><br>
  
It's important to double-check the parameters before attempting to delete the email.

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
We get this code when an internal server error occurs. This is not email related.

For example:

```json
{
  "uuid": "5e67b7a2-ee10-493d-83da-15ba3cf66bd2",
  "error": "Internal Server Error"
}
```

<h3 id="getEmails">Get Emails</h3>

This API endpoint to get the user emails.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/emails>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to use the authorization header._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```

To get the currents emails, use a **GET** request, for example:

```
curl -X 'GET' \
  'https://intelligence.threatwinds.com/api/auth/v2/emails' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer 63VD8JoautQNqRLcqNOlJid02R7CDbWK'
```

><h4>Returns</h4>

> <h5>Code 202</h5>

<h5>Returns a list of Emails</h5>

It returns the list of the current user Emails.

>Fields of the response:

* **address** (_string_): The address of the email.

* **emailID** (_string_): The id of the email that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>
  
* **preferred** (boolean): A boolean that represents if the email is set as preferred or not._<br>

* **verified** (_boolean_): A boolean that represents if the email is verified or not.


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

<h3 id="verifyEmail">Verify Email</h3>

This API endpoint verifies the Email using code sent by email.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/email/verification>

> <h4> Parameters</h4>

* **verificationCodeID** (_string_): You can obtain it from the email that you wish to verify at the time of its creation or with the New Verification Code endpoint. _e.g.: "1c233e4a-27e7-4b77-8b0d-9a35cf212afe"_<br>

* **code** (_string_): A code sent to your preferred email when a email is created in the system. _e.g.: "757564"_<br>
  
To verify a Email, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/verification' \
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

When a email is successfully verified, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.

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

<h3 id="newCodeEmail">New Verification Code</h3>

This API endpoint sends a new verification code.<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/email/verification>

> <h4> Parameters</h4>

* **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request._e.g.:

```
e.g.: Bearer fq6JoEFTsxiXAl1cVxPDnK4emIQCwaUBfq6JoEFTsxiXAl1cVxPDnK4emIQCwaUB
```
* **Message** (_Json_):
   * **address**	(string): The email parameter is the email address that you want to verify and you want to send a new verification code. _e.g.: john@doe.net_<br><br>

To get a new verification code, use a **POST** request, for example:

```
curl -X 'POST' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/verification' \
  -H 'accept: application/json' \
  -H 'Authorization: Jlore138gST9TnQKRWZZzLW4NfxCo0q8' \
  -H 'Content-Type: application/json' \
  -d '{
  "address": "john@doe.com"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns a verificationCodeID. </h5>

Returns a new verificationCodeID and a code is sent to the email. Then you can use both in the Verification Endpoint to verify your email.

>Fields of the response:

* **verificationCodeID** (_string_): The verification code id that is used in the Email Verification Endpoint ensure that the email is valid. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br><br>

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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the email, cannot be found.

There are several reasons why this may occur, such as the email not existing in the system or this has expired. It's important to double-check the authorization header.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.

<h3 id="setPreferredkEmail">Set Preferred Email</h3>

This API endpoint an email as preferred<br><br>

**EndPoint:** <https://intelligence.threatwinds.com/api/auth/v2/
/email/preferred>

> <h4> Parameters</h4>

   * **Authorization header** (_string_): This authorization header can be obtained from an active session of the account. Please ensure that the session is active before attempting to retrieve the authorization header for the request._e.g.:
  * **Message** (_Json_):
    * **emailID** (_string_): The id of the email that was created. _e.g.: 5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e_<br>

To set a preferred email, use a **PUT** request, for example:

```bash
curl -X 'PUT' \
  'https://intelligence.threatwinds.com/api/auth/v2/email/preferred' \
  -H 'accept: application/json' \
  -H 'Authorization: Jlore138gST9TnQKRWZZzLW4NfxCo0q8' \
  -H 'Content-Type: application/json' \
  -d '{
  "emailID": "5f35d2c4-5633-4b16-bbf0-5ca22ef8ea2e"
}'
```

><h4>Returns</h4>

> <h5>Code 202</h5>
<h5>Returns the message "acknowledged" </h5>

When a email is successfully set as preferred, the code returns a JSON object with a 'message' field whose value is 'acknowledged'.


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

The HTTP status code 404, also known as a "Not Found" error occurs when the requested resource, in this case the email, cannot be found.

There are several reasons why this may occur, such as the email not existing in the system or this has expired. It's important to double-check the parameters.

For example:

```json
{
  "uuid": "9ca21f08-c3a3-4eeb-8179-49669d7fe1fa",
  "error": "record not found"
}
```
> <h5> Code 500 - Internal server error</h5>
We get this code when an internal server error occurs. This is not user related.
