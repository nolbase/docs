# NolBase Architecture

## System Overview

NolBase is an AI-powered ad campaign management platform. It generates marketing content, manages review workflows, deploys campaigns to ad platforms, and provides real-time analytics.

```
Frame (context) --> Forge (AI generation) --> Gate (review/approve)
                                                     |
                                                     v
Signal (data ingest) <-- Pulse (analytics) <-- Deploy (publish)
        |                      |
        v                      v
    Memory (knowledge)    Predict (forecasts) --> Spend (budgets)
```

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Backend | NestJS 10 (TypeScript 5.3+, Node.js 20 LTS) |
| Frontend | Next.js 14 (App Router, React 18, Tailwind CSS) |
| Console | Next.js 14 (Admin panel, port 3003) |
| Database | PostgreSQL 15+ (Prisma 5.8 ORM) |
| Queues | Redis 7+ (BullMQ) |
| Auth | JWT (access + refresh tokens) |
| AI | OpenAI (`gpt-4-turbo-preview`), Anthropic (`claude-sonnet-4-20250514`) |
| Images | DALL-E 3 (via OpenAI SDK) |
| Video | HuMo AI renderer + Remotion 4.0 (adapter pattern) |
| Email | Haraka SMTP server (self-hosted) |
| SDK | `@nolbase/dispatch` — email/SMS SDK for developers |
| Monorepo | npm workspaces (`packages/shared`, `packages/dispatch-sdk`, `console`) |
| Containers | Docker Compose (5 services) |

## Module Map

### Core Pipeline (Forge --> Gate --> Deploy)

| Module | Purpose | Key Services |
|--------|---------|-------------|
| **Frame** | Context & brand setup | PersonaService, FeatureService, BrandRuleService, WebScraperService |
| **Forge** | AI content generation | ForgeService, CreativeService, AIProviderFactory (OpenAI/Anthropic) |
| **Gate** | Review & approval workflows | ReviewService, ApprovalService, WorkflowService |
| **Deploy** | Campaign deployment to ad platforms | CampaignService, DeploymentService, DeployAdapterFactory |

### Analytics Pipeline (Signal --> Pulse --> Predict/Spend)

| Module | Purpose | Key Services |
|--------|---------|-------------|
| **Signal** | OAuth + metric ingestion from ad platforms | SignalService, 4 OAuth providers, 4 sync processors |
| **Pulse** | Real-time analytics, alerts, fatigue detection | MetricsService, AlertService, FatigueService, AttributionService |
| **Predict** | Forecasting & recommendations | PredictService, RecommendationService |
| **Spend** | Budget tracking & allocation | SpendService, AllocationService |

### Supporting Modules

| Module | Purpose | Key Services |
|--------|---------|-------------|
| **Auth** | Authentication (JWT) | AuthService, JwtStrategy |
| **Workspace** | Multi-tenant workspace management | WorkspaceService, InvitationService |
| **Memory** | Knowledge base & embeddings | KnowledgeBlockService, EmbeddingService, VectorSearchService |
| **Video** | Video generation pipeline | VideoService, SceneService, HuMoRenderer |
| **Export** | Format content for platforms | ExportService, FormatterFactory (Meta/Google/LinkedIn/TikTok) |
| **Connect** | Third-party integrations & webhooks | IntegrationService, WebhookService, SyncJobService |
| **Dispatch** | Transactional email/SMS | MessageService, SenderService, TrackingService, DomainService |
| **Automate** | Workflow automation & triggers | WorkflowService, TriggerService, ActionService |
| **ABTest** | A/B testing & statistical analysis | ABTestService, StatisticsService |
| **Audience** | Audience management & platform sync | AudienceService, AudienceAdapterFactory |
| **Analytics** | Legacy analytics aggregation | AnalyticsService, MetricsAggregatorService |

## Data Flow Pipelines

### Content Creation Pipeline
```
Frame (personas, features, brand rules)
  --> Forge (AI generates copy/visual_prompt/video_script)
    --> Gate (team reviews, approves/rejects)
      --> Deploy (attaches to campaign, deploys to Meta/Google/LinkedIn/TikTok)
```

### Creative Types
- **Copy**: `headline`, `body_copy`, `copy`, `text`, `html`
- **Visual**: `image`, `visual_prompt`, `carousel`
- **Video**: `video_script`, `video`

### Analytics Pipeline
```
Ad Platforms (Meta, Google, LinkedIn, TikTok)
  --> Signal (OAuth sync, metric ingestion every 60min)
    --> Pulse (aggregation, trend analysis, fatigue detection, alerts)
      --> Predict (forecasting, budget recommendations)
        --> Spend (budget allocation, utilization tracking)
```

### Video Pipeline
```
Forge (video_script creative)
  --> Video Module (scene structuring, shot planning)
    --> HuMo Renderer (visual generation, motion, audio)
      --> Assembled Video Output
```

### Email Pipeline
```
Dispatch (compose message)
  --> Haraka SMTP (send)
  --> Tracking (open/click pixels)
  --> Bounce/Complaint handling
```

## Queue Architecture (BullMQ)

All queues connect to a shared Redis instance. Cross-module queues enable pipeline communication.

