# SSO Login

The SSO Login is based on OpenID connect and we're using the PKCE Method.
https://oauth.net/2/pkce/

## Recommended SDKs

* PHP (https://github.com/jumbojett/OpenID-Connect-PHP)
* Node.JS (https://www.npmjs.com/package/openid-client)

## Endpoints

* Dev Env (https://config.dev.youre.id/)
* Staging (https://config.prepro.youre.id/)
* Production (https://config.pro.youre.id/)

##  Needed informations

We need for every Environment all possible callbacks url to ensure that the login flow will work.

## Example Response (After claimed token)

Once you claimed the token, you'll see those fields. Before claiming the token itself, you will get a id_token, access_token and refresh_token. Those tokens should be saved into a session.

```
{
  "sub": "4d685ed0-aea3-44ab-9c40-427a1e5ae2c0", // userId to use
  "auth_time": 1678783375,
  "exp": 1678786975,
  "email": "user@email.com"
}
```
