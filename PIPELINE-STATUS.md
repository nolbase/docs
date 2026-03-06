# NolBase Pipeline Status

Honest assessment of each pipeline's current state. Last updated: 2026-02-28.

## Status Legend

| Status | Meaning |
|--------|---------|
| **Working** | Functional end-to-end in development |
| **Partial** | Core logic works but some pieces are stubbed |
| **Stubbed** | Interfaces exist, implementation returns mock data |
| **Broken** | Pipeline has structural issues preventing use |

---

## Content Creation Pipeline

### Frame --> Forge (AI Generation)

**Status: Working**

- Frame module provides personas, features, use cases, objections, and brand rules
- QuickStart web scraper extracts context from URLs
- Forge generates creatives via OpenAI (GPT-4 Turbo) or Anthropic (Claude Sonnet)
- BullMQ queue (`generation`) handles async generation
- Three creative types supported: `copy`, `visual_prompt`, `video_script`

**What works:**
- Full prompt building from Frame context
- AI generation with model selection
- Creative versioning and variant generation
- Content saved to database with metadata

### Forge --> Gate (Review & Approval)

**Status: Working**

- Creatives flow from Forge to Gate for team review
- Multi-step approval workflows with configurable stages
- Comment threads on creatives
- Status transitions: `draft` --> `pending_review` --> `approved` / `rejected`
- Event-driven notifications on review actions

**What works:**
- Review assignment and workflow routing
- Comment/feedback system
- Approval state machine
- Notification system (email + in-app)

### Gate --> Deploy (Campaign Deployment)

**Status: Partial (was Broken, now fixed)**

- Creative selector now exists in deploy UI (previously missing)
- `creativeIds` are attached to campaigns on creation
- Deployment processor correctly maps creative types (`copy` --> `text`, `visual_prompt` --> `image`, etc.)
- All 4 platform adapters validate text-only campaigns

**What works:**
- Campaign CRUD with creative attachment
- Creative type mapping in deployment processor
- Multi-platform deployment queue with retry
- Deployment status tracking (validating --> uploading_assets --> creating_campaign --> platform_review --> active)

**What's stubbed:**
- All 4 platform deploy adapters (Meta, Google, LinkedIn, TikTok) return mock IDs instead of making real API calls
- Asset uploader skips upload when no `assetUrl` on creatives (correct for text, placeholder for media)
- Platform review polling simulates immediate approval

**To make production-ready:**
1. Implement real Facebook Marketing API calls in MetaDeployAdapter
2. Implement real Google Ads API calls in GoogleDeployAdapter
3. Implement real LinkedIn Campaign Manager API calls in LinkedInDeployAdapter
4. Implement real TikTok Marketing API calls in TikTokDeployAdapter
5. Replace polling with platform webhooks for review status
6. Implement actual file upload for media assets

---

## Analytics Pipeline

### Signal (Platform Data Ingestion)

**Status: Partial**

- OAuth flows for all 4 platforms are implemented with real API URLs
- Token refresh processor handles expiring tokens
- Credential encryption (AES-256-GCM) works
- Metric transformation maps platform-specific fields to unified schema

**What works:**
- OAuth authorization URL generation
- Token exchange and refresh (real API calls)
- Credential storage with encryption
- Metric data model and transformation logic

**What's stubbed:**
- Actual metric fetching (API calls are real, but need valid credentials to test)
- Sync scheduling (cron-based interval configured but not battle-tested)

**To make production-ready:**
1. Set up Meta/Google/LinkedIn/TikTok developer accounts
2. Configure OAuth redirect URIs
3. Test full OAuth flow with real credentials
4. Validate metric ingestion with real campaign data
5. Add error recovery for API rate limits

### Pulse (Analytics & Alerts)

**Status: Partial**

- Metric aggregation processors exist (AggregationProcessor, TrendProcessor)
- Alert evaluation system with threshold rules
- Ad fatigue detection algorithm
- Attribution service for cross-channel analysis
- Comparison service for A/B metric comparison
- Configurable cache TTLs

**What works:**
- Aggregation and trend calculation logic
- Alert rule evaluation
- Fatigue score computation
- Cache layer with configurable TTLs

**What's missing:**
- No real data flowing from Signal (needs connected platforms)
- Dashboard visualizations need real data to validate

### Predict (Forecasting)

**Status: Stubbed**

- RecommendationService and PredictService interfaces exist
- Queue infrastructure set up

**To make production-ready:**
1. Implement forecasting algorithms (time series, regression)
2. Train models on historical campaign data from Pulse
3. Build recommendation engine for budget/targeting optimization

### Spend (Budget Management)

**Status: Stubbed**

- SpendService and AllocationService defined
- Utilization tracking processor exists

**To make production-ready:**
1. Connect to Pulse for real spend data
2. Implement budget allocation algorithms
3. Add spend alerts and pacing logic

---

## Graphics & Animation Pipeline

### Visual Prompt Generation

**Status: Working (text output only)**

- Forge generates `visual_prompt` type creatives
- These are detailed text descriptions for image generation (e.g., "A professional product shot of...")
- Prompts include brand colors, style guidelines, composition notes

**What works:**
- AI-generated visual descriptions/prompts
- Brand-consistent prompt construction from Frame context

