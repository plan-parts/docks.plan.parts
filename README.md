# plan.parts — Developer Documentation

Official documentation for integrating with the [plan.parts](https://plan.parts) platform — an API for OEM and IAM auto parts cross-references, vehicle catalogs, and price intelligence.

---

## What is plan.parts?

**plan.parts** is a pay-per-request API platform for the automotive aftermarket. It helps auto parts distributors, online stores, and developers to:

- Find **OEM ↔ IAM analog parts** across brands (Bosch, MANN, MAHLE, Febi, etc.)
- Look up parts by **vehicle** (brand → model → modification → category)
- Resolve **OEM numbers** (e.g. VW `03L115562`) into aftermarket analogs
- Access **article details**, images, technical specs, and fitment data

> **Token-based pricing.** Most endpoints cost 1–2 tokens per request. A health check endpoint is always free.

---

## Getting Started

1. **Register** at [plan.parts/register](https://plan.parts/register) to create an account.
2. Go to **API Settings** and generate a personal access token.
3. Add it to your requests as a Bearer token.

```http
Authorization: Bearer YOUR_TOKEN_HERE
```

---

## Base URL

```
https://plan.parts/api/v1/aftermarket
```

---

## Authentication

All endpoints (except `/health`) require a Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
     "https://plan.parts/api/v1/aftermarket/health"
```

**Error responses:**

| HTTP Status | Meaning |
|-------------|---------|
| `401` | Invalid or missing token |
| `402` | Insufficient token balance |
| `429` | Rate limit exceeded |

---

## API Overview

### Utility

| Endpoint | Cost | Description |
|----------|------|-------------|
| `GET /health` | Free | Check API availability |

### Search & Cross-Reference

| Endpoint | Cost | Description |
|----------|------|-------------|
| `GET /search` | 2 tokens | Search by article number, OEM, EAN, or IAM |
| `GET /search/suggest` | 1 token | Autocomplete suggestions |
| `GET /analogs` | 2 tokens | Find OEM/IAM analogs by article + brand |
| `GET /articles/{art_id}/crosses` | 2 tokens | OEM and cross-reference numbers for an article |
| `GET /articles/{art_id}` | 2 tokens | Full article details, name, brand, OEM numbers |

### Article Data

| Endpoint | Cost | Description |
|----------|------|-------------|
| `GET /articles/{art_id}/criteria` | 1 token | Technical specs and fitment criteria |
| `GET /articles/{art_id}/media` | 1 token | Images and documents |
| `GET /articles/{art_id}/vehicles` | 2 tokens | Compatible vehicles and fitments |

### Vehicle Catalog

| Endpoint | Cost | Description |
|----------|------|-------------|
| `GET /catalog/brands` | 1 token | Vehicle manufacturer list (708 brands) |
| `GET /catalog/models` | 1 token | Model series by manufacturer |
| `GET /catalog/modifications` | 1 token | Vehicle modifications per model |
| `GET /catalog/categories` | 1 token | Part categories for a vehicle |
| `GET /catalog/articles` | 2 tokens | Spare parts list by vehicle + category |
| `GET /ktype` | 1 token | Vehicle lookup by brand, model, engine, year |

Full interactive documentation: [plan.parts/docs/aftermarket-oem-oe-iam](https://plan.parts/docs/aftermarket-oem-oe-iam)

---

## Quick Examples

### 1. Find analogs for a Bosch oil filter

```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/analogs?article=F026407123&brand=BOSCH"
```

**Result:** MANN `W712/95`, MAHLE `OC 229`, WIX `WL7374` — all share the same OEM cross-reference (`03L115562`).

### 2. Resolve an OEM number to aftermarket analogs

```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/search?article=03L115562"
```

### 3. Get cross-references for an article

```bash
# First: get art_id from /search or /analogs response
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/articles/{art_id}/crosses"
```

### 4. Look up parts for a VW Golf 2.0 TDI

```bash
# Step 1: find K-Type by vehicle parameters
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/ktype?brand=VW&model=Golf&engine=2.0+TDI"

# Step 2: get categories for that vehicle
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/catalog/categories?ktype=12345"

# Step 3: get articles for a category
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  "https://plan.parts/api/v1/aftermarket/catalog/articles?ktype=12345&category_id=678"
```

---

## IAM vs OEM Numbers — Key Distinction

Understanding this is essential for correct API usage:

| Type | Issued by | Example | Use with |
|------|-----------|---------|----------|
| **IAM** | Aftermarket brand (Bosch, MANN, Febi…) | `F026407123` (Bosch) | `/analogs`, `/crosses` |
| **OEM** | Vehicle manufacturer (VW, BMW, Toyota…) | `03L115562` (Volkswagen) | `/search?article=...` |

**Common mistake:** sending an OEM number to `/analogs` or `/crosses`. Use `/search` first to identify the article type and resolve it to an `art_id`.

Read more: [plan.parts/blog/aftermarket-crosses-iam-vs-oem](https://plan.parts/blog/aftermarket-crosses-iam-vs-oem)

---

## Versioning

This documentation is versioned alongside the plan.parts API. Each update is tagged with the release version.

| Version | Date | Notes |
|---------|------|-------|
| `v1.0` | 2024-10 | Initial documentation release |

---

## SDKs & Integrations

| Package | Language | Description |
|---------|----------|-------------|
| [oem-iam-parts-cross-matcher](https://github.com/plan-parts/oem-iam-parts-cross-matcher) | PHP / Laravel | Upload price lists and find analogs via plan.parts API |

---

## Support

- Register and open a ticket: [plan.parts/register](https://plan.parts/register)
- API playground: [plan.parts/docs/aftermarket-oem-oe-iam](https://plan.parts/docs/aftermarket-oem-oe-iam)
- Website: [plan.parts](https://plan.parts)

---

## License

Documentation is provided as-is. The plan.parts API is a commercial service — see [plan.parts](https://plan.parts) for terms and pricing.
