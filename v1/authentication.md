# Authentication

All plan.parts API endpoints (except free health checks) require a Bearer token.

## Getting a Token

1. Register at [plan.parts/register](https://plan.parts/register)
2. Open your portal: [plan.parts/portal/api-keys](https://plan.parts/portal/api-keys)
3. Generate a new API key

## Using the Token

Pass it in the `Authorization` header on every request:

```http
Authorization: Bearer YOUR_TOKEN_HERE
```

**curl:**
```bash
curl -H "Authorization: Bearer YOUR_TOKEN_HERE" \
     "https://plan.parts/api/v1/aftermarket/analogs?article=F026407123&brand=BOSCH"
```

**PHP (Guzzle / Laravel HTTP):**
```php
$response = Http::withToken(config('services.planparts.token'))
    ->get('https://plan.parts/api/v1/aftermarket/analogs', [
        'article' => 'F026407123',
        'brand'   => 'BOSCH',
    ]);
```

**JavaScript (fetch):**
```js
const res = await fetch(
    'https://plan.parts/api/v1/aftermarket/analogs?article=F026407123&brand=BOSCH',
    { headers: { Authorization: `Bearer ${TOKEN}` } }
);
const data = await res.json();
```

## Token Billing

- Tokens are deducted **only on successful `2xx` responses**.
- Failed requests (`4xx`, `5xx`) are not charged.

## Error Responses

| HTTP Status | Meaning | Action |
|-------------|---------|--------|
| `401 Unauthorized` | Missing or invalid token | Check your API key |
| `402 Payment Required` | Zero token balance | Top up at [plan.parts/portal](https://plan.parts/portal) |
| `429 Too Many Requests` | Rate limit exceeded | Slow down requests |

---

← [Back to v1 index](README.md)
