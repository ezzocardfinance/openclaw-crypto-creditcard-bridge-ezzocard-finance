# OpenClaw Crypto Creditcard Bridge (Ezzocard.Finance)

https://Ezzocard.Finance

Supports 11 cryptocurrencies, real blockchain verification, OpenClaw/MCP integration, and autonomous card delivery.  
AI Agent API for automated purchase of prepaid virtual cards.

[![PHP](https://img.shields.io/badge/PHP-7.4%2B-blue)](https://php.net)
[![API](https://img.shields.io/badge/API-REST%20%2B%20JSON-green)](https://ezzocard.finance/ai-agent/api-docs.php)
[![OpenClaw](https://img.shields.io/badge/OpenClaw-MCP%20Ready-orange)](https://ezzocard.finance/.well-known/ai-plugin.json)
[![AI Agent Ready](https://img.shields.io/badge/AI-Agent%20Ready-brightgreen)](https://ezzocard.finance/ai-agent/api-docs.php)

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

## ⚡ Quick Example

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

JSON
```bash
{
  "success": true,
  "secret": "a8f9c1d9e7b4c6f2",
  "amount_crypto": "0.00367512",
  "address": "bc1qnuwcxx75f4r4f6xankfkgae2megjj37zg7xuz8",
  "status_check_url": "https://ezzocard.finance/api/ai-check.php?secret=a8f9c1d9e7b4c6f2"
}
```

🤖 Example Agent Flow (Step-by-Step)

This is the recommended integration flow for AI agents such as GPT, Claude, OpenClaw, and external autonomous systems.

1. Create Order

Send a POST request to:
/ai-agent/process.php

Required fields:
card_type
amount
crypto

Optional fields:
currency (default: USD)
email

Example request:
JSON
```bash
{
  "card_type": "gold_visa",
  "amount": 250,
  "currency": "USD",
  "crypto": "BTC",
  "email": "client@example.com"
}
```

2. Store the Returned Secret Immediately
The API returns a unique secret.
This secret is the main client-facing identifier and must be stored immediately.

The secret is used for:
order status checks
payment tracking
webhook registration
final card retrieval

Do not rely on order_id for client-side tracking.

3. Present Payment Instructions
Use the exact values returned by the API:
amount_crypto
address
crypto

Do not recalculate exchange rates manually.
Do not modify payment network names.
Do not round the returned crypto amount.

4. Wait for Blockchain Confirmation
The user or agent must send the exact amount_crypto to the returned address within the payment window.
Orders expire automatically after 30 minutes.

5. Poll Order Status
Check status using:
GET /api/ai-check.php?secret={secret}
Repeat every 20–30 seconds until:
status = confirmed, or
status = expired

6. Retrieve Card Data
When payment is confirmed and delivery is completed, card data is returned in the status response.

7. Optional: Register Webhooks
Instead of polling, agents may register a webhook using the same secret:
POST /api/openclaw-webhook.php

Supported events:
payment.confirmed
cards.delivered
order.expired

🧠 OpenAI Function Calling
Below is a complete example schema for OpenAI function/tool calling.

JSON
```bash
{
  "type": "function",
  "function": {
    "name": "create_card_order",
    "description": "Create a prepaid virtual card order paid with cryptocurrency through the Ezzocard API.",
    "parameters": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "card_type": {
          "type": "string",
          "description": "Type of virtual prepaid card to purchase.",
          "enum": [
            "gold_visa",
            "gold_mastercard",
            "violet",
            "lime-7",
            "lime-30",
            "brown",
            "orange",
            "yellow",
            "maroon",
            "teal"
          ]
        },
        "amount": {
          "type": "number",
          "description": "Card denomination.",
          "minimum": 10,
          "maximum": 10000
        },
        "currency": {
          "type": "string",
          "description": "Card currency. Defaults to USD.",
          "enum": ["USD", "CAD"]
        },
        "crypto": {
          "type": "string",
          "description": "Public crypto payment value accepted by the API.",
          "enum": [
            "BTC",
            "ETH",
            "USDT.ERC20",
            "USDT.TRC20",
            "USDT.BEP20",
            "USDT.SOL",
            "DOGE",
            "LTC",
            "TRX",
            "SOL",
            "BNB"
          ]
        },
        "email": {
          "type": "string",
          "description": "Optional customer email address.",
          "format": "email"
        }
      },
      "required": ["card_type", "amount", "crypto"]
    }
  }
}
```

Recommended OpenAI Tool Set

JSON
```bash
[
  {
    "type": "function",
    "function": {
      "name": "create_card_order",
      "description": "Create a prepaid virtual card order paid with cryptocurrency through the Ezzocard API.",
      "parameters": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "card_type": {
            "type": "string",
            "enum": [
              "gold_visa",
              "gold_mastercard",
              "violet",
              "lime-7",
              "lime-30",
              "brown",
              "orange",
              "yellow",
              "maroon",
              "teal"
            ]
          },
          "amount": {
            "type": "number",
            "minimum": 10,
            "maximum": 10000
          },
          "currency": {
            "type": "string",
            "enum": ["USD", "CAD"]
          },
          "crypto": {
            "type": "string",
            "enum": [
              "BTC",
              "ETH",
              "USDT.ERC20",
              "USDT.TRC20",
              "USDT.BEP20",
              "USDT.SOL",
              "DOGE",
              "LTC",
              "TRX",
              "SOL",
              "BNB"
            ]
          },
          "email": {
            "type": "string",
            "format": "email"
          }
        },
        "required": ["card_type", "amount", "crypto"]
      }
    }
  },
  {
    "type": "function",
    "function": {
      "name": "check_card_order_status",
      "description": "Check payment and delivery status for an Ezzocard order using the returned secret.",
      "parameters": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "secret": {
            "type": "string",
            "pattern": "^[a-f0-9]{16}$",
            "description": "16-character secret returned by the create order endpoint."
          }
        },
        "required": ["secret"]
      }
    }
  },
  {
    "type": "function",
    "function": {
      "name": "register_card_order_webhook",
      "description": "Register a webhook for payment and delivery events for an Ezzocard order.",
      "parameters": {
        "type": "object",
        "additionalProperties": false,
        "properties": {
          "secret": {
            "type": "string",
            "pattern": "^[a-f0-9]{16}$"
          },
          "webhook_url": {
            "type": "string",
            "format": "uri"
          },
          "events": {
            "type": "array",
            "items": {
              "type": "string",
              "enum": [
                "payment.confirmed",
                "cards.delivered",
                "order.expired"
              ]
            }
          }
        },
        "required": ["secret", "webhook_url"]
      }
    }
  }
]
```

🧩 Claude Tool Schema (Advanced)
Below is an advanced Claude-compatible tool schema with enums and validation.

JSON
```bash
[
  {
    "name": "create_card_order",
    "description": "Create a prepaid virtual card order paid with cryptocurrency.",
    "input_schema": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "card_type": {
          "type": "string",
          "enum": [
            "gold_visa",
            "gold_mastercard",
            "violet",
            "lime-7",
            "lime-30",
            "brown",
            "orange",
            "yellow",
            "maroon",
            "teal"
          ],
          "description": "Exact card type identifier."
        },
        "amount": {
          "type": "number",
          "minimum": 10,
          "maximum": 10000,
          "description": "Requested card denomination."
        },
        "currency": {
          "type": "string",
          "enum": ["USD", "CAD"],
          "description": "Card currency. Omit to use USD."
        },
        "crypto": {
          "type": "string",
          "enum": [
            "BTC",
            "ETH",
            "USDT.ERC20",
            "USDT.TRC20",
            "USDT.BEP20",
            "USDT.SOL",
            "DOGE",
            "LTC",
            "TRX",
            "SOL",
            "BNB"
          ],
          "description": "Public crypto network identifier."
        },
        "email": {
          "type": "string",
          "format": "email",
          "description": "Optional delivery or notification email."
        }
      },
      "required": ["card_type", "amount", "crypto"]
    }
  },
  {
    "name": "check_card_order_status",
    "description": "Check payment and delivery status using the returned secret.",
    "input_schema": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "secret": {
          "type": "string",
          "pattern": "^[a-f0-9]{16}$",
          "description": "Order secret returned when the order was created."
        }
      },
      "required": ["secret"]
    }
  },
  {
    "name": "register_card_order_webhook",
    "description": "Register a webhook for payment, delivery, and expiry events.",
    "input_schema": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "secret": {
          "type": "string",
          "pattern": "^[a-f0-9]{16}$"
        },
        "webhook_url": {
          "type": "string",
          "format": "uri"
        },
        "events": {
          "type": "array",
          "description": "Optional list of event types to subscribe to.",
          "items": {
            "type": "string",
            "enum": [
              "payment.confirmed",
              "cards.delivered",
              "order.expired"
            ]
          }
        }
      },
      "required": ["secret", "webhook_url"]
    }
  }
]
```

📦 Create Order (Full Spec)
Request Body

JSON
```bash
{
  "card_type": "gold_visa",
  "amount": 250,
  "currency": "USD",
  "crypto": "BTC",
  "email": "client@example.com"
}
```

Fields
Required: card_type, amount, crypto

Optional:
currency (default: USD)
email

Success Response

JSON
```bash
{
  "success": true,
  "order_id": "AI-GOLD_VISA-1773842620",
  "secret": "07df4a3f136171ff",
  "card_type": "gold_visa",
  "amount": 250,
  "currency": "USD",
  "payment_url": "https://ezzocard.finance/ai-agent/order-ai.php?secret=07df4a3f136171ff",
  "status_check_url": "https://ezzocard.finance/api/ai-check.php?secret=07df4a3f136171ff",
  "amount_crypto": "0.00367512",
  "crypto": "BTC",
  "address": "bc1qnuwcxx75f4r4f6xankfkgae2megjj37zg7xuz8",
  "expires_in": 1800,
  "message": "Order created successfully. Save the secret and send the exact crypto amount to the provided address."
}
```

Error Response

JSON
```bash
{
  "success": false,
  "error": "Invalid card type or amount",
  "code": "INVALID_PARAMETERS"
}
```

🔍 Check Order Status
GET /api/ai-check.php?secret={secret}
Always use the original amount_crypto and address returned during order creation.

Pending Response

JSON
```bash
{
  "order_id": "AI-GOLD_VISA-1773842620",
  "secret": "07df4a3f136171ff",
  "status": "awaiting_payment",
  "time_remaining_seconds": 1425,
  "payment_detected": false,
  "next_step": "continue_waiting",
  "check_again_in_seconds": 30,
  "payment_instructions": {
    "address": "bc1qnuwcxx75f4r4f6xankfkgae2megjj37zg7xuz8",
    "amount": "0.00367512",
    "currency": "BTC"
  }
}
```

Confirmed Response

JSON
```bash
{
  "order_id": "AI-GOLD_VISA-1773842620",
  "secret": "07df4a3f136171ff",
  "status": "confirmed",
  "payment_detected": true,
  "delivery_status": "completed",
  "tx_hash": "3c7b48242ae74f8f8480b65010fcfe836ba3490aac780b2e99671a7f938dc04b",
  "confirmations": 3,
  "cards": [
    {
      "display_name": "$250 Gold VISA",
      "number": "4246051668636185",
      "expiry": "10/28",
      "cvv": "607",
      "system": "VISA",
      "card_type": "gold_visa"
    }
  ]
}
```

Expired Response

JSON
```bash
{
  "order_id": "AI-GOLD_VISA-1773842620",
  "secret": "07df4a3f136171ff",
  "status": "expired",
  "payment_detected": false,
  "error": "Payment window expired (30 minutes)",
  "next_step": "order_failed"
}
```

🔔 Register Webhook
POST /api/openclaw-webhook.php

Request Body

JSON
```bash
{
  "secret": "07df4a3f136171ff",
  "webhook_url": "https://example-agent.com/webhooks/ezzocard",
  "events": [
    "payment.confirmed",
    "cards.delivered"
  ]
}
```

Fields

Required: secret, webhook_url
Optional: events
Supported events:
payment.confirmed
cards.delivered
order.expired

Success Response

JSON
```bash
{
  "success": true,
  "webhook_id": "wh_9fbc0f14a1b2",
  "secret": "07df4a3f136171ff",
  "signing_secret": "9d8f6d0b5ea3f71a9a5e5d8c9b2a7f11",
  "events": [
    "payment.confirmed",
    "cards.delivered"
  ],
  "message": "Webhook registered successfully"
}
```

🤖 AI Agent Rules

Always store the returned secret
Always use the returned status_check_url
Always use the exact amount_crypto
Never recalculate conversion rates manually
Never alter crypto network names
Treat order_id as informational only
Treat secret as the primary tracking key
Poll every 20–30 seconds or use webhooks
Orders expire after 30 minutes
If expired, create a new order

📚 Documentation

LLMs: https://ezzocard.finance/llms.txt 

OpenAPI: https://ezzocard.finance/openapi.json 

OpenAPI YAML: https://ezzocard.finance/ai-agent/openapi.yaml

AI Plugin: https://ezzocard.finance/.well-known/ai-plugin.json 

Full Docs: https://ezzocard.finance/ai-agent/api-docs.php 

🛡️ Security

Unique secret per order (16-char hex)
Timestamp verification (anti-replay)
Real blockchain validation
30-minute payment window

🆘 Support

For AI integration issues: support@ezzocard.finance
API Docs: https://ezzocard.finance/ai-agent/api-docs.php

📄 License
Proprietary - All rights reserved. Copyright © 2026 Ezzocard.Finance.
