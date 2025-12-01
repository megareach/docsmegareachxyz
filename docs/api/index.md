# API Documentation

Build powerful integrations with MegaReach's comprehensive API platform.

## 🚀 Quick Start

### Get Started in 5 Minutes

1. **Get API Key**
   ```bash
   # Dashboard → Settings → API → Generate Key
   export MEGAREACH_API_KEY="sk_live_..."
   ```

2. **Install SDK**
   ```bash
   # Node.js
   npm install @megareach/sdk
   
   # Python
   pip install megareach
   
   # Go
   go get github.com/megareach/go-sdk
   ```

3. **Make First Request**
   ```javascript
   const MegaReach = require('@megareach/sdk');
   const client = new MegaReach(process.env.MEGAREACH_API_KEY);
   
   const user = await client.user.get();
   console.log(`Hello, ${user.name}!`);
   ```

## 📖 Documentation

### Core Documentation
- [API Reference](./reference.md) - Complete endpoint documentation
- [Authentication](./authentication.md) - API key management
- [Rate Limits](./rate-limits.md) - Usage limits and best practices
- [Errors](./errors.md) - Error codes and handling

### Feature APIs
- [ReplyGuy API](./replyguy-api.md) - AI reply generation
- [MegaBoost API](./megaboost-api.md) - Content amplification
- [Analytics API](./analytics-api.md) - Data and insights
- [Blockchain API](./blockchain-api.md) - Web3 integration

### Integration Guides
- [Webhooks](./webhooks.md) - Real-time events
- [Batch Operations](./batch.md) - Bulk processing
- [Streaming](./streaming.md) - Real-time data streams
- [GraphQL](./graphql.md) - Alternative query interface

## 🔑 Authentication

### API Key Types

| Type | Use Case | Permissions | Rate Limit |
|------|----------|-------------|------------|
| **Test** | Development | Limited | 100/hour |
| **Live** | Production | Full | Plan-based |
| **Restricted** | Public apps | Custom | Custom |
| **Admin** | Internal | All | Unlimited |

### Security Best Practices

```javascript
// ✅ Good - Environment variables
const apiKey = process.env.MEGAREACH_API_KEY;

// ❌ Bad - Hardcoded
const apiKey = "sk_live_abc123";

// ✅ Good - Server-side only
app.post('/api/reply', authenticate, async (req, res) => {
  const reply = await megareach.reply.generate(req.body);
  res.json(reply);
});

// ❌ Bad - Client-side exposure
fetch('https://api.megareach.xyz/v1/reply', {
  headers: { 'Authorization': 'Bearer sk_live_...' }
});
```

## 🚄 Rate Limits

### Limits by Plan

| Plan | Requests/Hour | Burst | Concurrent | Priority |
|------|--------------|-------|------------|----------|
| Free | 100 | 10/min | 1 | Low |
| Starter | 1,000 | 100/min | 5 | Normal |
| Growth | 5,000 | 500/min | 10 | High |
| Pro | 20,000 | 2,000/min | 50 | High |
| Enterprise | Unlimited | Custom | Custom | Highest |

### Rate Limit Headers

```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1609459200
X-RateLimit-Retry-After: 60
```

## 🔧 SDKs & Libraries

### Official SDKs

**JavaScript/TypeScript**
```typescript
import { MegaReach } from '@megareach/sdk';

const client = new MegaReach({
  apiKey: process.env.MEGAREACH_API_KEY,
  timeout: 30000,
  maxRetries: 3
});

// Type-safe API calls
const reply = await client.replyguy.generate({
  tweetId: '123',
  voice: 'professional'
});
```

**Python**
```python
from megareach import MegaReach
from megareach.types import Voice

client = MegaReach(
    api_key=os.getenv('MEGAREACH_API_KEY'),
    timeout=30
)

# Pythonic interface
reply = client.replyguy.generate(
    tweet_id='123',
    voice=Voice.PROFESSIONAL
)
```

**Go**
```go
import "github.com/megareach/go-sdk"

client := megareach.NewClient(
    megareach.WithAPIKey(os.Getenv("MEGAREACH_API_KEY")),
    megareach.WithTimeout(30 * time.Second),
)

reply, err := client.ReplyGuy.Generate(ctx, &megareach.GenerateRequest{
    TweetID: "123",
    Voice:   megareach.VoiceProfessional,
})
```

### Community SDKs
- Ruby: `gem install megareach`
- PHP: `composer require megareach/sdk`
- Java: `com.megareach:sdk:1.0.0`
- C#: `Install-Package MegaReach`
- Rust: `megareach = "1.0"`

## 🌐 Endpoints Overview

### Core Endpoints

**User Management**
```
GET    /v1/user                 # Get profile
PATCH  /v1/user                 # Update profile
GET    /v1/user/accounts        # List connected accounts
POST   /v1/user/accounts        # Connect new account
```

**ReplyGuy AI**
```
POST   /v1/replyguy/generate    # Generate reply
GET    /v1/replyguy/history     # Reply history
POST   /v1/replyguy/config      # Configure AI
GET    /v1/replyguy/analytics   # Performance metrics
```

**MegaBoost**
```
POST   /v1/boost/apply          # Apply boost
GET    /v1/boost/:id            # Get boost status
GET    /v1/boost/analytics      # Boost performance
POST   /v1/boost/credits        # Buy credits
```

**Analytics**
```
GET    /v1/analytics/dashboard  # Dashboard data
GET    /v1/analytics/growth     # Growth metrics
GET    /v1/analytics/content    # Content performance
POST   /v1/analytics/export     # Export data
```

