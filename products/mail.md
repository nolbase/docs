# Mail

> Professional email hosting for your organization.

**Module slug:** `mail`
**Category:** Communication
**URL:** `/business-email`

## Overview

NolBase Mail is professional email hosting for teams and organizations. Get custom domain email (you@company.com), a full-featured webmail interface, contacts, calendar, groups, and powerful admin controls — all self-hosted on your infrastructure.

Mail is built for organizations that want reliable, private email without the complexity and cost of Microsoft 365 or Google Workspace. Start free with 5 mailboxes and scale as you grow.

## Key Features

### Custom Domain Email
Use your own domain for professional email:
- Automated DNS verification with step-by-step wizard
- DKIM signing for deliverability
- SPF and DMARC configuration
- Multiple domains per workspace (plan-dependent)

### Full Webmail Interface
A modern, feature-rich webmail experience:
- Threaded conversations with collapsible message view
- Rich text compose with formatting toolbar
- File attachments with drag-and-drop
- Folder management (system + custom folders)
- Labels and filters for organization
- Full-text search across all messages

### Calendar
Integrated calendar for scheduling:
- Event creation with location, description, and recurrence
- Attendee invitations with RSVP tracking
- Multiple calendar views (day, week, month)
- Calendar sharing between team members

### Groups & Aliases
Manage team communication:
- Distribution lists for team-wide emails
- Email aliases for role-based addresses (support@, sales@, info@)
- Posting policies (who can send to the group)
- Member management with admin controls

### Contacts & Address Book
Built-in contact management:
- Contact creation with full details (email, phone, company, notes)
- Import contacts from CSV
- Contact search and filtering
- Shared address books per workspace

### Spam & Virus Protection
Advanced filtering keeps inboxes clean:
- **Rspamd** — Intelligent spam filtering with machine learning
- **ClamAV** — Real-time virus scanning for attachments
- Configurable spam thresholds
- Quarantine management for false positives

### IMAP/POP3 Access
Connect any email client:
- IMAP for synchronized mailbox access (Thunderbird, Outlook, Apple Mail)
- POP3 for download-based access
- SSL/TLS encryption for all connections
- Standard ports (993 IMAP, 995 POP3, 587 SMTP)

### Admin Console
Full control for workspace administrators:
- User and mailbox management
- Storage quota configuration per user
- Security policies (password requirements, session timeouts)
- Domain management and DNS verification
- Usage analytics and storage monitoring
- Account suspension and reactivation

## Pricing

| Plan | Price | Mailboxes | Storage/User | Domains | Key Feature |
|------|-------|-----------|-------------|---------|-------------|
| **Free** | $0/mo | 5 | 1 GB | 1 | Get started |
| **Lite** | $1/user/mo | 50 | 10 GB | 5 | IMAP/POP3 + Groups |
| **Premium** | $4/user/mo | 500 | 50 GB | Unlimited | Calendar + Priority support |

### Send Rate Limits
- Free: 100 emails/hour
- Lite: 500 emails/hour
- Premium: 2,000 emails/hour

## Infrastructure

Mail runs on proven, open-source email infrastructure:

| Component | Technology | Purpose |
|-----------|-----------|---------|
| IMAP/POP3 | Dovecot 2.3.x | Mailbox storage and client access |
| Spam filtering | Rspamd | Machine learning spam detection |
| Virus scanning | ClamAV | Attachment virus scanning |
| SMTP | Haraka (extended) | Inbound mail delivery |
| Database | PostgreSQL | Metadata storage |
| Cache | Redis | Session and performance caching |

## Self-Hosted Advantage

NolBase Mail runs on your infrastructure, giving you:
- **Data sovereignty** — Your email data stays on your servers
- **No vendor lock-in** — Standard IMAP/POP3 means you can migrate anytime
- **Privacy** — No third-party scanning of your emails
- **Compliance** — Meet data residency requirements for your jurisdiction
- **Cost control** — Predictable pricing without per-feature upsells

## How It Works

1. **Add domain** — Verify your domain ownership with DNS records
2. **Create mailboxes** — Set up email accounts for your team
3. **Configure clients** — Connect webmail, desktop clients, or mobile apps
4. **Set policies** — Configure security, quotas, and admin settings
5. **Communicate** — Send and receive email on your custom domain

## Who It's For

- **Teams** that need professional email on their own domain
- **Organizations** wanting email independence from Microsoft and Google
- **Companies** with data privacy and compliance requirements
- **Startups** that want professional email without enterprise pricing

## Comparison

### vs. Google Workspace
- Self-hosted: your data, your servers
- Start free (5 mailboxes vs. $7/user/month)
- No ecosystem lock-in

### vs. Microsoft 365
- Simple and focused vs. bloated enterprise suite
- $1-4/user vs. $6+/user
- No Exchange admin complexity

---

*Professional email that respects your privacy and your budget. Start free with 5 mailboxes.*
