2025-06-09 18:22

Status: #Leaf 

Tags: #ProgrammingConcepts #Algorithms #Cybersecurity

# Hashing
**Hashing**: The process of converting input data (of any size) into a fixed-length string of characters using an algorithm called a **hash function**.
- **Deterministic:** The same input data will always produce the same hash
- **One-Way**: You cannot reverse a hash to get its original input
- **Fixed Output Size:** Regardless of the input, the output hash will always be the same length
	- Ex: The SHA-256 algorithm always produces a 256 bit hash, whether the input is 1kb or 100
## Password Hashing
**Problem Scenario:** We have a server with a login system and users with passwords. If we store the passwords locally, malicious users who gain access would be able to see all the passwords. We can prevent them by **hashing** the passwords and storing the hash, rather than the original password. 

**Process**
1) User creates an account with a new password
2) Server runs hashing algorithm on password and stores the hash only
3) User enters password to log on
4) Server runs hashing algorithm on entered password and checks to see if it matches the server-side hash
### Salting and Rainbow Tables
Because hashing is *deterministic* it will always produce the same hash. This means that two users with the same password will produce an identical hash. If a malicious user is viewing the hashes, they can see which users share a hash and know that they have the same password. Or worse, they can calculate hashes themselves and see if they match users.

**Rainbow Table Attack**: Using a precomputed list of common hashes to crack common passwords
1) An attacker creates a large list of common passwords (`password`, `abc123`)
2) They hash the list using the same hashing function, creating a table of common passwords and their respective hashes
3) They gain access to a database of password hashes. They can then see if any hashes match their rainbow table, easily cracking common passwords

**Salting**: Adding a unique "salt" value to each password before hashing to ensure uniqueness
1) User creates an account with a new password
2) Server generates a random salt, appends it to the password, and then runs the hashing algorithm
3) The server stores the salt value and the hashed password
4) User enters password to log on
5) Server fetches salt value for user, appends it to password, then runs hashing algorithm to verify it's the correct password
## References
