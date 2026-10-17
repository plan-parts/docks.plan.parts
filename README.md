# plan.parts — Developer Documentation

Official documentation for integrating with the [plan.parts](https://plan.parts) platform — a REST API for OEM and IAM auto parts cross-references, vehicle catalogs, and price intelligence.

---

## What is plan.parts?

**plan.parts** is a pay-per-request API platform for the automotive aftermarket. It helps auto parts distributors, online stores, and developers to:

- Find **OEM ↔ IAM analog parts** across brands (Bosch, MANN, MAHLE, Febi, etc.)
- Look up parts by **vehicle** (brand → model → modification → category)
- Resolve **OEM numbers** (e.g. VW `03L115562`) into aftermarket analogs
- Browse **VAG Group** (VW, Audi, Skoda, SEAT, Porsche) OEM catalogs
- Access parts for **power equipment** — ATVs, motorcycles, watercraft

> **Token-based pricing.** Tokens are deducted only on successful `2xx` responses.

---

## Getting Started

1. Register at [plan.parts/register](https://plan.parts/register)
2. Generate an API key in your portal: [plan.parts/portal/api-keys](https://plan.parts/portal/api-keys)
3. Pass the key as a Bearer token on every request

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     "https://cdn.plan.parts/api/v1/ping"
```

---

## Documentation Versions

| Version | Status | Docs |
|---------|--------|------|
| **v1** (current) | Stable | [v1/README.md](v1/README.md) |

---

## API Overview

| Section | Endpoints | Description |
|---------|-----------|-------------|
| [Aftermarket (OEM / OE / IAM)](v1/aftermarket.md) | 15 | Cross-references, vehicle catalog, article search |
| [VAG Catalog (ETKA)](v1/vag-catalog.md) | 13 | VW, Audi, Skoda, SEAT, Porsche OEM parts |
| [PowerCatalog](v1/power-catalog.md) | 6 | ATVs, motorcycles, watercraft |
| [VIN Decoder](v1/vin-decoder.md) | 1 | Universal VIN decode — manufacturer, model, vehicles |
| [System](v1/system.md) | 1 | Health check and auth ping |
| [Authentication](v1/authentication.md) | — | Token setup and error reference |
| [Data Tasks](v1/data-tasks.md) | — | Managed bulk data enrichment jobs |

---

## Machine-Readable Resources

- **OpenAPI 3.0 schema:** [plan.parts/openapi.json](https://plan.parts/openapi.json)
- **AI agent docs:** [plan.parts/llms.txt](https://plan.parts/llms.txt)
- **Interactive playground:** [plan.parts/docs](https://plan.parts/docs)

---

## SDKs & Integrations

| Package | Language | Description |
|---------|----------|-------------|
| [oem-iam-parts-cross-matcher](https://github.com/plan-parts/oem-iam-parts-cross-matcher) | PHP / Laravel | Upload price lists and find analogs via plan.parts API |

---

## Support

- Register and open a ticket: [plan.parts/register](https://plan.parts/register)
- Website: [plan.parts](https://plan.parts)

---

## Changelog

See [CHANGELOG.md](CHANGELOG.md).
