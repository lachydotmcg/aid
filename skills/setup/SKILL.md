---
description: Configure your AID Helpdesk API key and verify the connection. Run this first before using any other /aid skills.
---

# AID Helpdesk — Setup

Check whether `AID_API_KEY` and `AID_URL` environment variables are set.

- `AID_API_KEY` — your tenant API key (found in Settings on your dashboard)
- `AID_URL` — your AID Helpdesk URL (e.g. `https://web-production-01ecc.up.railway.app`)

If either is missing, tell the user exactly which one is absent and how to set it:
```
export AID_API_KEY=your_key_here
export AID_URL=https://web-production-01ecc.up.railway.app
```

If both are set, test the connection by running:
```bash
curl -s -X GET "$AID_URL/api/v1/status" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json"
```

If the response contains `"success": true`, report the tenant name and plan from the response and confirm the plugin is ready to use.

If the request fails or returns an error, show the error message and suggest the user double-check their API key in the AID Helpdesk dashboard under Settings.
