# Terms in OpenID Connect (OIDC) and OAuth2 parlance

## OAuth 2
----------
OAuth 2 is a set of standards defined by IETF through [RFC 6749](http://tools.ietf.org/html/rfc6749).

Basically, OAuth2 is a means for delegated authorization. 
- The authorization server provides authorization as-a-service
- Resource owners (we'll define these in a moment) delegate the responsibility of authorizing clients to the authorization server. They establish trust with the authz server (to be elaborated later, but this trust is verifiable)

## OpenID Connect
---------
*Caveat* OpenID Connect (Oidc henceforth) is in no way related to OpenID

Oidc provides the (missing) authentication piece on top of OAuth2. It authenticates users, issues "Identity Token" containing identity claims and exposes a userinfo endpoint to work with an Identity (i.e. who are we dealing with)  
This is how oidc [defines itself](http://openid.net/specs/openid-connect-core-1_0.html).
## OAuth2 Actors
-----------
The RFC doc calls these as roles and it defines four of these:

1. Resource Owner
```
This is the owner of the data or service that needs to be protected. E.g. Consider the case of my Google profile data, I am the resource ownder who owns my profile data.
``` 
2. Resource Server
```
This is the server that exposes endpoints to access resource owner's data. It has the responsibilty to protect the resources and make them available only to callers who possess valid access tokens
```
3. Client
```
This is the app that accesses data hosted by the resource server with approval from the resource owner. The approval is in the form of "Consent" provided by the resource owner and is conveyed to the resource server through the access token
```
4. Authorization Server
```
This is the server that has the responsibility of obtaining consent from the resource owner, and issuing access tokens to Client so it can access resource server on-behalf-of resource owner 
```

## JSON Web Tokens (JWT)
--------
JWT is a security token and a data structure that - 
- contains info about issuer (who issued this) and subject (the user) and audience (who can this token be used with - usually the resource server)
- digitally signed - tamper proof and authentic (the robustness of the signature depends on the signing mechanism - more on that later)
- (typically) contains an expiration time
- JSON encoded (JSON is the data structure format)
- symmetric and assymetric signatures - HMACSHA256-384, ECDSA, RSA
- symmetric and assymetric encryption using RSA, AES/CGM

```
Header			  {
						"typ": "JWT",
						"alg": "HS256"                 //--> The signatures algorithm						
				    }
Claims			  {
						"iss": "http://myissuer",
						"exp": "1340819380",			//--> ticks since 1/1/1970
						"aud": "http://mysresource",	//--> can be used with myresource
						"sub": "alice",
						"client": "xyz"
						"scope": ["read", "search"]     // what is the token good for?
                        "claims": [{"type": "something", "value": "foo"},{"type": "something", "value": "foo"}]
					}					
					
General format: base64UrlEncodedHeader.base64UrlEncodedClaims.base64UrlEncodedSignatureOfheaderConcatenatedWithClaimsWithAPeriod

eyJhbGcas2345l.eyJpc234k524135mn45kh545klj2345klj423.4MTsadf4325jh435hg45
```