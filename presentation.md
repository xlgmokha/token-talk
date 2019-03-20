# Token Talk

OAuth 2.0 - Token Exchange with mo


# Agenda

* 1. Authentication vs Authorization
* 2. Roles
* 3. Protocol Flow
* 4. Grant Types
* 5. Tokens - Expiration & Refresh
* 6. Tokens - Stateful vs Stateless
* 7. Questions


#

Authentication vs Authorization

* Authentication: to verify the identity of the user given the credentials received.
* Authorization: to determine if the user should be granted access to a particular resource.

E.g.

Woop-woop! That's the sound of...

> Drivers license and registration?

* driver license: identity
* vehicle registration: authorization

(slightly, flawed example. i know)


# Roles

* RO - Resource Owner: The HUMAN!
* RS - Resource Server: The Api
* AS - Authorization Server: The OAuth 2.0 server.
* C - Client: Your service, web app, SPA, mobile app.


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
  &scope='read:scim.me write:scime.me'
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
  &scope='read:scim.me write:scime.me'
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
  -H "Authorization: Basic MjA5OTA4N2EtMDdmZi00ZThkLTljOGEtYmZmNWNkNjdlMGRkOnZQZWduZnRWNzUxaHdmTW1ZaGs0RG9FbQ=="
```

Response:

```text
200 OK

Cache-Control: private, no-store
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Etag: W/"02c4124cd382f6fd8bcd453970193ea4"
X-Request-Id: 03f7448c-5ae8-4f0c-8fa1-5298665c8a9b
Transfer-Encoding: chunked
```
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiJ9",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "eyJleHAiOjE1NDA5M"
}
```


# Tokens - Types

* Access Token: Used to access a resource.
* Refresh Token: Exchanged for a new access/refresh token.


# Tokens - Access Token

The purpose of the access token is to allow clients to access a
protected resource scoped to the privileges defined by the token and
scope.

The `access_token` represents a subject (mo), and audience (mobile
application) an issuer (OAuth authorization server) and an expiration.

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

`GET /api/body_weight/`: returns the resource owners body weight over time.

Request:

```text
GET /api/body_weight/
Authorization: Bearer access_token
Accept: application/json
Content-Type: application/json
```

Response:

```text
HTTP/1.1 200 OK
Content-Type: application/json

[
  {
    "body_weight": "200",
    "unit": "lbs",
    "created_at": "2015-01-01T00:00:00+00:00"
  },
  {
    "body_weight": "210",
    "unit": "lbs",
    "created_at": "2016-01-01T00:00:00+00:00"
  },
  {
    "body_weight": "220",
    "unit": "lbs",
    "created_at": "2017-01-01T00:00:00+00:00"
  },
  {
    "body_weight": "250",
    "unit": "lbs",
    "created_at": "2018-01-01T00:00:00+00:00"
  },
  {
    "body_weight": "260",
    "unit": "lbs",
    "created_at": "2019-01-01T00:00:00+00:00"
  }
]
```


# Tokens - Refresh Token

An `access_token` can expire. When an `access_token` expires a
client can exchange a `refresh_token` to gain a new `access_token`
and `refresh_token`.

The purpose of the `refresh_token` is to allow a client to get a new
`access_token` and `refresh_token` pair. Once a `refresh_token` is used
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


# Tokens - Refresh Token

Refresh Token Grant: This endpoint is used by a client to exchange a
`refresh_token` for a new `access_token` and `refresh_token`.

Request:

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


# Tokens - Stateful vs Stateless

JWT ... yada yada yada.


# Conclusion

An `access_token` decouples a resource owners credentials from the
authorization that it is delgating to a client to access protected
resources from a resource server. A `refresh_token` can be used by a
client to gain a new `access_token` and `refresh_token`.

The exchange process can be triggered when an `access_token` expires or
is revoked.


# Thanks

References:

* https://tools.ietf.org/html/rfc6749
