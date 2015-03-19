Single-sign on is performed by passing the `nick` and/or `jws` options via the [Embed API](../Embed API 2)

### nick ###

Pseudo Single Sign On: Suggests a nickname for the current user, which will be assigned if they would otherwise have an auto-generated nickname. This option will have no effect if the user is signed into Scrollback, has manually chosen a nickname (even on another website) or was assigned a nickname due to another website’s suggestion.

In the latter two cases only occur if the user loads your site while another tab with scrollback (on another website) is also open.

### jws ###

Single Sign On: Provides an *assertion* containing the email address of the current user in the form of a JSON Web Signature (JWS), which must be generated and cryptographically signed on the server-side. This will sign the user into Scrollback using that email address. If no account with that email addr|ess exists, one will be created automatically. If the user is already signed in using some other account, they will be prompted to pick an account to continue with.

This option is only enabled for whitelisted domains, and additionally has the following restrictions:

1. A user who has signed in using jws can only talk in rooms that trust (have whitelisted) the embedding website. If they navigate to any other room or to other parts of the scrollback such as the user preferences screen, they will be prompted to sign in using facebook or persona.

2. Users may whitelist or blacklist websites that may sign them in. If the website is neither whitelisted nor blacklisted by the user, sign in will succeed but the user will receive an email informing them that their account was signed into from this website. The email will contain links to help the user add this site to the whitelist or blacklist.

3. If a new user account is created due to this feature, the website will be automatically added to the whitelist for that user, and the user will receive an email welcoming them to scrollback and informing them about all the sign-in options.

Implementing SSO using JWS. Email us at integrate@scrollback.io to obtain a shared HMAC_SHA256 key. Keep it secure: do not check it into version control or put it in a public folder. Use it to generate a JWS token on the server with a JWT library for your language. The header should be `{ "alg": "HS256", "typ": "JWS" }` and the payload should be:

```json
{ "iss": "<issuing website's domain>",
  "sub": "<email address of user>",
  "aud": "scrollback.io",
  "iat": "<time of issue; unix timestamp>"
  "exp": "<time of expiry; unix timestamp>" }
```
Note that an aud containing scrollback.io and a reasonable iat OR exp are mandatory. After running it through your JWS generation library (along with the shared key), you might get a string like `eyJ…CJ9.eyJ…Mn0.vuq…jR0` (ellipses show omitted characters). This can be used as the value of the jws option.