# System

System-level endpoints for connection and authentication checks.

---

## Ping

### `GET /api/v1/ping`
**Cost:** Free

Verifies connection and token validity. Use this to check that your API key is working before making paid requests.

```bash
curl -H "Authorization: Bearer TOKEN" \
  "https://cdn.plan.parts/api/v1/ping"
```

**Response (success):**
```json
{ "status": "ok" }
```

**Response (invalid token):**
```json
{ "message": "Unauthenticated." }
```
HTTP status `401`.

---

← [Back to v1 index](README.md)
