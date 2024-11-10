# SHA 
**SHA** (Secure Hash Algorithm) is a set of cryptographic hash functions designed to produce a unique, fixed-size hash from input data of any size. Hashes are useful for ensuring data integrity and securing sensitive data, as they create a "fingerprint" of the data that cannot be reversed back to the original data.

# Key Points About SHA
- Hash Functions: SHA functions are one-way algorithms that generate a unique, fixed-length hash (also called a "digest") for each unique input.
- Versions of SHA:
  - SHA-1: Produces a 160-bit hash but is now considered weak due to vulnerabilities.
  - SHA-2: Offers more secure versions like SHA-224, SHA-256, SHA-384, and SHA-512.
  - SHA-3: The latest version, providing additional security and flexibility.
 
## Common Use Cases in NestJS
1. **Password Hashing**: Storing user passwords as hashes instead of plain text for security.
2. **Data Integrity Verification**: Ensuring that data hasn't been tampered with by comparing hashes.
3. **Digital Signatures**: Generating a hash of data to verify authenticity.

### Implementing SHA in NestJS
While SHA alone can be used to create hashes, NestJS developers often use libraries like `bcrypt` (which incorporates salt) for password hashing, as SHA functions are not inherently designed for secure password storage.

## Best Practices for Hashing Passwords
- **Use Salt**: SHA functions alone do not add salt, making them vulnerable to rainbow table attacks. Consider bcrypt or argon2 for password hashing, which include salting mechanisms.
- **Avoid SHA-1**: Use SHA-2 (e.g., SHA-256) or SHA-3 for stronger security.
- **Apply Hashing for Data Integrity**: For data verification, SHA-256 or SHA-3 can be used to confirm data hasn't been altered.

## Recommended Libraries in NestJS
For secure password hashing, consider:

- **bcrypt**: Adds salt and is secure for password storage.
- **argon2**: A secure hashing algorithm also suited for password storage.
