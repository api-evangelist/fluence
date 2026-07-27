---
name: Deploy a Fluence virtual machine
description: Find a hardware configuration on the Fluence decentralized compute marketplace, price it, register an SSH key, and deploy a VM.
api: openapi/fluence-openapi-original.yml
operations: [get_vm_configurations, get_vm_prices, cost, create_users_ssh_key, create_users_vm, get_users_vm]
---

# Deploy a Fluence virtual machine

Base URL: `https://api.fluence.dev`. Authenticate every request with the
`X-API-KEY: <your key>` header (create keys at
https://console.fluence.network/settings/api-keys). The API key must carry the
`vms:write` and `ssh_key:create` permission scopes.

## Steps

1. **Pick a hardware configuration** — `GET /v1/configurations/virtual_machines`
   (`get_vm_configurations`) to list available VM configurations (CPU/RAM/GPU).
2. **Price it** — `GET /v1/prices/vm` (`get_vm_prices`), or `POST /v1/prices/cost`
   (`cost`) to estimate the full cost of a configuration before committing.
3. **Register an SSH key** — `POST /v1/ssh_keys` (`create_users_ssh_key`) with your
   public key so you can log in to the instance. Skip if the key already exists.
4. **Create the VM** — `POST /v2/vms` (`create_users_vm`) with the configuration,
   chosen cluster/region, SSH key, and any storage/network settings. Send
   `Content-Type: application/json`.
5. **Confirm it is running** — `GET /v2/vms/{vm_id}` (`get_users_vm`) and poll until
   the VM reports it is ready (`readySince` set).

## Rules

- Requests and responses are JSON. There is **no idempotency key** — do not blindly
  retry `create_users_vm`; on a network error, first `GET /v2/vms` (`get_users_vms`)
  to check whether the VM was already created.
- Compute is billed in USDC against your account balance. If the balance is too low
  the API returns error code `insufficient_balance` — top up before deploying.
- On any non-2xx response, branch on the JSON `code` field (not the `error` text).
  See `errors/fluence-error-codes.yml`.
