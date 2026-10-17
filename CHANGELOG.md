# Changelog

## [v1.1] — 2024-10

- Base URL updated to `https://cdn.plan.parts/api/v1`
- Added VIN Decoder endpoint (`GET /vin/{vin}`, 50 tokens)
- `/analogs` — `brand` parameter marked as required
- `/search` — updated `type` values: `OEM`, `ART`, `EAN`, `IAM`; default `ART`
- `/search/suggest` — `limit` max 20, default 10
- `/articles/{art_id}/vehicles` — `per_page` max 100, default 50
- VAG `/vin/{vin}` — cost corrected to 2 tokens
- VAG `/vehicles` — `catalog` marked as required; added `limit` parameter
- VAG `/vehicle/{epis_typ}/parts` — documented `hg_ug` and `bildtafel2` as required

## [v1.0] — 2024-10

- Initial documentation release
- Aftermarket (OEM / OE / IAM) — 15 endpoints
- VAG Catalog (ETKA) — 13 endpoints
- PowerCatalog — 6 endpoints
- System — 1 endpoint
- Authentication guide
- Data Tasks service overview
