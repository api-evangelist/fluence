---
name: Manage Fluence SSH keys
description: List, register, and delete the SSH keys used to access Fluence virtual machines.
api: openapi/fluence-openapi-original.yml
operations: [get_users_ssh_keys, create_users_ssh_key, delete_users_ssh_key]
---

# Manage Fluence SSH keys

Base URL: `https://api.fluence.dev`. Authenticate with `X-API-KEY`. Requires the
`ssh_key:list`, `ssh_key:create`, and `ssh_key:remove` permission scopes.

## Steps

1. **List existing keys** — `GET /v1/ssh_keys` (`get_users_ssh_keys`) to see registered
   keys with their `fingerprint` and `algorithm`; check before creating a duplicate.
2. **Register a key** — `POST /v1/ssh_keys` (`create_users_ssh_key`) with a `name` and
   the `publicKey`. The response returns the key `id` and `fingerprint`.
3. **Delete a key** — `DELETE /v1/ssh_keys/{ssh_key_id}` (`delete_users_ssh_key`), or
   bulk-delete with `POST /v1/ssh_keys/delete` (`delete_users_ssh_keys`).

## Rules

- Register the SSH key **before** creating a VM so it can be injected at boot.
- JSON in, JSON out. On errors branch on the `code` field (see
  `errors/fluence-error-codes.yml`); a missing/invalid `X-API-KEY` returns `forbidden`.
