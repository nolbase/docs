# NolBase Environment Variable Reference

All environment variables used across the system. Variables marked **required** must be set for the application to start. Variables with defaults will use the listed default if not set.

> Canonical source: `backend/.env.example` and `frontend/.env.example`

---

## App / General

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `NODE_ENV` | No | `development` | Runtime environment |
| `API_PORT` | No | `3001` | Backend HTTP port |
| `APP_URL` | No | `http://nolbase.test:8080` | Public application URL |
| `APP_NAME` | No | `NolBase` | Application display name |
| `FRONTEND_URL` | No | `http://localhost:3000` | Frontend URL (OAuth redirects) |
| `CORS_ORIGINS` | No | `http://localhost:3000` | Comma-separated allowed CORS origins |
| `SWAGGER_DESCRIPTION` | No | `NolBase API Documentation` | Swagger docs description |
| `SWAGGER_VERSION` | No | `1.0` | Swagger docs version |

## Database

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DATABASE_URL` | **Yes** | — | PostgreSQL connection string. Recommended: append `?connection_limit=25&pool_timeout=10` |

## Redis

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `REDIS_HOST` | No | `localhost` | Redis hostname |
| `REDIS_PORT` | No | `6379` | Redis port |

## Authentication (JWT)

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `JWT_SECRET` | **Yes** | — | JWT signing secret |
| `JWT_REFRESH_SECRET` | **Yes** | — | JWT refresh token secret |
| `JWT_ACCESS_EXPIRY` | No | `15m` | Access token expiry |
| `JWT_REFRESH_EXPIRY` | No | `7d` | Refresh token expiry |

## AI Providers

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `OPENAI_API_KEY` | **Yes** | — | OpenAI API key |
| `ANTHROPIC_API_KEY` | **Yes** | — | Anthropic API key |
| `OPENAI_MODEL` | No | `gpt-4-turbo-preview` | Default OpenAI model |
| `ANTHROPIC_MODEL` | No | `claude-sonnet-4-20250514` | Default Anthropic model |

## Platform OAuth (Signal)

### Meta Ads

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `META_APP_ID` | Yes* | — | Meta (Facebook) App ID |
| `META_APP_SECRET` | Yes* | — | Meta App Secret |
| `META_REDIRECT_URI` | Yes* | — | Meta OAuth redirect URI |
| `META_API_VERSION` | No | `v18.0` | Meta Marketing API version |

### Google Ads

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `GOOGLE_CLIENT_ID` | Yes* | — | Google OAuth client ID |
| `GOOGLE_CLIENT_SECRET` | Yes* | — | Google OAuth client secret |
| `GOOGLE_REDIRECT_URI` | Yes* | — | Google OAuth redirect URI |
| `GOOGLE_ADS_API_VERSION` | No | `v15` | Google Ads API version |

### LinkedIn Ads

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `LINKEDIN_CLIENT_ID` | Yes* | — | LinkedIn OAuth client ID |
| `LINKEDIN_CLIENT_SECRET` | Yes* | — | LinkedIn OAuth client secret |
| `LINKEDIN_REDIRECT_URI` | Yes* | — | LinkedIn OAuth redirect URI |
| `LINKEDIN_API_VERSION` | No | `202401` | LinkedIn API version |

### TikTok Ads

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `TIKTOK_APP_ID` | Yes* | — | TikTok App ID |
| `TIKTOK_APP_SECRET` | Yes* | — | TikTok App Secret |
| `TIKTOK_REDIRECT_URI` | Yes* | — | TikTok OAuth redirect URI |
| `TIKTOK_API_VERSION` | No | `v1.3` | TikTok API version |

> *Platform OAuth variables are only required if you want to connect that specific platform.

## Signal Sync

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `SIGNAL_SYNC_INTERVAL_MINUTES` | No | `60` | Metric sync interval (minutes) |
| `SIGNAL_RETENTION_DAYS` | No | `90` | How long to retain raw metrics |

## Encryption

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `CREDENTIALS_ENCRYPTION_KEY` | Yes* | — | 32-byte hex key for OAuth credential encryption |
| `CREDENTIAL_ENCRYPTION_SALT` | No | `nolbase-credential-encryption-v1` | PBKDF2 salt for credential hashing |

> *Required if using platform OAuth connections.

## Dispatch (Email/SMS)

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `SMTP_HOST` | Yes* | — | SMTP server host |
| `SMTP_PORT` | No | `587` | SMTP server port |
| `SMTP_USER` | Yes* | — | SMTP username |
| `SMTP_PASS` | Yes* | — | SMTP password |
| `EMAIL_FROM` | No | `noreply@nolbase.io` | Default sender email |
| `DISPATCH_TRACKING_URL` | No | `http://localhost:3000/t` | Email tracking pixel base URL |
| `DISPATCH_COMPANY_DOMAIN` | No | `nolbase.io` | Company domain for SPF/DKIM/DMARC |

> *Required if using email features.

