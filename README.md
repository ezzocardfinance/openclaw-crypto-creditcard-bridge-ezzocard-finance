# OpenClaw Crypto Creditcard Bridge (Ezzocard.Finance)

https://Ezzocard.Finance

Supports 11 cryptocurrencies, real blockchain verification,  OpenClaw/MCP integration, and autonomous card delivery.
AI Agent API for automated purchase of prepaid virtual cards.

[![PHP](https://img.shields.io/badge/PHP-7.4%2B-blue)](https://php.net)
[![API](https://img.shields.io/badge/API-REST%20%2B%20JSON-green)](https://ezzocard.finance/ai-agent/api-docs.php)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-MCP%20Ready-orange)](https://ezzocard.finance/.well-known/ai-plugin.json)

## 🚀 Features

- **AI Agent Native**: JSON API designed for autonomous systems (Claude, GPT, etc.)
- **11 Crypto Networks**: BTC, ETH, USDT (ERC20/TRC20/BEP20/SOL), DOGE, LTC, TRX, SOL, BNB
- **Real Blockchain Verification**: On-chain payment confirmation before card delivery
- **OpenClaw/MCP Integration**: Model Context Protocol support with webhooks
- **Dynamic Pricing**: Real-time rates from Binance API
- **Auto Card Allocation**: Automatic assignment from inventory pools
- **30min Payment Window**: Auto-expiration with status tracking
- **Dual Interface**: Human-friendly UI + Machine API
- **10 Card Types**: Gold Visa/Mastercard, Violet, Lime-7/30, Brown, Orange, Yellow, Maroon, Teal

## 🔌 API Endpoints

### Create Order
`POST /ai-agent/process.php`

### Check Status  
`GET /api/ai-check.php?secret={secret}`

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/ai-agent/process.php` | POST | Create new order |
| `/api/ai-check.php` | GET | Check payment status & retrieve cards |
| `/api/ai-status.php` | GET | Simplified status check |
| `/api/openclaw-webhook.php` | POST | Async notifications |

### Quick Example

```bash
curl -X POST https://ezzocard.finance/ai-agent/process.php \
  -H "Content-Type: application/json" \
  -d '{
    "card_type": "gold_visa",
    "amount": 250,
    "currency": "USD",
    "crypto": "BTC",
    "email": "agent@example.com"
  }'
```
Response:
```bash
{
  "success": true,
  "secret": "a8f9c1d9e7b4c6f2",
  "amount_crypto": "0.00367512",
  "address": "bc1qnuwcxx75f4r4f6xankfkgae2megjj37zg7xuz8",
  "status_check_url": "https://ezzocard.finance/api/ai-check.php?secret=a8f9c1d9e7b4c6f2"
}
```
### Documentation
- LLMs: https://ezzocard.finance/llms.txt
- OpenAPI: https://ezzocard.finance/openapi.json

## 🛡️ Security
- Unique `secret` per order (16-char hex)
- Timestamp verification (anti-replay)
- Real blockchain validation
- 30-minute payment window

### 🆘 Support
For AI integration issues: support@ezzocard.finance
API Docs: https://ezzocard.finance/ai-agent/api-docs.php
  
📄 License
Proprietary - All rights reserved. Copyright © 2026 Ezzocard.Finance.


