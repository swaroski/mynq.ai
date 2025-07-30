# 🚀 MYNQ AI API Quick Start Guide

Get up and running with the MYNQ AI API in under 5 minutes.

## 📋 Prerequisites

- MYNQ AI account ([sign up here](https://mynq.ai/signup))
- API key from your [dashboard](https://mynq.ai/dashboard/api)
- Basic knowledge of REST APIs

## 🔑 Authentication

All API requests require authentication using your API key in the header:

```bash
Authorization: Bearer YOUR_API_KEY
```

## 🌐 Base URL

```
https://api.mynq.ai/v1
```

## 📚 Quick Examples

### 1. Resume Analysis

Analyze a resume for ATS compatibility:

```bash
curl -X POST https://api.mynq.ai/v1/resume/analyze \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "resumeText": "John Doe\nSoftware Engineer\n...",
    "jobDescription": "Looking for a Python developer...",
    "analysisType": "ats_score"
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "ats_score": 85,
    "recommendations": [
      "Add more relevant keywords",
      "Improve work experience descriptions"
    ],
    "missing_keywords": ["Python", "Django", "REST API"],
    "strength_areas": ["Education", "Technical Skills"]
  }
}
```

### 2. Create AI Agent

Deploy a custom AI agent:

```bash
curl -X POST https://api.mynq.ai/v1/agents \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Customer Support Bot",
    "description": "Handles basic customer inquiries",
    "tools": ["resume-checker", "lead-generator"],
    "configuration": {
      "response_tone": "professional",
      "max_response_length": 500
    }
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "agent_id": "agent_abc123",
    "name": "Customer Support Bot",
    "status": "active",
    "webhook_url": "https://api.mynq.ai/v1/agents/agent_abc123/webhook",
    "created_at": "2025-01-30T10:30:00Z"
  }
}
```

### 3. Voice AI Session

Start a voice automation session:

```bash
curl -X POST https://api.mynq.ai/v1/voice/create-session \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "customer_service",
    "phoneNumber": "+1234567890",
    "configuration": {
      "voice": "professional_female",
      "language": "en-US",
      "script_template": "customer_support_v1"
    }
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "session_id": "voice_xyz789",
    "status": "initialized",
    "phone_number": "+1234567890",
    "estimated_start_time": "2025-01-30T11:00:00Z"
  }
}
```

### 4. Lead Generation

Generate B2B leads:

```bash
curl -X POST https://api.mynq.ai/v1/leads/generate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "industry": "technology",
    "company_size": "50-200",
    "location": "San Francisco, CA",
    "job_titles": ["CTO", "VP Engineering", "Tech Lead"],
    "limit": 10
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "leads": [
      {
        "company": "TechCorp Inc",
        "contact_name": "Jane Smith",
        "title": "CTO",
        "email": "jane@techcorp.com",
        "linkedin": "linkedin.com/in/janesmith",
        "company_size": "150",
        "confidence_score": 92
      }
    ],
    "total_found": 47,
    "credits_used": 10
  }
}
```

## 📊 Response Format

All API responses follow this structure:

```json
{
  "success": boolean,
  "data": object | array,
  "error": {
    "code": "string",
    "message": "string"
  },
  "meta": {
    "credits_used": number,
    "rate_limit_remaining": number
  }
}
```

## ⚠️ Error Handling

Common error codes:

| Code | Status | Description |
|------|--------|-------------|
| `unauthorized` | 401 | Invalid or missing API key |
| `forbidden` | 403 | Insufficient permissions |
| `not_found` | 404 | Resource not found |
| `rate_limited` | 429 | Too many requests |
| `validation_error` | 400 | Invalid request data |
| `server_error` | 500 | Internal server error |

**Example error response:**
```json
{
  "success": false,
  "error": {
    "code": "validation_error",
    "message": "Resume text is required"
  }
}
```

## 🔄 Rate Limits

Rate limits vary by plan:

| Plan | Requests/Minute | Daily Limit |
|------|----------------|-------------|
| Free | 10 | 100 |
| Professional | 100 | 10,000 |
| Enterprise | 1,000 | Unlimited |

Rate limit headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1643723400
```

## 📝 SDK Libraries

Official SDKs available:

### JavaScript/Node.js
```bash
npm install @mynqai/sdk
```

```javascript
import { MynqAI } from '@mynqai/sdk';

const client = new MynqAI('YOUR_API_KEY');

const result = await client.resume.analyze({
  resumeText: 'John Doe...',
  analysisType: 'ats_score'
});
```

### Python
```bash
pip install mynqai
```

```python
from mynqai import MynqAI

client = MynqAI('YOUR_API_KEY')

result = client.resume.analyze(
    resume_text='John Doe...',
    analysis_type='ats_score'
)
```

### Go
```bash
go get github.com/mynqai/go-sdk
```

```go
import "github.com/mynqai/go-sdk"

client := mynqai.NewClient("YOUR_API_KEY")
result, err := client.Resume.Analyze(ctx, &mynqai.ResumeAnalyzeRequest{
    ResumeText: "John Doe...",
    AnalysisType: "ats_score",
})
```

## 🔗 Webhooks

Subscribe to events with webhooks:

```bash
curl -X POST https://api.mynq.ai/v1/webhooks \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://your-app.com/webhook",
    "events": ["agent.completed", "voice.session_ended"],
    "secret": "your_webhook_secret"
  }'
```

**Webhook payload example:**
```json
{
  "event": "agent.completed",
  "timestamp": "2025-01-30T10:30:00Z",
  "data": {
    "agent_id": "agent_abc123",
    "task_id": "task_def456",
    "result": {...}
  }
}
```

## 🛠️ Testing

Test your integration with our sandbox environment:

**Sandbox Base URL:** `https://sandbox-api.mynq.ai/v1`

Use test API keys (prefix: `test_`) for safe testing without consuming credits.

## 📚 Next Steps

- [Full API Documentation](https://docs.mynq.ai/api)
- [Authentication Guide](https://docs.mynq.ai/auth)
- [Webhook Guide](https://docs.mynq.ai/webhooks)
- [SDK Documentation](https://docs.mynq.ai/sdks)
- [Code Examples](https://github.com/mynqai/examples)

## 💬 Support

Need help? We're here for you:

- **Documentation**: [docs.mynq.ai](https://docs.mynq.ai)
- **Discord**: [discord.gg/mynqai](https://discord.gg/mynqai)
- **Email**: api-support@mynq.ai
- **Status**: [status.mynq.ai](https://status.mynq.ai)

---

*Ready to build something amazing? Start coding with MYNQ AI! 🚀*