## Image Generation

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `IMAGE_GEN_PROVIDER` | No | `openai` | Image generation provider |
| `DALL_E_MODEL` | No | `dall-e-3` | DALL-E model version |
| `DALL_E_DEFAULT_SIZE` | No | `1024x1024` | Default image size |
| `DALL_E_DEFAULT_QUALITY` | No | `standard` | Default image quality |

## Storage

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `STORAGE_PROVIDER` | No | `local` | Storage provider (`local` or S3-compatible) |
| `STORAGE_LOCAL_DIR` | No | `./uploads` | Local upload directory |
| `STORAGE_BASE_URL` | No | `http://localhost:3001/api/v1/storage` | Public URL for stored files |

## Video / HuMo

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `HUMO_API_URL` | No | `https://api.humo.ai` | HuMo API endpoint |
| `HUMO_API_KEY` | Yes* | — | HuMo API key |
| `HUMO_STORAGE_URL` | No | `https://storage.humo.ai/renders/` | HuMo render storage URL |
| `REMOTION_CONCURRENCY` | No | `1` | Remotion video render concurrency |

> *Required if using video generation.

## Web Scraper (Frame QuickStart)

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `SCRAPER_USER_AGENT` | No | `NolBase/1.0 (QuickStart Bot)` | HTTP User-Agent for web scraping |
| `SCRAPER_TIMEOUT_MS` | No | `15000` | Scraper fetch timeout (ms) |
| `SCRAPER_MAX_TEXT_LENGTH` | No | `30000` | Max text characters to extract |

## Cache TTLs

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `PULSE_CACHE_TTL_MS` | No | `300000` | Pulse module cache TTL (ms) |
| `METRICS_DASHBOARD_CACHE_TTL` | No | `120` | Dashboard metrics cache (seconds) |
| `METRICS_OVERVIEW_CACHE_TTL` | No | `180` | Overview metrics cache (seconds) |
| `METRICS_TREND_CACHE_TTL` | No | `300` | Trend data cache (seconds) |

## Deploy

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DEPLOY_POLL_INTERVAL_MS` | No | `5000` | Platform review poll interval (ms) |

## Load Testing

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `LOAD_TEST_BASE_URL` | No | `http://localhost:3001/api/v1` | Base URL for k6 load tests |
| `RATE_LIMIT_MAX_REQUESTS` | No | `10` | Auth rate limit (increase during load tests) |

## Haraka SMTP Server

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `INTERNAL_API_SECRET` | **Yes** | — | Secret for Haraka → Backend API calls |
| `NOLBASE_API_HOST` | No | `backend` | Backend hostname (from Haraka) |
| `NOLBASE_API_PORT` | No | `3001` | Backend API port (from Haraka) |
| `HARAKA_SMTP_LISTEN` | No | `0.0.0.0:25,0.0.0.0:587` | SMTP listen addresses |

## Frontend

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `NEXT_PUBLIC_API_URL` | No | `http://nolbase.test:8080/api/v1` | Backend API URL |
| `NEXT_PUBLIC_APP_NAME` | No | `NolBase` | App display name |
| `NEXT_PUBLIC_APP_URL` | No | `http://nolbase.test:8080` | Public app URL |

## Console (Admin Panel)

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `NEXT_PUBLIC_API_URL` | No | `http://console.nolbase.test:8080/api/v1` | Backend API URL for console |
| `NEXT_PUBLIC_APP_NAME` | No | `NolBase Console` | Console display name |
| `NEXT_PUBLIC_APP_URL` | No | `http://console.nolbase.test:8080` | Console public URL |
| `NEXT_PUBLIC_MAIN_APP_URL` | No | `http://nolbase.test:8080` | Main app URL |

## Docker Compose Ports

When running with `docker-compose up`, services are exposed on different external ports:

| Service | Internal Port | External Port | URL |
|---------|--------------|---------------|-----|
| Backend | 3001 | 4001 | `http://localhost:4001/api/v1` |
| Frontend | 3000 | 4011 | `http://localhost:4011` |
| PostgreSQL | 5432 | 5501 | `localhost:5501` |
| Redis | 6379 | 6391 | `localhost:6391` |
| Haraka SMTP | 25, 587 | 25, 587 | — |

Override Docker defaults via root `.env`:

| Variable | Docker Default | Description |
|----------|---------------|-------------|
| `NEXT_PUBLIC_API_URL` | `http://localhost:4001/api/v1` | Backend API URL for frontend |
| `APP_URL` | `http://localhost:4011` | Public application URL |
| `JWT_SECRET` | `change-me-in-production` | JWT secret |
| `JWT_REFRESH_SECRET` | `change-me-in-production-refresh` | JWT refresh secret |
| `OPENAI_API_KEY` | — | OpenAI API key |
| `ANTHROPIC_API_KEY` | — | Anthropic API key |
| `INTERNAL_API_SECRET` | `internal-secret-change-me` | Haraka internal API secret |
