# 🧺 dyrt-storage

Public, stateless steganographic storage backend for **[Dirty Laundry (Dyrt)](https://github.com/zjweiss/dirty-laundry)**.

This repository serves as an external, quota-free, zero-maintenance storage tier for Dirty Laundry running on serverless platforms (e.g. Google Cloud Run).

## 📁 Directory Structure & Sharding

To prevent hitting GitHub's **3,000 files/directory** limit, containers are automatically sharded across two levels of directory prefixes based on the SHA-256 hash of the owner username:

```
images/
└── <prefix1>/           # e.g., '3f/'
    └── <prefix2>/       # e.g., '8a/'
        └── <hash>.png   # Steganographic image container
```

## 🔒 Security & Data Integrity

- **Encrypted Payloads**: Images in this repository contain AES-256-GCM AEAD encrypted ciphertext. No plaintext secrets are ever stored.
- **Random Noise Padding**: Unused canvas bytes are padded with cryptographically secure pseudo-random bytes (`secrets.token_bytes()`), ensuring each image exhibits maximum Shannon entropy ($\ge 7.95$ bits/byte) resembling white noise.
- **Stateless & Decoupled**: The Dirty Laundry application server retains zero database state or credentials; secrets are retrieved and decrypted on demand via client-supplied passwords.
- **Direct CDN Serving**: Images are served publicly to the viewing gallery via `https://raw.githubusercontent.com/zjweiss/dyrt-storage/main/images/...`.
