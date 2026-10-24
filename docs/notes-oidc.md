## 1. Delegated Authentication

### Example
- large resort complex => a realm keycloak 
- Central desk => keycloack
- Lea's passport => login + password
- Restaurant, pool ... => Client app
- Wristband => id_token

Léa goes on vacation to a large resort complex made up several independant venues: a restaurant, a spa, a pool and a gym. Each venue is run by a different compagny that simply rents its space within the complex. None of these venues has it own staff for cheking identities: not the restaurant, not the spa, not the pool hires someone to checks passports. They all agreed, before the complex even opened, to trust one single thing: the wristband issued by the resort's central welcome desk. None of them verify identity themselves, they've all delegated that job to the central desk. 

When Lea arrives, she goes to the central welcome desk once, at the start of her stay She shows her passport, the agent verifies her identity, then puts a wristband on her, stamped with the resort's official seal with her name written on it and the list of zones she's allowed into. 

Lea then goes to the restaurant. Nobody asks for her passport again/ the host looks at the Wristband, recognizes the resort'seal, Lea's name, and let her in. Same thing at the spa, same thing at the pool. None of the venues ever saw Lea's passporrt, they all trust the same wristband, issues once by the central desk. If Lea wants to visit the gym she hasn't been to yes, she doesn't need to go back to the welcolme desk, she just shows her wristband, still valid.

That's delegated authentication.  

## 2. OAuth2 vs OIDC

### OAuth2
OAuth2 is an authorization protocol. It lets an application access a resource (an API, a user's data) on someone's behalf, without that person sharing their password with the app. Instead, the app receives an access token that proves it has permission to act whithin a limited scope.

### OIDC
OIDC (OpenID Connect) is built on top of OAuth2. It adds an authentification layer: it answers the question "who is the user". It does this by introducing the ID token, a JWT that identifies the user.

0Auth2 = authorization (what can you access) && OIDC = authentification (who are you ?), built on top of OAuth2 

### Example
- James => User
- Valet's key => access token
- Hostel => Keycloak
- Restaurant => Client application
- "RESTO-042" => Client_id
- Secret password of contract => Client_secret 
- Ticket 42 => authorization_code
- When the maitres gives slip + Partner number + contract's seccret professionnal password => app sends code + client_id + client_secret
- Bracelet => ID token

Month before James's visit, the restaurant on the corner struck a deal with the hotel so its customers could park at the hotel's lot without the restaurant needing it's own valet. On that day, hotel management handed the restaurant two things, an official partner number "Resto-42", written plainly into the contract, which anyone could come to know, and a secret professional password, agreed between the two managements, never given to any customer.

Tonight, James is dining at the restaurant and asks to have her car parked; the headwaiterwalks her over to the hotel's counterright next doot; the employee displays: "restaurant Resto-42 wants to park your car, do you authorize this ," he recognizes the partner number from the contract, verifies lea's identity whith her id, she confirms; the employee doesn't hand over the valet key right away. He gives the headwaiter a small, short-lived slip, valid for 2 minutes, slip number 42. 

The headwaiter doesn't park himself. He goes to the restaurant's back office, picks up the direct line reserved for hotel's partners, and presents three things together: slip 42, the partner number Resto-42, and the contrat's secret professional password. The hotel's back office checks that the slip is valid, hasn't already been use, and that the pssword matches the one on file for resto-42. Everithing matches, so it finally  hands the restaurant the real valet key (which starts the engine and allows slow driving) and, on top of that, the signed wristband confirming lea's identity. 

 
## 3. PKCE

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
 