| Queue | Module | Purpose | Retries |
|-------|--------|---------|---------|
| `generation` | Forge | AI content generation | default |
| `export` | Export | Content export formatting | default |
| `analytics` | Analytics | Metrics aggregation | default |
| `signal_meta` | Signal | Meta API data sync | default |
| `signal_google` | Signal | Google Ads data sync | default |
| `signal_linkedin` | Signal | LinkedIn API data sync | default |
| `signal_tiktok` | Signal | TikTok API data sync | default |
| `signal_token-refresh` | Signal | OAuth token refresh | default |
| `pulse_aggregation` | Pulse | Metric aggregation | default |
| `pulse_trends` | Pulse | Trend computation | default |
| `pulse_fatigue` | Pulse | Ad fatigue detection | default |
| `pulse_alerts` | Pulse | Alert evaluation | default |
| `predict_forecasts` | Predict | Budget forecasting | default |
| `predict_recommendations` | Predict | AI recommendations | default |
| `spend_updates` | Spend | Spend data updates | default |
| `spend_utilization` | Spend | Utilization tracking | default |
| `spend_alerts` | Spend | Budget alerts | default |
| `deployment` | Deploy | Campaign deployment | 3 (5s backoff) |
| `schedule` | Deploy | Scheduled deployments | 3 (1s backoff) |
| `video_render` | Video | Video rendering | 3 (5s backoff) |
| `dispatch_email` | Dispatch | Email delivery | default |
| `dispatch_sms` | Dispatch | SMS delivery | default |
| `dispatch_webhook` | Dispatch | Webhook delivery | 1 (manual) |
| `connect_sync` | Connect | Integration sync | default |
| `webhook_delivery` | Connect | Webhook delivery | 1 (manual) |
| `workflow` | Automate | Workflow execution | 3 (1s backoff) |
| `trigger` | Automate | Trigger evaluation | 1 |
| `analysis` | ABTest | Statistical analysis | 3 (5s backoff) |
| `audience_sync` | Audience | Audience platform sync | 3 (5s backoff) |

## Platform Adapter Pattern

Deploy, Audience, and Export modules use a factory + adapter pattern for multi-platform support:

```typescript
// Factory selects adapter by platform
const adapter = adapterFactory.getAdapter('meta'); // or 'google', 'linkedin', 'tiktok'

// All adapters implement the same interface
await adapter.deploy(request);
await adapter.getStatus(campaignId, accountId);
await adapter.pause(campaignId, accountId);
```

Supported platforms: **Meta**, **Google Ads**, **LinkedIn**, **TikTok**

## Frontend Route Structure

| Route | Module | Page |
|-------|--------|------|
| `/forge/generate` | Forge | AI content generation |
| `/forge/creatives` | Forge | Creative library |
| `/forge/creatives/[id]` | Forge | Creative detail/edit |
| `/gate/reviews` | Gate | Review queue |
| `/gate/reviews/[id]` | Gate | Review detail |
| `/deploy` | Deploy | Campaign overview |
| `/deploy/new` | Deploy | Create deployment |
| `/deploy/[id]` | Deploy | Deployment detail |
| `/deploy/calendar` | Deploy | Campaign calendar |
| `/signal` | Signal | Platform connections |
| `/pulse` | Pulse | Analytics dashboard |
| `/pulse/compare` | Pulse | Metric comparison |
| `/predict` | Predict | Forecasts |
| `/spend` | Spend | Budget tracking |
| `/automate` | Automate | Workflow builder |
| `/connect` | Connect | Integrations |
| `/dispatch` | Dispatch | Email/messaging |
| `/export` | Export | Content export |
| `/abtests` | ABTest | A/B testing |
| `/audiences` | Audience | Audience management |
| `/frame/*` | Frame | Personas, features, objections |
| `/settings/*` | Workspace | Team, webhooks, brand rules |

## Console (Admin Panel)

Separate Next.js application for platform administration. Runs on port 3003.

| Route | Purpose |
|-------|---------|
| `/dashboard` | Admin overview |
| `/users` | User management |
| `/workspaces` | Workspace management |
| `/modules` | Module catalog |
| `/bundles` | Bundle management |
| `/billing/*` | Revenue, invoices, subscriptions |
| `/dispatch/*` | Dispatch overview, domains, bounces, IP pools |
| `/audit` | Audit log |
| `/system/*` | Settings, queues, system info |

Requires super admin credentials. Talks to the same backend API as the frontend.

## Shared Packages

| Package | Purpose |
|---------|---------|
| `@nolbase/shared` | Shared TypeScript types and utilities |
| `@nolbase/dispatch` | Email/SMS SDK for external developers (see `packages/dispatch-sdk/README.md`) |

## Authentication

- Global JWT guard on all routes (except `@Public()` decorated endpoints)
- Access token: 15min expiry (configurable via `JWT_ACCESS_EXPIRY`)
- Refresh token: 7d expiry (configurable via `JWT_REFRESH_EXPIRY`)
- Rate limiting: 10 requests/minute on auth endpoints
- OAuth for platform connections (Signal module) with encrypted credential storage
- API keys for Dispatch module (external developer access)
