# Authentication

- Verifying who a user actually is
- Authenticate often with username and password
- could be more advanced such as facial recognition

---

# Authorization

- Verifying the access of a certain user such as user vs admin.

---

- use a hashing function to store passwords.

# Cryptographic hashing functions

- one way , no invert
- small input change = larger output change
- same input = same output
- unlikely same value for 2 outputs
- hash functions are deliberately slow - prevent brute force attack from being a feasible attack

---

# Password salts - extra safety

- lots of people use same password to each other and themselves.
- adds a random value to the password to differentiate it from the same common passwords as others in their hash function used.
- concatenate to the password

---

- hacker gains access to the hashed passwords

---

- sees multiple same hash and knows bcrypt used then brute force common passwords.

- use a reverse lookup table to find all other passwords that match the one password they find to break multiple accounts at once. 

# Passport

https://www.npmjs.com/package/passport