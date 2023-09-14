# SSO Login

The SSO Login is based on OpenID connect and we're using the PKCE Method.
https://oauth.net/2/pkce/

## Recommended SDKs

* PHP (https://github.com/jumbojett/OpenID-Connect-PHP)
* Node.JS (https://www.npmjs.com/package/openid-client)

## Endpoints

* Development:
  * Auth Url: https://dev-youre-id.eu.auth0.com/authorize/
  * Access Token Url: https://dev-youre-id.eu.auth0.com/oauth/token
* Staging:
  * Auth Url: https://stage-youre-id.eu.auth0.com/authorize/
  * Access Token: Url: https://stage-youre-id.eu.auth0.com/oauth/token
* Production:
  * Auth Url: https://auth.youre.id/authorize/
  * Access Token Url: https://auth.youre.id/oauth/token
 
## Well known Urls

* Development: https://dev-youre-id.eu.auth0.com/.well-known/openid-configuration
* Staging: https://stage-youre-id.eu.auth0.com/.well-known/openid-configuration
* Production: https://auth.youre.id/.well-known/openid-configuration

##  Needed informations

We need for every Environment all possible callbacks url to ensure that the login flow will work.

## Example Response (After claimed token)

Once you claimed the token, you'll see those fields. Before claiming the token itself, you will get a id_token, access_token and refresh_token. Those tokens should be saved into a session.

Important token claims:
```
{
  "terms": true,    //terms of provider accepted
  "newsletter": false,  // newsletter of provider accepted
  "username": "WeakMoccasin#3984",  //username 
  "email": "max.muster@test.com",
  "email_verified": true,
  "sub": "d6d932f1-f350-4c90-a34f-0c397ac092e2", // userId to use
}
```
