# Aftermarket API (OEM / OE / IAM)

15 endpoints for cross-reference lookup, vehicle catalog, and article data.

**Base path:** `https://cdn.plan.parts/api/v1/aftermarket`

---

## Health Check

### `GET /health`
**Cost:** Free

Check Aftermarket API availability.

```bash
curl "https://cdn.plan.parts/api/v1/aftermarket/health"
```

---

## Search & Cross-Reference

### `GET /analogs`
**Cost:** 2 tokens

Find OEM and IAM analog articles by article number + brand. Supports IAM numbers, OEM numbers, and EAN codes.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `article` | string | Yes | Article number (IAM, OEM, or EAN) |
| `brand` | string | Yes | Brand name, e.g. `BOSCH`, `MANN` |
| `lang` | string | No | Response language (default: `en`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/analogs?article=F026407123&brand=BOSCH"
```

**Example use case:** Customer asks for Bosch `F026407123` — find all equivalent MANN, MAHLE, WIX parts.

> **Note:** Both `article` and `brand` are required. If brand is unknown, use [`/search`](#get-search) first to identify it.

---

### `GET /articles/{art_id}/crosses`
**Cost:** 2 tokens

OEM and cross-reference numbers for a specific article.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `art_id` | integer | Yes (path) | Article ID from `/search` or `/analogs` |
| `lang` | string | No | Response language |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/articles/12345678/crosses"
```

---

### `GET /search`
**Cost:** 2 tokens

Search articles by number. Accepts IAM article numbers, OEM numbers, and EAN codes.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | Yes | Article/OEM/EAN/IAM code |
| `type` | string | No | Search type: `OEM`, `ART`, `EAN`, `IAM` (default: `ART`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/search?q=03L115562&type=OEM"
```

---

### `GET /search/suggest`
**Cost:** 1 token

Autocomplete suggestions for an article search query. Minimum 2 characters.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `q` | string | Yes | Search query (min 2 characters) |
| `lang` | string | No | Language: `en`, `ru`, `de` (default: `en`) |
| `limit` | integer | No | Max results 1–20 (default: `10`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/search/suggest?q=F026&limit=5"
```

---

## Article Details

### `GET /articles/{art_id}`
**Cost:** 2 tokens

Full article details: name, brand, attributes, images, OEM numbers.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `art_id` | integer | Yes (path) | Article ID |
| `lang` | string | No | Response language |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/articles/12345678"
```

---

### `GET /articles/{art_id}/criteria`
**Cost:** 1 token

Technical specifications and fitment criteria for an article.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `art_id` | integer | Yes (path) | Article ID |
| `lang` | string | No | Response language |

---

### `GET /articles/{art_id}/media`
**Cost:** 1 token

Images and documents for an article.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `art_id` | integer | Yes (path) | Article ID |

---

### `GET /articles/{art_id}/vehicles`
**Cost:** 2 tokens

Compatible vehicles (fitments/linkages) for an aftermarket article. Supports pagination.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `art_id` | integer | Yes (path) | Article ID |
| `lang` | string | No | Language (default: `en`) |
| `page` | integer | No | Page number (default: `1`) |
| `per_page` | integer | No | Results per page, max 100 (default: `50`) |

---

## Vehicle Catalog

### `GET /ktype`
**Cost:** 1 token

Search vehicles by brand, model, engine, year, or K-Type identifier.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `brand` | string | No | Vehicle brand, e.g. `VW`, `BMW` |
| `model` | string | No | Model name |
| `year` | integer | No | Production year |
| `ktype` | integer | No | K-Type ID (if already known) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/ktype?brand=VW&model=Golf&year=2018"
```

---

### `GET /catalog/brands`
**Cost:** 1 token

List of all vehicle manufacturers. Returns ~708 brands.

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/aftermarket/catalog/brands"
```

---

### `GET /catalog/models`
**Cost:** 1 token

Model series for a manufacturer.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mfa_id` | integer | Yes | Manufacturer ID from `/catalog/brands` |

---

### `GET /catalog/modifications`
**Cost:** 1 token

Vehicle modifications for a model series.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ms_id` | integer | Yes | Model series ID from `/catalog/models` |

---

### `GET /catalog/categories`
**Cost:** 1 token

Part categories available for a specific vehicle.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ktype` | integer | Yes | K-Type vehicle ID |

---

### `GET /catalog/articles`
**Cost:** 2 tokens

Spare parts list for a vehicle category.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ktype` | integer | Yes | K-Type vehicle ID |
| `pt_id` | integer | Yes | Part type / category ID from `/catalog/categories` |

---

## IAM vs OEM — Key Distinction

| Type | Issued by | Example |
|------|-----------|---------|
| **IAM** | Aftermarket brand (Bosch, MANN, Febi…) | `F026407123` — Bosch oil filter |
| **OEM** | Vehicle manufacturer (VW, BMW…) | `03L115562` — VW original number |

**Rule:** use `/analogs` for IAM article numbers; use `/search` to resolve OEM numbers first.

Read more: [plan.parts/blog/aftermarket-crosses-iam-vs-oem](https://plan.parts/blog/aftermarket-crosses-iam-vs-oem)

---

## Typical Workflows

### Find analogs for a known IAM part

```
GET /analogs?article=F026407123&brand=BOSCH
  → list of analog art_ids + brands (MANN, MAHLE, WIX...)

GET /articles/{art_id}/crosses
  → OEM numbers linked to this analog
```

### Look up parts for a vehicle

```
GET /catalog/brands
  → pick mfa_id for VW

GET /catalog/models?mfa_id=...
  → pick ms_id for Golf

GET /catalog/modifications?ms_id=...
  → pick ktype for Golf 2.0 TDI 2018

GET /catalog/categories?ktype=...
  → pick pt_id for "Oil Filters"

GET /catalog/articles?ktype=...&pt_id=...
  → list of oil filter articles for this car
```

---

← [Back to v1 index](README.md)
