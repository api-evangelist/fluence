---
name: Find and price Fluence compute
description: Discover available clusters and hardware resources on the Fluence marketplace and estimate cost before deploying.
api: openapi/fluence-openapi-original.yml
operations: [get_bulk_resources, get_available_resources, get_vm_configurations, get_vm_prices, cost]
---

# Find and price Fluence compute

Base URL: `https://api.fluence.dev`. Authenticate with `X-API-KEY`. Read paths require
the `clusters:read` and `prices:read` permission scopes.

## Steps

1. **List clusters** — `GET /v1/clusters` (`Get available clusters`) to enumerate the
   compute clusters / regions on the marketplace.
2. **Check available resources** — `GET /v1/clusters/resources` (`get_bulk_resources`)
   across clusters, or `GET /v1/clusters/{cluster_id}/resources`
   (`get_available_resources`) for one cluster's current capacity.
3. **List VM configurations** — `GET /v1/configurations/virtual_machines`
   (`get_vm_configurations`) for the CPU/RAM/GPU shapes you can deploy.
4. **Get prices** — `GET /v1/prices/vm` (`get_vm_prices`); also `GET /v1/prices/storage`
   (`get_storage_prices`) and `GET /v1/prices/public-ip` (`get_public_ip_prices`).
5. **Estimate total cost** — `POST /v1/prices/cost` (`cost`) with the full desired
   configuration to get the hourly/monthly USDC estimate before committing.

## Rules

- List responses are page-numbered: use `page` and `perPage`, and read
  `PaginationInfo` (`totalRecords`, `totalPages`, `currentPage`) from the response.
- Prices and cost are quoted in USDC. Read paths are safe/read-only.
