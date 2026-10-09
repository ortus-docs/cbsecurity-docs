---
icon: rotate
---

# Refresh Tokens

ColdBox Security supports the concept of refresh tokens alongside the normal JWT access tokens. Let's start exploring this feature in detail.

## What Is a Refresh Token?

A refresh token is a credential artifact that lets a client application get new access tokens without having to ask the user to log in again. Access tokens may be valid for a short amount of time. Once they expire, client applications can use a refresh token to "**refresh**" the access token.

The client application can get a new access token as long as the refresh token is valid and unexpired. Consequently, a refresh token that has a very long lifespan could theoretically give infinite power to the token bearer to get a new access token to access protected resources anytime. The bearer of the refresh token could be a legitimate user or a malicious user.

### Refresh Token Configuration

Set these in the `jwt` section of your `cbsecurity` settings. The other [JWT settings](../getting-started/configuration/jwt.md) are still required.

| Setting | Default | Description |
| --- | --- | --- |
| `enableRefreshTokens` | `false` | Turns on refresh tokens. Token creation methods then return a struct, `{ access_token, refresh_token }`, instead of a string. While it is `false`, using any refresh capability throws an exception. |
| `refreshExpiration` | `10080` | Minutes until a refresh token expires. The default is 7 days. |
| `customRefreshHeader` | `x-refresh-token` | The header inspected for refresh tokens, by the auto refresh validator and by the refresh endpoint. |
| `enableAutoRefreshValidator` | `false` | The JWT validator tries to refresh expired access tokens by itself when a rule or secured annotation fires, and returns the new tokens as response headers (see below). |
| `enableRefreshEndpoint` | `true` | Enables the `POST /cbsecurity/refreshtoken` endpoint so clients can refresh their tokens. |

## Token Creation

You must enable the setting (`enableRefreshTokens`) in order for the following methods to return a `struct` of tokens. If not, only the access token will be returned as a `string`.

* `attempt( username, password ):struct`
* `fromUser( user, customClaims ):struct`

The returned struct will contain the access and refresh tokens:

```javascript
{
    "access_token"  : "AYjcyMzY3ZDhiNmJk",
    "refresh_token" : "RjY2NjM5NzA2OWJj"
}
```

This is the same procedure for creating access tokens, but now you get a struct of tokens instead of a single access token.

## Refreshing Tokens Manually

You can refresh tokens manually by using the `refreshToken( token, customClaims )` method on the `JwtService` object. You can pass a valid refresh token to be used for refreshment or pass **none** and the token will be inspected from the headers or incoming rc using the `x-refresh-token` value or whatever you setup as your `customRefreshHeader` setting.

```javascript
var newTokens = jwtService.refreshToken();

var newTokens = jwtService.refreshToken( storedRefreshToken );
```

Here is the signature for the refresh method:

```javascript
/**
 * Manually refresh tokens by passing a valid refresh token and returning two new tokens:
 * <code>{ access_token : "", refresh_token : "" }</code>
 *
 * @refreshToken A refresh token
 * @customClaims A struct of custom claims to apply to the new tokens
 *
 * @throws RefreshTokensNotActive If the setting enableRefreshTokens is false
 * @throws TokenExpiredException If the token has expired or no longer in the storage (invalidated)
 * @throws TokenInvalidException If the token doesn't verify decoding
 * @throws TokenNotFoundException If the token cannot be found in the headers
 *
 * @return A struct of { access_token : "", refresh_token : "" }
 */
struct function refreshToken( token = discoverRefreshToken(), struct customClaims = {} )
```

{% hint style="success" %}
`customClaims` where added in v2.15.0
{% endhint %}

{% hint style="danger" %}
**Important:**

Please note that the currently used refresh token will be **invalidated** and **rotated** for you automatically. This is a security feature called **refresh token rotation**, where the refreshed token is automatically rotated for you upon refresh usage.
{% endhint %}

## Refresh Token Endpoint

If you have enabled the refresh token setting (`enableRefreshEndpoint`) then you will also have access to the `POST > /cbsecurity/refreshtoken` API endpoint.

This endpoint is used for applications to refresh their access tokens using the refresh token received when authenticating in the application.

This endpoint must be executed with a `POST` and you will need to pass in your refresh token via the following header: `x-refresh-token` or rc form variable of the same name. If valid, the response `data` will contain two new tokens for you:

```javascript
{
    "access_token" : "AYjcyMzY3ZDhiNmJk",
    "refresh_token" : "RjY2NjM5NzA2OWJj"
}
```

{% hint style="danger" %}
**Important:**

Please note that the currently used refresh token will be **invalidated** and **rotated** for you automatically. This is a security feature called **refresh token rotation**, where the refreshed token is automatically rotated for you upon refresh usage.
{% endhint %}

## Refresh Token Rotation

CBSecurity by default provides you with refresh token rotation every time you want to refresh your access token. This guarantees that every time an application exchanges a refresh token for an access token, a **NEW** refresh token is returned as well. The old refresh token is invalidated and can no longer be used.

Therefore, you no longer have a long-lived refresh token that could provide illegitimate access to resources if it ever becomes compromised or leaked. The threat of unauthorized access is reduced as refresh tokens are continually exchanged and invalidated.

## Refresh Token Header Auto Refreshment

CBSecurity has the ability to refresh access tokens automatically for you when calling any secure resource that is protected by the JWT Validator. All you have to do is send in both tokens via the appropriate headers and enable the `autoRefreshValidator` setting:

* access token
  * bearer token or
  * `x-auth-token`
* refresh token
  * `x-refresh-token`

If the access token has expired or is invalid or missing and the `x-refresh-token` was passed and is valid, then the access token will be re-generated, the refresh token will be rotated, the request will continue as normal and two new **response** headers will be sent back to the calling application.

* `x-auth-token` : refreshed access token
* `x-refresh-token` : new refresh token

The calling application can monitor if those two response headers are sent and save them appropriately.
