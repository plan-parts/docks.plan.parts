# plan.parts API — v1 Documentation

**Revision 1** · October 2024

This is the complete reference for the plan.parts REST API v1.

## Base URL

```
https://plan.parts/api/v1
```

## Sections

| Section | Endpoints | Description |
|---------|-----------|-------------|
| [Authentication](authentication.md) | — | How to get and use API tokens |
| [Aftermarket (OEM / OE / IAM)](aftermarket.md) | 15 | Cross-references, vehicle catalog, article search |
| [VAG Catalog (ETKA)](vag-catalog.md) | 13 | VW, Audi, Skoda, SEAT, Porsche OEM parts |
| [PowerCatalog](power-catalog.md) | 6 | ATVs, motorcycles, watercraft |
| [System](system.md) | 1 | Health check and auth ping |
| [Data Tasks](data-tasks.md) | — | Managed one-time data enrichment jobs |

## Token Costs at a Glance

| Cost | Endpoints |
|------|-----------|
| Free | `/ping`, `/aftermarket/health`, `/vag/health`, `/pc/groups`, `/vag/catalogs` |
| 1 token | Catalog browsing, vehicle lookup, suggest, article media/criteria |
| 2 tokens | Article search, analogs, crosses, vehicles, parts listing |

## Getting Started

1. Register at [plan.parts/register](https://plan.parts/register)
2. Generate an API key in your portal
3. Pass it as a Bearer token on every request

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     "https://plan.parts/api/v1/aftermarket/health"
```

---

← [Back to root](../README.md)