**What's NOT implemented:**
- No image generation service (no DALL-E, Midjourney, Stable Diffusion integration)
- `visual_prompt` creatives contain text prompts only, not actual images
- No image storage/CDN pipeline

**To make production-ready:**
1. Add image generation adapter (Flux, DALL-E 3, or Stable Diffusion)
2. Implement image storage (S3/R2 bucket)
3. Connect generated images to creative `assetUrl`
4. Add image review workflow in Gate

### Carousel / Multi-Image

**Status: Stubbed**

- `carousel` creative type exists in the data model
- No carousel layout/composition logic

**To make production-ready:**
1. Implement carousel slide generation (text + image per slide)
2. Add carousel preview in UI
3. Platform-specific carousel formatting in deploy adapters

---

## Video Pipeline

### Script Generation

**Status: Working**

- Forge generates `video_script` type creatives
- Scripts include scene structure, dialogue, visual directions
- Scene and shot models defined in Video module

**What works:**
- AI script generation with brand context
- Scene/shot data model
- Voice profile management

### Video Rendering (HuMo)

**Status: Stubbed**

- HuMoRenderer implements VideoRenderer interface
- Scene-to-render pipeline defined (script --> scenes --> shots --> render)
- BullMQ queue with retry (3 attempts, 5s backoff)
- Render progress simulation exists

**What's stubbed:**
- HuMo API calls return mock render IDs
- No actual video output generated
- Storage URL is configurable but points to HuMo placeholder

**To make production-ready:**
1. Integrate HuMo API (or alternative: Remotion for motion graphics)
2. Implement scene-to-prompt conversion for video AI
3. Add audio/voice generation (F5-TTS or similar)
4. Build video assembly pipeline (Remotion)
5. Set up video storage (S3/R2)
6. Add video preview player in UI

### Recommended Video Stack (Cost-Optimized)

| Tier | Stack | Cost/Video | Use Case |
|------|-------|-----------|----------|
| Free | Remotion (self-hosted) | ~$0.02 | Kinetic text, motion graphics |
| Standard | Self-hosted models (HuMo/Wan2.1) | ~$0.40 | Full video with AI clips |
| Premium | Runway API | ~$1.80 | High-fidelity hero content |

---

## Email Pipeline

### Dispatch --> Haraka

**Status: Partial**

- Full message composition, template rendering, and queue system
- Haraka SMTP server configured with TLS, DKIM
- Tracking pixel generation (open/click)
- Domain verification (SPF, DKIM, DMARC records)
- Bounce and complaint handling

**What works:**
- Message queuing and delivery via Haraka
- Template rendering with variable substitution
- Domain DNS record generation
- API key management for external sends

**What needs testing:**
- End-to-end email delivery with real domains
- DKIM signing verification
- Bounce webhook processing
- Deliverability monitoring

---

## Export Pipeline

**Status: Partial**

- FormatterFactory with platform-specific formatters (Meta, Google, LinkedIn, TikTok)
- Creative selection and batch export

**What's stubbed:**
- File storage uses data URLs (base64 inline) instead of actual file storage
- No CDN or S3 integration

**To make production-ready:**
1. Implement file storage adapter (S3, R2, or local)
2. Generate downloadable export files
3. Platform-specific format validation

---

## Automation Pipeline

### Automate (Workflows & Triggers)

**Status: Partial**

- Workflow builder with node-based execution
- Trigger evaluation system (event-based, scheduled, conditional)
- Action execution engine
- Integration with Deploy module

**What works:**
- Workflow CRUD and execution engine
- Trigger registration and evaluation
- Action dispatching

**What needs work:**
- More action types (currently limited set)
- Complex conditional logic
- Error recovery in multi-step workflows

---

## Integration Pipeline

### Connect (Third-Party Integrations)

**Status: Partial**

- Integration management with API key storage
- Webhook delivery with retry
- Sync job scheduling
- Mapping rules for data transformation

**What works:**
- Integration CRUD
- Webhook registration and delivery
- Sync job execution framework

---

## A/B Testing Pipeline

### ABTest

**Status: Stubbed**

- Test creation and management UI
- Statistical analysis processor (queue-based)

**To make production-ready:**
1. Connect to Deploy for traffic splitting
2. Implement statistical significance calculations
3. Automatic winner selection logic

---

## Summary Table

| Pipeline | Status | Priority |
|----------|--------|----------|
| Frame --> Forge (AI content) | **Working** | - |
| Forge --> Gate (review) | **Working** | - |
| Gate --> Deploy (campaign) | **Partial** | High - needs real platform APIs |
| Signal (data ingest) | **Partial** | High - needs real OAuth setup |
| Pulse (analytics) | **Partial** | Medium - needs Signal data |
| Visual/Image generation | **Text prompts only** | Medium - needs image gen API |
| Video rendering | **Stubbed** | Low - HuMo/Remotion integration |
| Dispatch (email) | **Partial** | Medium - needs domain setup |
| Export | **Partial** | Low - needs file storage |
| Predict/Spend | **Stubbed** | Low - needs Pulse data first |
| Automate | **Partial** | Low |
| ABTest | **Stubbed** | Low |
