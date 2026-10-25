## JWT

A JWT (JSON Web Token) is a digital ID card made of three parts, glued together by dots. A real token looks like this: "eyJT123.POEy23.EIDZNSNRD13".

What looks like gibberish is just three base64-encoded chunks, separated by dots. If you decode the first two(the third isn't meant to be read, you get this:

### 1.The header:
{
 "alg":"RS256",
 "typ": "JWT"
}

This just says: "this is a JWT, and here is the algorithm used to sign it"

### 2. The payload
{
 "sub":"123456",
 "name": "Lea Martin",
 "roles":[user],
 "exp": 193842
}

Each line is called a claim. For example sub (subject) is Lea's unique identifier, name is her names, roles is her list of roles, and exp (expiration) is the exact moment this token sopts being valid.

### 3. The signature

This is what stops someone from editing the payload by hand to, say, change "roles":["user"] into "roles":["admin"]. The signature is computed by Keyclaok using its private key, and the API verifies that signatures using keycloack's matching public key. If someone changes even one character of the payload, the signature no longer matches, and the token is rejected. 

### Access token VS ID token

Both are JWT with the dame three-part structure, but their claims differ because they serve different purposes: 
- the access token mostly carries information about what the app is allowed to do: roles, scopes, exp. The API reads it to decide whether to authorize a request.
- The ID token mostly caaries information about who the user is: name, email, sometimes a profil picture. your fronted reads it to display "Welcolme Lea Martin";

 