## 🔄 Webhooks

### Available Events

**Reply Events**
- `reply.posted` - Reply successfully posted
- `reply.failed` - Reply failed to post
- `reply.engagement` - Reply received engagement

**Boost Events**
- `boost.started` - Boost campaign started
- `boost.completed` - Boost campaign ended
- `boost.milestone` - Boost reached milestone

**Account Events**
- `follower.new` - New follower gained
- `mention.received` - Mention detected
- `message.received` - DM received

### Webhook Setup

```javascript
// Register webhook endpoint
await client.webhooks.create({
  url: 'https://your-app.com/webhook',
  events: ['reply.posted', 'boost.completed'],
  secret: 'webhook_secret_123'
});

// Verify webhook signature
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hash = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  
  return `sha256=${hash}` === signature;
}
```

## 📊 API Examples

### Common Use Cases

**Auto-Reply to Mentions**
```javascript
// Set up mention monitoring
const monitor = await client.monitors.create({
  keywords: ['@yourbrand'],
  action: 'auto_reply',
  replyTemplate: 'Thanks for mentioning us!'
});
```

**Boost Viral Content**
```javascript
// Detect and boost high-performing content
const tweets = await client.content.list({ 
  engagement_rate: '>5%' 
});

for (const tweet of tweets) {
  await client.boost.apply({
    contentId: tweet.id,
    multiplier: 10,
    strategy: 'viral'
  });
}
```

**Generate Analytics Report**
```javascript
// Get comprehensive analytics
const report = await client.analytics.generate({
  period: '30d',
  metrics: ['growth', 'engagement', 'revenue'],
  format: 'pdf'
});

console.log(`Report URL: ${report.downloadUrl}`);
```

## 🧪 Testing

### Test Environment

**Base URL**: `https://testnet-api.megareach.xyz/v1`

**Test Credentials**
```bash
# Test API Key (public)
export MEGAREACH_TEST_KEY="sk_test_abc123xyz"

# Test wallet
export TEST_WALLET="0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb0"
```

**Test Data**
- Reset daily at midnight UTC
- 10,000 test credits provided
- Unlimited API calls
- No real transactions

### Postman Collection

Import our Postman collection for easy testing:

1. [Download Collection](https://postman.megareach.xyz/collection)
2. Import into Postman
3. Set environment variables
4. Start testing

## 🛠️ Tools & Resources

### Developer Tools

**API Explorer**
Interactive API documentation:
[explorer.megareach.xyz](https://explorer.megareach.xyz)

**SDK Playground**
Test SDK code in browser:
[playground.megareach.xyz](https://playground.megareach.xyz)

**Webhook Tester**
Debug webhook events:
[webhooks.megareach.xyz](https://webhooks.megareach.xyz)

### Code Examples

**GitHub Repository**
Complete examples for all languages:
[github.com/megareach/examples](https://github.com/megareach/examples)

**CodeSandbox Templates**
- [React Integration](https://codesandbox.io/s/megareach-react)
- [Node.js Backend](https://codesandbox.io/s/megareach-node)
- [Python Flask](https://codesandbox.io/s/megareach-python)

## 📈 API Analytics

### Monitor Your Usage

**Dashboard Metrics**
- API calls per endpoint
- Response time percentiles
- Error rates
- Credit consumption

**Usage Alerts**
```javascript
// Set up usage alerts
await client.alerts.create({
  type: 'api_usage',
  threshold: 80, // 80% of limit
  notification: 'email'
});
```

## 🔐 Security

### API Security Features

**Request Signing**
Optional HMAC signing for extra security:
```javascript
const signature = crypto
  .createHmac('sha256', apiSecret)
  .update(requestBody)
  .digest('hex');

headers['X-Signature'] = signature;
```

**IP Whitelisting**
Restrict API key to specific IPs:
```javascript
await client.security.updateKey({
  keyId: 'sk_live_...',
  allowedIps: ['1.2.3.4', '5.6.7.8']
});
```

## 🚨 Error Handling

### Error Response Format

```json
{
  "error": {
    "type": "rate_limit_error",
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests",
    "details": {
      "limit": 1000,
      "reset_at": "2024-01-01T12:00:00Z"
    }
  }
}
```

### Best Practices

```javascript
// Implement exponential backoff
async function retryWithBackoff(fn, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn();
    } catch (error) {
      if (error.code === 'RATE_LIMIT_EXCEEDED') {
        const delay = Math.pow(2, i) * 1000;
        await sleep(delay);
      } else {
        throw error;
      }
    }
  }
}
```

## 📞 API Support

### Getting Help

**Documentation**
- [API Reference](./reference.md)
- [FAQ](./faq.md)
- [Changelog](./changelog.md)

**Support Channels**
- **Email**: api@megareach.xyz
- **Discord**: #api-help
- **Stack Overflow**: [megareach-api](https://stackoverflow.com/questions/tagged/megareach-api)

**Office Hours**
- Wednesdays 3 PM EST
- Live coding sessions
- Q&A with API team

### SLA Guarantees

| Plan | Uptime | Support Response |
|------|--------|-----------------|
| Free | 99% | 72 hours |
| Starter | 99.5% | 48 hours |
| Growth | 99.9% | 24 hours |
| Pro | 99.95% | 2 hours |
| Enterprise | 99.99% | 15 minutes |

---

*Ready to build? Get your API key at [app.megareach.xyz/api](https://app.megareach.xyz/api)*