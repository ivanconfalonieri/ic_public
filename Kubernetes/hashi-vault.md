# HashiCorp Vault – Token and AppRole Security Guide

## 1. Introduction

HashiCorp Vault provides a secure way to manage secrets and credentials.
Tokens are the primary mechanism for authentication and authorization, while AppRole is designed for machine-to-machine authentication.

This document explains:

* Vault tokens and their use
* AppRole authentication
* Dynamic security mechanisms
* Best practices for secure implementation

---

## 2. Vault Tokens

* Tokens are temporary credentials used to interact with Vault.
* Each token is associated with **policies** that define its permitted actions.
* Anyone with a valid token can perform the actions allowed by its policy.
* Tokens have a **time-to-live (TTL)** and can optionally be renewable.
* Shorter TTLs reduce exposure risk if a token is compromised.

---

## 3. AppRole Authentication

AppRole allows scripts or applications to obtain a Vault token without human intervention.

### How it works:

1. The AppRole is configured with a `role_id` (public) and a `secret_id` (sensitive).
2. The script or application authenticates with Vault using these credentials.
3. Vault returns a **client token** with limited TTL and permissions defined by policy.

### Security Considerations:

* If someone obtains both `role_id` and `secret_id`, they can request a token and access Vault with the associated permissions.
* Best practices for AppRole:

  * Use **one-time SecretIDs**.
  * Configure **short TTLs** for tokens and SecretIDs.
  * Bind to specific IP ranges using **bound CIDR**.
  * Assign **minimal policies** (principle of least privilege).
  * Enable **audit logging** for all authentications and token usage.

---

## 4. Dynamic Security Mechanisms

Vault provides dynamic methods to reduce the risk of static credentials.

### Dynamic Secrets

* Vault can generate temporary credentials for databases, cloud services, and other systems.
* Credentials automatically expire after TTL, reducing the risk of compromise.

### Response Wrapping

* Secrets or tokens can be distributed via **wrapping tokens** with very short TTLs.
* The script unwraps the token to obtain the actual credential, minimizing exposure.

### Dynamic Authentication Methods

* **Kubernetes Auth**: Pods authenticate via service accounts.
* **OIDC/JWT Auth**: Apps authenticate using an identity provider (Keycloak, Okta, Azure AD).
* **AWS/GCP Auth**: Instances authenticate using cloud metadata tokens.
* **TLS Certificates**: Authenticate using client certificates.

### Leasing and Renewal

* Each token or secret has a **lease**.
* Tokens and secrets can be renewed if required, or they are revoked automatically at expiration.

---

## 5. Best Practices

* Never hardcode tokens or SecretIDs in scripts.
* Prefer **dynamic authentication** methods over static credentials.
* Assign **least privilege policies**.
* Enable **audit logging** for full traceability.
* Use **short TTLs** and one-time SecretIDs for AppRole.
* Rotate SecretIDs regularly and revoke tokens when no longer needed.

---

## 6. Example Usage

### Bash Script Example (AppRole Login)

```bash
VAULT_ADDR="http://127.0.0.1:8200"
ROLE_ID="xxx"
SECRET_ID="yyy"

VAULT_TOKEN=$(curl -s --request POST \
  --data "{\"role_id\":\"$ROLE_ID\",\"secret_id\":\"$SECRET_ID\"}" \
  $VAULT_ADDR/v1/auth/approle/login | jq -r '.auth.client_token')

curl -s --header "X-Vault-Token: $VAULT_TOKEN" \
  $VAULT_ADDR/v1/secret/data/my-secret | jq .
```

### Python Example (AppRole Login)

```python
import hvac

client = hvac.Client(url="http://127.0.0.1:8200")

role_id = "xxx"
secret_id = "yyy"
login = client.auth.approle.login(role_id=role_id, secret_id=secret_id)

client.token = login['auth']['client_token']
secret = client.secrets.kv.read_secret_version(path="my-secret")

print(secret["data"]["data"])
```

---

## 7. Official Resources

* [AppRole Auth Method](https://developer.hashicorp.com/vault/docs/auth/approle)
* [AppRole Best Practices](https://developer.hashicorp.com/vault/docs/auth/approle/approle-pattern)
* [Dynamic Secrets](https://developer.hashicorp.com/vault/tutorials/get-started/understand-static-dynamic-secrets)

---
