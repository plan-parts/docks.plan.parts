# PowerCatalog

6 endpoints for sports and power equipment parts — ATVs, motorcycles, watercraft.

**Base path:** `https://cdn.plan.parts/api/v1/pc`

---

## Equipment Groups

### `GET /groups`
**Cost:** Free

List of all top-level equipment groups (ATVs, motorcycles, watercraft, snowmobiles, etc.).

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/pc/groups"
```

---

## Navigation Hierarchy

### `GET /brands`
**Cost:** 1 token

Brands available for a given equipment group.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `group_id` | integer | Yes | Group ID from `/groups` (e.g. `1`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/pc/brands?group_id=1"
```

---

### `GET /years`
**Cost:** 1 token

Available production years for a brand.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mfa_id` | integer | Yes | Brand ID from `/brands` (e.g. `121`) |

---

### `GET /models`
**Cost:** 1 token

Models for a brand + year combination.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `mfa_id` | integer | Yes | Brand ID |
| `year` | integer | Yes | Production year |

---

### `GET /bodies`
**Cost:** 1 token

Assemblies / body variants for a model.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `ms_id` | integer | Yes | Model series ID from `/models` (e.g. `1994`) |

---

## Parts

### `GET /parts`
**Cost:** 2 tokens

Spare parts list with articles and images for a specific assembly.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `body_id` | integer | Yes | Assembly ID from `/bodies` (e.g. `301`) |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/pc/parts?body_id=301"
```

---

## Typical Workflow

```
GET /pc/groups
  → pick group "Motorcycles" (group_id=1)

GET /pc/brands?group_id=1
  → pick Honda (mfa_id=121)

GET /pc/years?mfa_id=121
  → pick year 2020

GET /pc/models?mfa_id=121&year=2020
  → pick "CBR600RR" (ms_id=1994)

GET /pc/bodies?ms_id=1994
  → pick body variant (body_id=301)

GET /pc/parts?body_id=301
  → full parts list with article numbers and images
```

---

← [Back to v1 index](README.md)
