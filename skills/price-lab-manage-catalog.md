---
name: Manage the Price Lab product catalog
description: Create and organize product categories and products, then apply batch price and cost updates.
api: openapi/price-lab-openapi.yml
operations: [listCategories, createCategory, listProducts, createProduct, updateProduct, updatePriceBatch, updateCostBatch, updatePriceAndCostBatch]
generated: '2026-07-20'
method: generated
---

# Manage the Price Lab product catalog

Grounded in the Price Lab REST API (`https://backend.pricelab.com.pe/api`). Authenticate first — see `price-lab-authenticate.md`.

## Steps

1. **Ensure categories exist** — `listCategories` (`GET /categoryproducts/`); create missing ones with `createCategory` (`POST /categoryproducts/`).
2. **List existing products** — `listProducts` (`GET /products/`) to reconcile against your source system.
3. **Create or edit products** — `createProduct` (`POST /products/`) for new SKUs; `updateProduct` (`PUT /products/{id}/`) to edit one.
4. **Apply price/cost changes in bulk** — for catalog-wide updates use the batch endpoints instead of per-product calls:
   - `updatePriceBatch` (`POST /products/update_price_product_for_lote/`) — prices only
   - `updateCostBatch` (`POST /products/update_cost_product_for_lote/`) — costs only
   - `updatePriceAndCostBatch` (`POST /products/update_price_cost_for_lote/`) — both together

## Rules

- Prefer the `*_for_lote` batch endpoints for volume changes; they are not documented as idempotent, so do not blindly retry a batch on timeout — verify with `listProducts` first (see `conventions/price-lab-conventions.yml`).
- `400` indicates a schema/validation problem with the payload; `401` means refresh your token.
- Entity relationships (Product → Category) are described in `data-model/price-lab-data-model.yml`.
