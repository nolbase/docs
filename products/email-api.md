# Email API

> Transactional email and SMS at scale, via API.

**Module slug:** `dispatch`
**Category:** Operations & Delivery
**URL:** `/email-api`

## Overview

Email API (Dispatch) is NolBase's developer-friendly email infrastructure. Send transactional emails and SMS at scale through a clean REST API. Built-in domain authentication (SPF, DKIM, DMARC), real-time delivery tracking, bounce management, and email validation.

Send your first email in under 5 minutes. No SMTP configuration. No deliverability headaches.

## Key Features

### API-First Design
A clean REST API with comprehensive SDKs:
- RESTful endpoints for sending, templates, and analytics
- Official SDKs for Node.js, Python, Ruby, Go, PHP
- Send your first email in under 5 minutes
- Batch sending support for high-volume use cases

### Domain Authentication
Complete email authentication out of the box:
- SPF record generation and verification
- DKIM signing with automated key rotation
- DMARC policy configuration
- Domain verification wizard with step-by-step DNS instructions

### Real-Time Tracking
Know exactly what happens to every message:
- Open tracking with pixel-based detection
- Click tracking with link wrapping
- Delivery webhooks for every event (sent, delivered, opened, clicked, bounced, complained)
- Real-time event stream via API

### Template Engine
Build and manage email templates:
- Handlebars template syntax for dynamic content
- MJML support for responsive email design
- Variable substitution and conditional blocks
- Template versioning and preview
- Test rendering before sending

### Bounce & Suppression Management
Protect your sender reputation automatically:
- Automatic hard bounce detection and suppression
- Soft bounce retry with configurable policy
- Complaint processing (feedback loop integration)
- Global suppression list management
- Unsubscribe handling

### Email Validation
Verify addresses before sending:
- Syntax validation
- MX record verification
- Disposable email domain detection
- Role-based address detection
- Bulk validation via API

## Technical Specifications

| Spec | Detail |
|------|--------|
| Uptime SLA | 99.9% |
| API Protocol | REST (JSON) |
| Authentication | API key (Bearer token) |
| Webhooks | Real-time delivery events |
| Rate Limits | Plan-dependent (see pricing) |
| SDKs | Node.js, Python, Ruby, Go, PHP |

## Pricing

| Plan | Price | Emails/Month | API Keys | Domains | Key Feature |
|------|-------|-------------|----------|---------|-------------|
| **Free** | $0/mo | 1,000 | 2 | 1 | Get started |
| **Starter** | $25/mo | 50,000 | 5 | 3 | Growing apps |
| **Pro** | $79/mo | 250,000 | 20 | 10 | Dedicated IP |
| **Enterprise** | $299/mo | 1,000,000 | Unlimited | Unlimited | Priority support |

## Use Cases

- **Account confirmations** and password reset emails
- **Order confirmations** and shipping notifications
- **Notification emails** for SaaS applications
- **Invitation emails** and onboarding sequences
- **Alert and monitoring** notifications
- **Two-factor authentication** codes

## How It Works

1. **Authenticate** — Add your domain and verify DNS records
2. **Integrate** — Install the SDK or call the REST API directly
3. **Send** — Send transactional emails with dynamic templates
4. **Track** — Monitor delivery, opens, and clicks in real-time
5. **Optimize** — Use analytics to improve deliverability and engagement

## Platform Integration

- **Publish** → Email campaigns use Dispatch for delivery
- **Audiences** → Send targeted emails to specific segments
- **Analytics** → Email performance data flows into unified dashboards
- **Automate** → Trigger emails from workflow automation

## Who It's For

- **Developers** building applications that send transactional email
- **SaaS companies** needing reliable email infrastructure
- **Product teams** sending user-facing notifications and alerts

## Infrastructure

Email API runs on NolBase's self-hosted Haraka SMTP infrastructure:
- High-throughput SMTP delivery
- IP warming and reputation management
- Dedicated IP addresses (Pro plan and above)
- Automatic retry with exponential backoff
- Queue management with BullMQ

---

*Built by developers, for developers. Send your first email in under 5 minutes.*
