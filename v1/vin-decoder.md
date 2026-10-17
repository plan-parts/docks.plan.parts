# VIN Decoder

Universal VIN decoder — identifies manufacturer, model, and matching vehicles from a 17-character VIN.

**Base path:** `https://cdn.plan.parts/api/v1`

> **Cost: 50 tokens per request.** Use this endpoint for on-demand lookups, not bulk processing.

---

## `GET /vin/{vin}`
**Cost:** 50 tokens

Decode a 17-character VIN. Returns manufacturer, model, production details, and a list of matching vehicles.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `vin` | string | Yes (path) | 17-character VIN code |

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/vin/WVWZZZ1JZXW000001"
```

---

## VAG VIN Lookup

For Volkswagen Group vehicles (VW, Audi, Skoda, SEAT, Porsche) there is also a VAG-specific endpoint at a lower cost:

**`GET /api/v1/vag/vin/{vin}` — 2 tokens**

See [vag-catalog.md](vag-catalog.md) for details.

---

← [Back to v1 index](README.md)
