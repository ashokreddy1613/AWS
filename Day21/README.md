# Secret Management

Secret management is the secure storage, retrieval, rotation, and auditing of sensitive data like:

- API keys
- Database passwords
- OAuth tokens
- SSH keys
- TLS certificates

## System Manager
- It's easy to retrieve the information, simple to use
- Store parameters and secrets

## Secret Manager
- Use for store certificates,passwords of DB
- Store, auto-rotate, and audit secrets (e.g., DB creds)

- You can integrate with Lambda to write a function to rotate secrets 

## KMS (Key Management Service)
Encrypt secrets at rest using customer-managed keys

## Hashcorp Vault
- Its a centralized solution which you can use with any cloud.
- dedicated open source platform community backup
- It offers many additional features