# OpenID Connect Token Authentication

I'm going to start with building a REST server in python using [falcon](https://github.com/falconry/falcon). We want the REST API to be only accessible to authenticated users from Keycloak. To achieve this, we need to create a client entity in keycloak. We can do this by clicking on `Clients` and then clicking on `Create` button

![Create Client Button](static/client-create-button.png)

In the `Add Client` view, you just need to provide the Client Id. Let's name this client `my-test-client`. Click on `Save`.

![Add Client](static/new-client.png)

What we have created so far is an [OAuth2 client](https://oauth.net/2/client-types/)

We'd also need to create a new test user with which we can login. For that, click on `Users` from the left panel, then click on `Add User`. 

![Add User](static/new-user.png)

Only a username is required to create a user. Let's name this user `admin` and click on `Save`. Once saved, click on `Credentials`, put in a password which you can remember and turn off the `Temporary` button before clicking on `Set Password` button. 

![Credentials](static/credentials.png)

Once the client and the user has been configured, you can perform the following `POST` request using `curl` to get an access token:

```
curl -X POST http://localhost:8080/auth/realms/test/protocol/openid-connect/token \
 --data "username=admin" \
 --data "password=password" \
 --data "client_id=my-test-client" \
 --data "grant_type=password"
 ```

The response of this request would be something similar to the payload below:

```
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJHS2pCb3lMV0lsYlNNVW1IMXhhZHFPNllaY0hvalhqZlBmMnluZ0hhajN3In0.eyJleHAiOjE1OTY1MTcwNDMsImlhdCI6MTU5NjQ4MTA0MywianRpIjoiMjQ2OTBmZDMtYmYwMC00NTBmLTg5MzctMTg5M2NlYThiZjhhIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL3Rlc3QiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiZjMzZDc0ZDQtYTVlZC00ZGEwLTgxN2MtNTAzZTc1YTgxNDY1IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoibXktdGVzdC1jbGllbnQiLCJzZXNzaW9uX3N0YXRlIjoiZjg1MzAzNTItMjliMy00N2YwLWJmYjUtOTE0YmM4YjY1MzZlIiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyIvKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoiZW1haWwgcHJvZmlsZSIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWRtaW4ifQ.BxmcDQKHcis3DQAJE-APpGsKRn3PkIvF1JhefVWqw4IkuQBwhFvDfVZMt27CC8cZdQEE5v5R_8wvIw4Ju69EQZVw2oqeN9JOvo5Sg9TxFw7dBrmMlPddIDSvbB8L7b4GklT3M75pUjIe2rTG91ZsSCFtFVP3Qj6V5iLPNBlVQWS9sMdVHPRM_fgltTSRetf-iKWox13DXz4cn3P5ARHMAJkj5tr8CNRp5cKJzCct0bQgIULlIhzx_tNdOlr39GwFqx_vjuOdGL-x_yy1uyMFMv-yZQn8KqsbJ5E7MIGQEVGKA31l5jFTYev2kgj8ZTFFeXwxReFcEqmVgVQTxNa2BQ",
    "expires_in": 36000,
    "refresh_expires_in": 98000,
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICI0NDM1MDllMi1kZTRhLTQ3ZDEtYTgzYi1lOTI3ZGFlNzJjMWUifQ.eyJleHAiOjE1OTY0ODI4NDMsImlhdCI6MTU5NjQ4MTA0MywianRpIjoiYTBhZjc0OWUtYThiYy00NTQ3LWIyODYtNzAxYjQ1NTBjNjA5IiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL3Rlc3QiLCJhdWQiOiJodHRwOi8vbG9jYWxob3N0OjgwODAvYXV0aC9yZWFsbXMvdGVzdCIsInN1YiI6ImYzM2Q3NGQ0LWE1ZWQtNGRhMC04MTdjLTUwM2U3NWE4MTQ2NSIsInR5cCI6IlJlZnJlc2giLCJhenAiOiJteS10ZXN0LWNsaWVudCIsInNlc3Npb25fc3RhdGUiOiJmODUzMDM1Mi0yOWIzLTQ3ZjAtYmZiNS05MTRiYzhiNjUzNmUiLCJzY29wZSI6ImVtYWlsIHByb2ZpbGUifQ.Olm0EjyvY5b-T6_eAr9zwTzYZsegrOsuWueO_cQLtYQ",
    "token_type": "bearer",
    "not-before-policy": 0,
    "session_state": "f8530352-29b3-47f0-bfb5-914bc8b6536e",
    "scope": "email profile"
}
```

The token you need is the value of `access_token` from above. If you focus on this value, it's actually 3 base64 encoded strings joined together through dots (`.`). Such a token is called a `JSON Web Token` or `JWT` in short. The part between the two dots is the `payload` which contains information about the user that we just used to generate the token. We can decode it as follows:


```
echo 'eyJleHAiOjE1OTY1MTcwNDMsImlhdCI6MTU5NjQ4MTA0MywianRpIjoiMjQ2OTBmZDMtYmYwMC00NTBmLTg5MzctMTg5M2NlYThiZjhhIiwiaXNzIjoiaHR0cDovL2xvY2FsaG9zdDo4MDgwL2F1dGgvcmVhbG1zL3Rlc3QiLCJhdWQiOiJhY2NvdW50Iiwic3ViIjoiZjMzZDc0ZDQtYTVlZC00ZGEwLTgxN2MtNTAzZTc1YTgxNDY1IiwidHlwIjoiQmVhcmVyIiwiYXpwIjoibXktdGVzdC1jbGllbnQiLCJzZXNzaW9uX3N0YXRlIjoiZjg1MzAzNTItMjliMy00N2YwLWJmYjUtOTE0YmM4YjY1MzZlIiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyIvKiJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsib2ZmbGluZV9hY2Nlc3MiLCJ1bWFfYXV0aG9yaXphdGlvbiJdfSwicmVzb3VyY2VfYWNjZXNzIjp7ImFjY291bnQiOnsicm9sZXMiOlsibWFuYWdlLWFjY291bnQiLCJtYW5hZ2UtYWNjb3VudC1saW5rcyIsInZpZXctcHJvZmlsZSJdfX0sInNjb3BlIjoiZW1haWwgcHJvZmlsZSIsImVtYWlsX3ZlcmlmaWVkIjpmYWxzZSwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWRtaW4ifQ' | base64 -D
```

Which yields:

```
{
    "exp": 1596517043,
    "iat": 1596481043,
    "jti": "24690fd3-bf00-450f-8937-1893cea8bf8a",
    "iss": "http://localhost:8080/auth/realms/test",
    "aud": "account",
    "sub": "f33d74d4-a5ed-4da0-817c-503e75a81465",
    "typ": "Bearer",
    "azp": "my-test-client",
    "session_state": "f8530352-29b3-47f0-bfb5-914bc8b6536e",
    "acr": "1",
    "allowed-origins": [
        "/*"
    ],
    "realm_access": {
        "roles": [
            "offline_access",
            "uma_authorization"
        ]
    },
    "resource_access": {
        "account": {
            "roles": [
                "manage-account",
                "manage-account-links",
                "view-profile"
            ]
        }
    },
    "scope": "email profile",
    "email_verified": false,
    "preferred_username": "admin"
}
```

As you can see, the value of `preferred_username` is `admin` and the value of `azp` (authorization party) is `my-test-client`. Each token also comes with an attached expiration specified as the value of `exp` after which the token should not be acceptable. 

Respectively, the parts at either ends of the `payload` are `header` and the `signature`. The `signature` is literally the digtial signature of the whole payload generated through a private key and an algorithm specified in the `header`. 

The main advantage of a signed `JWT` is that if you have the public key and the algorithm information, you can simply verify the signature and once the signature is verified, you can trust the information in the payload to have come from a valid source (Keycloak Server). If you can trust the payload which contains information about the logged-in user, you can establish that the holder of this token is in-fact the user who's logged in. 

So, in our demo REST API, all we have to do is expect the request to contain the token. If the token is there, validate its signature. Once the signature has been validated, we can establish the identity of the user who performed the request through `preferred_username` field. 

Let's implement a REST endpoint `/v1/self` that returns the info of the logged-in user. You'd need to install the following pip packages:

```
pip install openidcpy==0.8 falcon==2.0.0 bjoern==3.1.0
```

You'd also need something called a `Discovery URL`, which can be found from the Keycloak Administration Console as the hyperlink value of `OpenID Endpoint Configuration`:

![Discovery URL](static/discovery.png)

This URL would be something like http://localhost:8080/auth/realms/test/.well-known/openid-configuration and is used to discover metadata about the keycloak server. Incidentally, this URL can also lead us to the public key required to verify the signature. When you open this URL in the browser, you'll see something like: 

```
{
    .
    .
    .
    "authorization_endpoint": "http://localhost:8080/auth/realms/test/protocol/openid-connect/auth",
    "token_endpoint": "http://localhost:8080/auth/realms/test/protocol/openid-connect/token",
    "jwks_uri": "http://localhost:8080/auth/realms/test/protocol/openid-connect/certs",
    "userinfo_endpoint": "http://localhost:8080/auth/realms/test/protocol/openid-connect/userinfo",
    .
    .
    .
}
```

Opening the URL specified against `jwks_uri` will give you all the public keys that this server has one of which will have the same `id` as specified in the `header` of the `JWT`. Other important endpoints specified here are authorization_endpoint and token_endpoint . We'll use these endpoints later on to generate tokens. 

For now, we'll need to specify this `URL` as an argument to the client from `openidcpy`. Our implementation is as follows:

```
import falcon
from openidcpy import OidcClient
from json import dumps
import bjoern

discovery_url = 'http://localhost:8080/auth/realms/test/.well-known/openid-configuration'
client_id = 'my-test-client'
# Initialize the openidcpy client, this is going to validate the token signature
client = OidcClient(discovery_uri=discovery_url, client_id=client_id)


# A function to extract out the user information
def get_user(claims):
  user = {'id': claims['sub']}
  if 'email' in claims:
    user['email'] = claims['email']
  if 'given_name' in claims:
    user['firstname'] = claims['given_name']
  if 'family_name' in claims:
    user['lastname'] = claims['family_name']
  if 'preferred_username' in claims:
    user['username'] = claims['preferred_username']
  return user


class SelfApi(object):

  def on_get(self, req, resp):
    # If a token isn't sent, we need to unauthorize the request
    if req.auth is None:
      raise falcon.HTTPUnauthorized('Unauthorized', 'Bearer token not provided')
    try:
      # Tokens are usually sent as 'Bearer tokens' i.e. in the format
      # 'Bearer <token>' against the 'Authorization' header
      token = req.auth.split(' ')[1]
      # We need to skip the verification of 'aud' for now
      claims = client.validate_jwt(token, options={'verify_aud': False})
      resp.body = dumps(get_user(claims))
      resp.status = falcon.HTTP_200
    except Exception as e:
      raise falcon.HTTPUnauthorized('Unauthorized', e.args[0])


if __name__ == '__main__':
  api = falcon.API()
  api.add_route('/v1/self', SelfApi())
  bjoern.run(api, '0.0.0.0', 1234)
```

Pasting this code in a file and running it will expose this endpoint on http://localhost:1234/v1/self. Once this is done, we need to perform a `GET` request on this endpoint: 

```
curl -H "Authorization: Bearer <PUT JWT TOKEN HERE>" http://localhost:1234/v1/self
```

Substitute the `JWT` token in the above call (without the `<>`) and press enter. This would yield:

```
{
    "id": "f33d74d4-a5ed-4da0-817c-503e75a81465",
    "username": "admin"
}
```