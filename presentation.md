  ,d               88
  88               88
MM88MMM ,adPPYba,  88   ,d8  ,adPPYba, 8b,dPPYba,  ,adPPYba,
  88   a8"     "8a 88 ,a8"  a8P_____88 88P'   `"8a I8[    ""
  88   8b       d8 8888[    8PP""""""" 88       88  `"Y8ba,
  88,  "8a,   ,a8" 88`"Yba, "8b,   ,aa 88       88 aa    ]8I
  "Y888 `"YbbdP"'  88   `Y8a `"Ybbd8"' 88       88 `"YbbdP"'

OAuth2 - Token Exchange with mo


# Agenda

* 1. Authn vs Authz
* 2. Tokens
* 3. Roles
* 4. Protocol Flow
* 5. Grant Types
* 6. Questions


# Authn vs Authz

* Authentication: to verify the identity of who you are.
* Authorization: to verify that you should be granted access to a resource or action.


# Authn vs Authz

Example 1: Getting pulled over by the police.

> Drivers license and registration?

* driver license: identity
* vehicle registration: authorization


# Authn vs Authz

Example 2: Riding the bus

Transit pass/token authorizes you to ride the bus for 90 minutes.
Authentication is not required.

```text
        +------------------------------+
        |        transit ticket        |
        |                              |
        |                              |
        |                              |
        | expires: 90 minutes          |
        +------------------------------+
```


# Authn vs Authz

Transferring the token to someone else, authorizes them to ride the bus for 90 minutes.
You're not supposed to transfer your transit token but detection is difficult.

```text
        +------------------------------+
        |        transit ticket        |
        |                              |
        |                              |
        |                              |
        | expires: 90 minutes          |
        +------------------------------+
```


# Tokens - Types

* Access Token: Used to access a resource.
* Refresh Token: Exchanged for a new access/refresh token.


# Tokens - Access Token

The purpose of the access token is to allow clients to access a
protected resource scoped to the privileges defined by the token and
scope.

The `access token` represents a subject, audience, issuer and expiration.

```text
        +------------------------------+
        |        transit ticket        |
        |                              |
        |                              |
        |                              |
        | expires: 90 minutes          |
        +------------------------------+
```


# Tokens - Access Token

Subject: Ticket holder
Audience: Bus Driver
Issuer: Calgary Transit
Expiration: 90 minutes from when the ticket was purchased.

```text
        +------------------------------+
        |        transit ticket        |
        |                              |
        |                              |
        |                              |
        | expires: 90 minutes          |
        +------------------------------+
```


# Tokens - Stateful vs Stateless

Stateful: A stateful token is one where the token
needs to be looked up in a database to honour it.

Stateless: A stateless token has all the information
encoded in the token.


# Tokens - Stateful

Example: Concert ticket

When you enter a concert venue and you are asked to present your ticket.
They will likely scan your ticket to verify that the ticket is legit.
The scan will need to verify that the ticket is in a database.

```text
        +------------------------------+
        |    KRS-ONE                   |
        |                              |
        |                              |
        |                              |
        |                              |
        |      |XXXX|XXXX|XXXX|XXXX|   |
        +------------------------------+
```


# Tokens - Stateless

Example: Calgary Transit Ticket

When you board a bus in Calgary, you must show the driver the ticket.
All the data the driver needs to admit you is in the ticket itself.

```text
        +------------------------------+
        |        transit ticket        |
        |                              |
        |                              |
        |                              |
        | expires: 90 minutes          |
        +------------------------------+
```


# Tokens - JWT

JSON web tokens allow us to create stateless
tokens that encode the necessary information into the token.

```text
{header}.{body}.{signature}
```


# Tokens - JWT

Example Token

```text
{header}.{body}.{signature}
```

```text
eyJhbGciOiJSUzI1NiJ9.eyJleHAiOjE1NTMyMDYxNDMsImlhdCI6MTU1MzExOTc0MywiaXNzIjoiaHR0cHM6Ly9zaGlyby50ZXN0L21ldGFkYXRhIiwibmJmIjoxNTUzMTE5NzQzLCJqdGkiOiIzMGVlNGYwNi0zZTJiLTRlZjQtOTYxZS01YTFkZmQ1MzBjYTUiLCJzdWIiOiJkOThlY2MwNS1lYWI4LTQ2ODMtODI4OC0yNDkzMTJkM2Y1OTIiLCJ0b2tlbl90eXBlIjoiYWNjZXNzX3Rva2VuIiwiZW1haWwiOiJtb2toYUBjaXNjby5jb20iLCJmaXJzdF9uYW1lIjoiVHN1eW9zaGkiLCJsYXN0X25hbWUiOiJHYXJyZXR0Iiwib3JnYW5pemF0aW9uX25hbWUiOiJ2b2x0cm9uIiwicm9sZXMiOlsiYWNjb3VudF9hZG1pbiJdfQ.  BrWtDArYiut47Oo76UTD2FGDMgrpFDa2wURVHCMuHb4P6lNP8-fcHqVAOl0bjEqT0RWx6w2MabWBELAWxUWpdVPR-pM_yfIlV0elIQvsFkQOItm8CJlkA-uHGEkBTiqBcOg9ia8ciPKX-sm4SZ0ufYS6kpR0udAOl1Rg0SSJMBkEmkCL4c7wDjACRdm_6M9vXlgpmr388o9wp7Dvd3ts-gRQ4T6BDHxm5F5ckEsnXEIRKstJdFWqgLKCPpyzXYRe5_QAIoB0qWKtzav8tPsKq_hHeKMSXniIY_WF9qMYk3XjxB5aHuqIWaw-zQnZyOUakCYIbgo-CWT8ta0sA0mt9g
```


# Tokens - JWT

```json
{
  "alg": "RS256"
}
```
```json
{
  "exp": 1553206143,
  "iat": 1553119743,
  "iss": "https://shiro.test/metadata",
  "nbf": 1553119743,
  "jti": "30ee4f06-3e2b-4ef4-961e-5a1dfd530ca5",
  "sub": "d98ecc05-eab8-4683-8288-249312d3f592",
  "token_type": "access_token",
  "email": "mokha@cisco.com",
  "first_name": "Tsuyoshi",
  "last_name": "Garrett",
  "organization_name": "voltron",
  "roles": [
    "account_admin"
  ]
}
```


# Tokens - Refresh Token

An `access token` can expire. When an `access token` expires a
client can exchange a `refresh token` to gain a new `access token`
and `refresh token`.

The purpose of the `refresh token` is to allow a client to get a new
`access token` and `refresh token` pair. Once a `refresh token` is used
it cannot be re-used.

```text
        +------------------------------+
        |        Credit Card           |
        |                              |
        |         XXXX-XXXX-XXXX-XXXX  |
        |                              |
        |            expires: 2024-01  |
        +------------------------------+
```


# Roles - OAuth 2.0

* Resource Owner: The HUMAN!
* Resource Server: The Api
* Authorization Server: The OAuth 2.0 server.
* Client: Your service, web app, SPA, mobile app.


# Protocol Flow

OAuth 2 is a delegation protocol. The `client` does not know the
credentials of the `resource owner` but can access resources on it's
behalf.

```text
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|   Resource    |
     |        |                               |     Owner     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->| Authorization |
     | Client |                               |     Server    |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|    Resource   |
     |        |                               |     Server    |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```


# Protocol Flow

OAuth 2 is a delegation protocol. The `client` does not know the
credentials of the `resource owner` but can access resources on it's
behalf.

```text
     +--------+                               +---------------+
     |        |--(A)- Authorization Request ->|               |
     |        |                               |     HUMAN     |
     |        |<-(B)-- Authorization Grant ---|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(C)-- Authorization Grant -->|               |
     | my app |                               |   auth.amp.*  |
     |        |<-(D)----- Access Token -------|               |
     |        |                               +---------------+
     |        |
     |        |                               +---------------+
     |        |--(E)----- Access Token ------>|               |
     |        |                               |   api.amp.*   |
     |        |<-(F)--- Protected Resource ---|               |
     +--------+                               +---------------+
```


# Protocol Flow

```text
    +--------+                                           +---------------+
    |        |--(A)------- Authorization Grant --------->|               |
    |        |                                           |               |
    |        |<-(B)----------- Access Token -------------|               |
    |        |               & Refresh Token             |               |
    |        |                                           |               |
    |        |                            +----------+   |               |
    |        |--(C)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(D)- Protected Resource --| Resource |   | Authorization |
    | Client |                            |  Server  |   |     Server    |
    |        |--(E)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(F)- Invalid Token Error -|          |   |               |
    |        |                            +----------+   |               |
    |        |                                           |               |
    |        |--(G)----------- Refresh Token ----------->|               |
    |        |                                           |               |
    |        |<-(H)----------- Access Token -------------|               |
    +--------+           & Optional Refresh Token        +---------------+
```


# Grant Types

* Authorization Code: for web apps
* Implicit: for single page apps.
* Password Credentials: for trusted clients.
* Client Credentials: for service authentication.
* Refresh: for exchanging a refresh token for an access token.
* Extensions: saml bearer, jwt bearer


# Grant Types - Authorization Code

For server based applications where a `client id` and `client secret`
can be stored securely.

This uses a redirect flow that depends on the user agent having access
to both the authorization server and the client web app.

## Authorization Code Grant

```text
    +----------+
    | Resource |
    |   Owner  |
    |          |
    +----------+
         ^
         |
        (B)
    +----|-----+          Client Identifier      +---------------+
    |         -+----(A)-- & Redirection URI ---->|               |
    |  User-   |                                 | Authorization |
    |  Agent  -+----(B)-- User authenticates --->|     Server    |
    |          |                                 |               |
    |         -+----(C)-- Authorization Code ---<|               |
    +-|----|---+                                 +---------------+
      |    |                                         ^      v
     (A)  (C)                                        |      |
      |    |                                         |      |
      ^    v                                         |      |
    +---------+                                      |      |
    |         |>---(D)-- Authorization Code ---------'      |
    |  Client |          & Redirection URI                  |
    |         |                                             |
    |         |<---(E)----- Access Token -------------------'
    +---------+       (w/ Optional Refresh Token)
```


# Grant Types - Authorization Code

```text
https://www.example.com/oauth/authorize
  ?response_type=code
  &client_id=client_id
  &redirect_uri=https://www.example.org/oauth/callback
  &scope='read:scim.me write:scim.me'
```

----------------------------------------------------
Login

username: xxxxxxx
password: xxxxxx

          [login]


# Grant Types - Authorization Code

`client_id` will be given the following permissions:

    read your profile information
    write your profile information

                      [okay]

----------------------------------------------------

```text
https://www.example.org/oauth/callback
  ?grant_type=authorization_code
  &code=secret
  &redirect_uri=https://www.example.org/oauth/callback
  &scope='read:scim.me write:scim.me'
```


# Grant Types - Authorization Code

```text
    +--------+                                           +---------------+
    |        |--(A)------- Authorization Grant --------->|               |
    |        |                                           |               |
    |        |<-(B)----------- Access Token -------------|               |
    |        |               & Refresh Token             |               |
    |        |                                           |               |
    |        |                            +----------+   |               |
    |        |                            |          |   |               |
    |        |                            |          |   |               |
    |        |                            | Resource |   | Authorization |
    | Client |                            |  Server  |   |     Server    |
    |        |                            |          |   |               |
    |        |                            |          |   |               |
    |        |                            |          |   |               |
    |        |                            +----------+   |               |
    |        |                                           |               |
    |        |                                           |               |
    |        |                                           |               |
    |        |                                           |               |
    +--------+                                           +---------------+
```


# Grant Types - Authorization Code

```bash
$ curl https://www.example.com/oauth/tokens \
  -X POST \
  -d '{"grant_type":"authorization_code","code":"KwuYwtE69C5dvhbpxwekp5ie"}' \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Basic base64(client_id:client_secret)"
```

```text
200 OK

Cache-Control: private, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
```
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "eyJleHAiOjE1NDA5M"
}
```


# Grant Types - Refresh Token

This grant can be used by a client to exchange a
`refresh token` for a new `access token` and `refresh token`.

```text
    +--------+                                           +---------------+
    |        |--(A)------- Authorization Grant --------->|               |
    |        |                                           |               |
    |        |<-(B)----------- Access Token -------------|               |
    |        |               & Refresh Token             |               |
    |        |                                           |               |
    |        |                            +----------+   |               |
    |        |--(C)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(D)- Protected Resource --| Resource |   | Authorization |
    | Client |                            |  Server  |   |     Server    |
    |        |--(E)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(F)- Invalid Token Error -|          |   |               |
    |        |                            +----------+   |               |
    |        |                                           |               |
    |        |--(G)----------- Refresh Token ----------->|               |
    |        |                                           |               |
    |        |<-(H)----------- Access Token -------------|               |
    +--------+           & Optional Refresh Token        +---------------+
```

```text
POST /token HTTP/1.1
Authorization: Basic base64(client_id:client_secret)
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&refresh_token=tGzv3JOkF0XG5Qx2TlKWIA
```

Response:

```text
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
}
```


# Protocol Flow - Accessing a Protected Resource

```text
GET /api/policies/
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9
Accept: application/json
Content-Type: application/json

HTTP/1.1 200 OK
Content-Type: application/json

[
  { "name": "Audit" },
  { "name": "Protect" },
]
```


# Moonwalk to this!

```text
            _,.-"T
      _.--{~    :l
    c"     `.    :I
    |  .-"~-.\    l     .--.
    | Y_r--. Y) ___I ,-"(~\ Y
    |[__L__/ j"~=__]~_~\." _/
 ___|  \.__.r--<~__.T T/ "~/
'--cl___/\ ( () ).,_L_]}--{
   `--'   `-^--^\ /___"(~\ Y
                 "~7/ \ " `/
                  // //]--[
                 /> oX |: L
                //  /  `| o\
               //. /    I  [
              / \]/     l: |
             Y.//       `|_I
             I_Z         L :]
            /".-7        [n]l
           Y / /         I //
           |] /         /]"/
           L:/         //./
          [_7      _  // /
            _  ,-="_"^K_/
           [ ][.-~" ~"-.]
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```



# Conclusion

An `access token` decouples a resource owners credentials from the
authorization that it is delgating to a client to access protected
resources from a resource server. A `refresh token` can be used by a
client to gain a new `access token` and `refresh token`.

The exchange process can be triggered when an `access token` expires or
is revoked.

```text
    +--------+                                           +---------------+
    |        |--(A)------- Authorization Grant --------->|               |
    |        |                                           |               |
    |        |<-(B)----------- Access Token -------------|               |
    |        |               & Refresh Token             |               |
    |        |                                           |               |
    |        |                            +----------+   |               |
    |        |--(C)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(D)- Protected Resource --| Resource |   | Authorization |
    | Client |                            |  Server  |   |     Server    |
    |        |--(E)---- Access Token ---->|          |   |               |
    |        |                            |          |   |               |
    |        |<-(F)- Invalid Token Error -|          |   |               |
    |        |                            +----------+   |               |
    |        |                                           |               |
    |        |--(G)----------- Refresh Token ----------->|               |
    |        |                                           |               |
    |        |<-(H)----------- Access Token -------------|               |
    +--------+           & Optional Refresh Token        +---------------+
```


# Addendum

> While we’re talking about credential rotation and how to implement it, Voltron team has implemented a form of it already in their services.
> Mo will present about their approach, rationale, and challenges related to OAuth and it’s implementation within Voltron.


> You can customize Lambda functions to extend Secrets Manager rotation to other secret types, such as API keys and OAuth tokens used to authenticate users to mobile applications.
- https://aws.amazon.com/secrets-manager/


# Thanks

References:

* https://auth.amp.cisco.com/doc
* https://aws.amazon.com/secrets-manager/
* https://jwt.io/
* https://tools.ietf.org/html/rfc6749
* https://tools.ietf.org/html/rfc7519
* https://tools.ietf.org/html/rfc7522
