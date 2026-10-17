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

Returns: `VW`, `AU` (Audi), `SK` (Skoda), `SE` (SEAT), `PO` (Porsche), `ML` (MAN).

---

### `GET /vehicles`
**Cost:** 1 token

Search vehicles by catalog, model, year, or text query.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `catalog` | string | **Yes** | Catalog ID: `AU`, `VW`, `SK`, `SE`, `PO`, `ML` |
| `model` | string | No | Model name filter (also accepted as `modell`) |
| `year` | integer | No | Production year filter |
| `q` | string | No | Free-text search query |
| `limit` | integer | No | Max results 1–500 (default: 100) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/vehicles?catalog=VW&model=Golf&year=2018"
```

---

### `GET /vin/{vin}`
**Cost:** 2 tokens

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

List of main part groups for a vehicle.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | integer | Yes (path) | Vehicle type ID from `/vehicles` |
| `catalog` | string | **Yes** | Catalog ID: `AU`, `VW`, `SK`, `SE`, `PO`, `ML` |
| `markt` | string | No | Market code (default: `RDW`) |
| `lang` | string | No | Language: `DE`, `EN`, `RU` (default: `EN`) |

---

### `GET /vehicle/{epis_typ}/subgroups`
**Cost:** 1 token

List of part subgroups. Returns `hg_ug` and `bildtafel2` values needed for `/parts`.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | integer | Yes (path) | Vehicle type ID from `/vehicles` |
| `catalog` | string | **Yes** | Catalog ID: `AU`, `VW`, `SK`, `SE`, `PO`, `ML` |
| `hg` | integer | No | Main group code from `/groups` (filter by group) |
| `dir_name` | string | No | Direction/section code (default: `R`) |
| `lang` | string | No | Language: `DE`, `EN`, `RU` (default: `EN`) |

---

### `GET /vehicle/{epis_typ}/parts`
**Cost:** 2 tokens

List of OEM parts for a vehicle subgroup. Use values from `/subgroups` response.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `epis_typ` | integer | Yes (path) | Vehicle type ID from `/vehicles` |
| `catalog` | string | **Yes** | Catalog ID: `AU`, `VW`, `SK`, `SE`, `PO`, `ML` |
| `hg_ug` | string | **Yes** | Subgroup code from `/subgroups` (e.g. `10-2`) |
| `bildtafel2` | integer | **Yes** | Diagram ID from `/subgroups` |
| `dir_name` | string | No | Direction/section code (default: `R`) |
| `lang` | string | No | Language: `DE`, `EN`, `RU` (default: `EN`) |

---

## Part Details

### `GET /part/{number}`
**Cost:** 1 token

OEM part details: name, status, synonyms, replacements.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number, e.g. `03L115562` |
| `lang` | string | No | Language: `DE`, `EN`, `RU` (default: `EN`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562"
```

---

### `GET /part/{number}/cross`
**Cost:** 2 tokens

Cross-reference numbers (IAM analogs) for an OEM part.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number |
| `limit` | integer | No | Max results 1–500 (default: 200) |
| `type` | string | No | Filter by type: `OEM`, `IAM`, `OE`, `EAN` |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562/cross?type=IAM&limit=50"
```

---

### `GET /part/{number}/vehicles`
**Cost:** 1 token

List of vehicles that use a specific OEM part.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number |

---

### `GET /part/{number}/price`
**Cost:** 1 token

Official OEM price for a part.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number |

---

### `GET /part/{number}/images`
**Cost:** 1 token

Images for an OEM part.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number |

---

### `GET /part/{number}/supersessions`
**Cost:** 1 token

Successor and predecessor part numbers (replacement chain).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `number` | string | Yes (path) | OEM part number |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://plan.parts/api/v1/vag/part/03L115562/supersessions"
```

---

## Typical Workflow

```
GET /vag/catalogs
  → get catalog ID: "VW"

GET /vag/vehicles?catalog=VW&model=Golf&year=2018
  → get epis_typ for the vehicle

GET /vag/vehicle/{epis_typ}/groups?catalog=VW&lang=EN
  → pick group, note hg code

GET /vag/vehicle/{epis_typ}/subgroups?catalog=VW&hg=10&lang=EN
  → pick subgroup, note hg_ug and bildtafel2

GET /vag/vehicle/{epis_typ}/parts?catalog=VW&hg_ug=10-2&bildtafel2=2&lang=EN
  → get OEM part numbers, e.g. 03L115562

GET /vag/part/03L115562
  → name, status, replacements

GET /vag/part/03L115562/cross?type=IAM
  → IAM analogs: BOSCH F026407123, MANN W712/95...

GET /vag/part/03L115562/price
  → official dealer price
```

---

← [Back to v1 index](README.md)
