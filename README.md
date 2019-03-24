# Authentication and Authorization for Express with PassportJS and Friends

## Lesson Objectives

- Break down the authentication strategy
  - Explain JSON Web Tokens
  - Break down the authorization strategy
  - Document each phase of the documentation to aid customization

## Intro - What is Authentication and Encryption?

#### Authentication

Today, we are going to learn about making our API more secure. Authentication is about making sure the server knows the identity of the person accessing the API and the data that is stored.

Authentication should be used whenever you want to know exactly who is using your API. To know which user is currently logged-in, an API needs to store sensitive data - this data will, therefore, be *encrypted*.

#### JSON Web Tokens

JWTs provide a stateless solution to authentication by removing the need to track session data on the server. Instead, JWTs allow us to safely and securely store our session data directly on the client in the form of a JWT.

#### How Authentication works
Before we begin, we need to agree what an authentication pipeline looks like:

1. User POSTs to our server authentication details (over HTTPS): `{ username, password }`
1. The server determines whether the user is who she claims to be
1. If the user’s authentication attempt succeeds, then our server sends some form of data (a token or a session id conventionally) that can be appended to every subsequent request which identifies the user as authenticated or not.

With sessionless auth, the data payload the client receives is our JWT, which should contain an encoded user identifier in JSON format signed by our back-end server. We put the JWT into our cookie or the local-storage.

The is the big difference between JWT auth and auth using sessions is that we don’t keep track of user sessions on the server! So, we have one less data source to worry about in our architecture with sessionless auth.

#### Encryption

When we talk about passwords, the commonly used word is "encryption", although the way passwords are used, most of the time, it is a technique called "hashing". Hashing and Encryption are pretty similar in terms of the processes executed, but the main difference is that hashing is a one-way encryption, meaning that it's very difficult for someone with access to the raw data to reverse it.  


|     | Hashing |   Symmetric Encryption -|  
|-----|---------|-----------------------|
|     |One-way function | Reversible Operation |
|Invertible Operation? |    No, For modern hashing algorithms it is not easy to reverse the hash value to obtain the original input value | Yes, Symmetric encryption is designed to allow anyone with access to the encryption key to decrypt and obtain the original input value |


#### Bcrypt

Hashing is when a function is called on a variable - in this case a password - in order to produce a constant-sized output; it being a one-way function, there isn't a function to reverse or undo a hash and calling the function again - or reapplying the hash - isn't going to produce the same output again.

From another [stack post](http://stackoverflow.com/questions/1602776/what-is-password-hashing):

_"Hashing a password will take a clear text string and perform an algorithm on it (depending on the hash type) to get a completely different value. This value will be the same every time, so you can store the hashed password in a database and check the user's entered password against the hash."_

This prevents you from storing the clear-text passwords in the database (bad idea, _very bad_).

Bcrypt is recognized as one of the most secure ways of encrypting passwords because of the per-password salt. Even with it being slower than any other algorithms, a lot of companies still prefer to use bcrypt for security reasons.

#### But wait, what's a salt?

A salt is random data that can be added as additional input to a one-way function, in our case a one-way function that  hashes a password or passphrase. We use salts to defend against dictionary attacks, a technique for "cracking" an authentication mechanism by trying to determine the decryption key.

#### Passport & Friends

1. passport
2. passport-jwt
3. jsonwebtoken
4. bcrypt

### Lab

**To Do:**

1. Allow authorized user to update their password using bcrypt.
2. Allow unauthorized users to make new accounts by providing username and password which should be hashed.
3. Protect Update and Delete routes based on requesting user.
4. Refactor `/api/login` route to its own routes file.