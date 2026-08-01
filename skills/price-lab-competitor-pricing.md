---
name: Run competitor pricing and recommendations on Price Lab
description: Load competitor prices and policies, export scraped prices, and accept or reject the resulting price recommendations.
api: openapi/price-lab-openapi.yml
operations: [importRecordsFile, createPricingPolicies, exportScrapedPrices, exportPriceBySku, exportRecommendations, respondToRecommendation]
generated: '2026-07-20'
method: generated
---

# Run competitor pricing and recommendations on Price Lab

Grounded in the Price Lab REST API (`https://backend.pricelab.com.pe/api`). Authenticate first — see `price-lab-authenticate.md`.

## Steps

1. **Ingest competitor prices** — `importRecordsFile` (`POST /recordsimportfiles/`) uploads a records file; this same endpoint ingests sales transactions, stock, replenishment, offers, and competitor prices depending on the record type.
2. **Define competitive policies** — `createPricingPolicies` (`POST /pricescrapingrules/create_policies/`) loads the rules that match your SKUs to competitor references.
3. **Review scraped data** — `exportScrapedPrices` (`GET /pricescrapings/export_data_scraping/`) or `exportPriceBySku` (`GET /pricescrapings/`) to pull the current competitor price landscape.
4. **Pull recommendations** — `exportRecommendations` (`GET /pricemachines/export_for_batch/`) to get the engine's suggested prices for a batch.
5. **Act on recommendations** — `respondToRecommendation` (`POST /pricemachines/response_recommendation/`) to accept or reject each recommendation.

## Rules

- Bulk ingestion and export are file/batch oriented; poll the export endpoints after import rather than assuming synchronous processing.
- Accepting a recommendation is a state-changing (`acting`) operation — see `agentic-access/price-lab-agentic-access.yml` for the recommended execution contract.
- `401` means refresh your JWT (`refreshToken`); `400` indicates a malformed file or policy payload.
