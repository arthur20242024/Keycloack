## 1. OAuth2 vs OIDC

### OAuth2
OAuth2 is an authorization protocol. It lets an application access a resource (an API, a user's data) on someone's behalf, without that person sharing their password with the app. Instead, the app receives an access token that proves it has permission to act whithin a limited scope.

### OIDC
OIDC (OpenID Connect) is built on top of OAuth2. It adds an authentification layer: it answers the question "who is the user". It does this by introducing the ID token, a JWT that identifies the user.

0Auth2 = authorization (what can you access) && OIDC = authentification (who are you ?), built on top of OAuth2 

### Example
- James => Useer
- Professionnal password => client_secret
- Cloackroom => Keyclaock
- Ticket 42 => authorization_code
- The private telephone ligne in the back-room => A direct call from the app’s server to Keycloak’s /token endpoint—never visible to the browser.
- When the waiter gives ticket + Professionnal password => app sends code + client_id + client_secret 
- The pass for retrieving the coat => Access Token 
- The file "It's James Martin" => ID token
## 2. PKCE

### PKCE
PKCE exists to protect public client (a frontend app running in the browser, which can't keep a secret) from having their authorization code stolen and replayed by an attacker. Even if the code is intercepted, it's useless without the code_verifier, which never leaves the app. 

### Example 
- Lea => navigator/frontend app
- Lea's secret word => the "code_verifier"
- Fingerprint => the "code_challenge"
- Attendant => keycloack server
- The notebook of the attendant => Keycloack session
- Ticket 42 => the "authorization_code"
- Max => An attacker
- When Lea comes back and give secret word + ticket => Final echange , app send ticket + code_verifier
- The coat give her back => the tokens (ID token + access token)

Lea Goes to a concert and checks her coat at the cloakroom. Before handing it over, she silently thinks of a secret word, say "lemon37". She tells it to no one. She runs that word through a special mathematical formulq that turns into a code you can't reverse back into the original word, something like "a8f3d1". That fingerprint is what she hands to the attendant, saying "here's my fingerprint, and this is my method,keep it on file. The attendant writes down "a8f3d1" and gives Lea a plain numbr ticket, ticket 42. That ticket holds no secret at all, anyone who got hold of it could read it. 

While Lea heads to her to her seat, Max, a sharp-fingered pickpocket, lifts tickets 42 from her bag. He thinks, "Greta now i can grab the coat". He walks up to the cloackroom and hands over the ticket. But the attendant says, "before I give you the coat, tell me the secret word that matches the fingerprint a8f3d1, I have on file". Max has no idea what that is: he stole the ticket, not the secret word, which only ever existed in Lea's head. The attendant refuses to hand over the coat. 

Later, Lea comes back with her ticket and speaks her seret word "lemon37". The attendant runs the same hashing formula on it, gets "a8f3d1" again, compares it to what's on file: it matches. She hands over the coat. 
 
