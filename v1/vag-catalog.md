# VAG Catalog (ETKA)

13 endpoints for Volkswagen Group OEM parts data — VW, Audi, Skoda, SEAT, Porsche.

**Base path:** `https://plan.parts/api/v1/vag`

---

## Health Check

### `GET /health`
**Cost:** Free

Check VAG Catalog API availability.

```bash
curl "https://plan.parts/api/v1/vag/health"
```

---

## Catalog & Vehicles

### `GET /catalogs`
**Cost:** Free

List of available VAG catalogs.

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/catalogs"
```

Returns: VW, Audi, Skoda, SEAT, Porsche catalog identifiers.

---

### `GET /vehicles`
**Cost:** 1 token

Search vehicles by catalog, model, year, or text query.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `catalog` | string | No | Catalog ID from `/catalogs` |
| `model` | string | No | Model name |
| `year` | integer | No | Production year |
| `q` | string | No | Free-text search query |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/vehicles?catalog=VW&model=Golf&year=2018"
```

---

### `GET /vin/{vin}`
**Cost:** 1 token

Decode a VIN to vehicle specification.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `vin` | string | Yes (path) | 17-character VIN |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/vin/WVWZZZ1JZXW000001"
```

---

## Part Groups & Navigation

### `GET /vehicle/{epis_typ}/groups`
**Cost:** 1 token

List of part groups for a vehicle.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | string | Yes (path) | Vehicle type ID from `/vehicles` |

---

### `GET /vehicle/{epis_typ}/subgroups`
**Cost:** 1 token

List of part subgroups for a specific group.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | string | Yes (path) | Vehicle type ID |
| `group_id` | integer | Yes (query) | Group ID from `/groups` |

---

### `GET /vehicle/{epis_typ}/parts`
**Cost:** 2 tokens

List of OEM parts for a vehicle subgroup.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | string | Yes (path) | Vehicle type ID |
| `subgroup_id` | integer | Yes (query) | Subgroup ID from `/subgroups` |

---

## Part Details

### `GET /part/{number}`
**Cost:** 1 token

OEM part details: name, status, synonyms, replacements.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number, e.g. `03L115562` |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562"
```

---

### `GET /part/{number}/cross`
**Cost:** 2 tokens

Cross-reference numbers (IAM analogs) for an OEM part.

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562/cross"
```

---

### `GET /part/{number}/vehicles`
**Cost:** 1 token

List of vehicles that use a specific OEM part.

---

### `GET /part/{number}/price`
**Cost:** 1 token

Official OEM price for a part.

---

### `GET /part/{number}/images`
**Cost:** 1 token

Images for an OEM part.

---

### `GET /part/{number}/supersessions`
**Cost:** 1 token

Successor and predecessor part numbers (replacement chain).

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562/supersessions"
```

---

## Typical Workflow

```
GET /vag/catalogs
  → get catalog ID for "VW"

GET /vag/vehicles?catalog=VW&model=Golf&year=2018
  → get epis_typ for the vehicle

GET /vag/vehicle/{epis_typ}/groups
  → pick group "Engine"

GET /vag/vehicle/{epis_typ}/subgroups?group_id=...
  → pick subgroup "Oil Filter"

GET /vag/vehicle/{epis_typ}/parts?subgroup_id=...
  → get OEM part number, e.g. 03L115562

GET /vag/part/03L115562
  → name, status, replacements

GET /vag/part/03L115562/cross
  → IAM analogs: BOSCH F026407123, MANN W712/95...

GET /vag/part/03L115562/price
  → official dealer price
```

---

← [Back to v1 index](README.md)
