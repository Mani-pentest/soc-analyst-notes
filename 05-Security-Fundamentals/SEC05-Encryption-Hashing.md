# SEC05: Encryption & Hashing

This chapter covers the distinction between encryption and hashing, password verification mechanics, salting, and the difference between symmetric and asymmetric encryption — including how HTTPS combines both.

## Encryption vs Hashing

| Property | Encryption | Hashing |
|---|---|---|
| Direction | Two-way (reversible with the correct key) | One-way (irreversible by design) |
| Purpose | Recover original data | Verify data or credentials without storing the original |
| Example | BitLocker (see WIN03) | NetNTLM password hashes (see AD01) |

## How Password Verification Works Without Storing Passwords

A login system never stores a user's actual password, and it never "reverses" a stored hash. Instead:

1. When a password is first set, the system hashes it and stores only the resulting hash.
2. When the user logs in again, the system hashes the newly entered password using the same method.
3. The newly generated hash is compared against the stored hash — a match confirms the correct password was entered.

## Salting

**Note:** Because hashing is deterministic, identical passwords produce identical hashes. This creates a real vulnerability: if two users share a password, an attacker who steals the password database can immediately identify this, and precomputed lookup tables ("rainbow tables") can instantly reverse-lookup common password hashes without brute-forcing them. Salting mitigates this by appending a random, unique value to each password before hashing, ensuring that identical passwords produce different stored hashes per user and rendering rainbow tables ineffective.

## NetNTLM vs Kerberos — Why Kerberos Is Preferred

**Note:** NetNTLM's security weakness is not primarily about missing salts — it stems from an outdated, computationally weak hash format, and from vulnerability to pass-the-hash attacks, where an attacker can authenticate by replaying a stolen hash directly, without needing to crack it. Kerberos (see AD01) avoids this class of vulnerability entirely by using a ticket-based authentication model (TGT/TGS), which does not rely on transmitting reusable password hashes across the network for ongoing authentication.

## Symmetric vs Asymmetric Encryption

| Type | Key Structure | Speed | Best Use Case |
|---|---|---|---|
| Symmetric | Single shared key for both encryption and decryption | Fast | Situations where both parties already share a secret key |
| Asymmetric | Public key (shareable) paired with a private key (kept secret) | Slower | Establishing trust between parties with no prior shared secret |

## HTTPS: A Hybrid Approach

**Note:** HTTPS (see CH07 - HTTP/HTTPS) uses both encryption types in sequence. Asymmetric encryption is used only during the initial TLS handshake, allowing two parties with no prior relationship to securely agree on a temporary shared secret using the server's public key. Once this shared secret is established, communication switches to symmetric encryption for the remainder of the session, since symmetric encryption is significantly faster for ongoing data transfer.

## Why It Matters

This chapter corrects a common misconception (NetNTLM's weakness being about salting) and explains why real-world systems don't pick one encryption approach exclusively — HTTPS's hybrid model is the practical answer to symmetric encryption's speed vs asymmetric encryption's trust-establishment tradeoff. Understanding hashing/salting also underpins why leaked password databases with unsalted hashes are catastrophic, while salted ones are far more resistant to mass cracking.

## Summary

- Encryption is reversible with the right key; hashing is one-way and used to verify without storing the original
- Passwords are verified by re-hashing the input and comparing to the stored hash — never by "unhashing"
- Salting adds a random unique value per password before hashing, defeating rainbow tables and hiding shared passwords
- NetNTLM's real weakness is an outdated hash format vulnerable to pass-the-hash attacks, not a lack of salting
- Kerberos avoids this entirely via ticket-based (TGT/TGS) authentication rather than transmitting reusable hashes
- Symmetric encryption is fast but requires a pre-shared key; asymmetric is slower but solves the no-prior-secret problem
- HTTPS uses asymmetric encryption only for the initial handshake, then switches to symmetric for speed during the actual session
