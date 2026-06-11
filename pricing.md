# Recipe API Pricing

Current plans and what they include. Live source of truth: [recipe-api.com/compare](https://recipe-api.com/compare).

## Plans at a Glance

| Feature | Free | Indie | Growth | Scale | Enterprise |
|---------|------|-------|--------|-------|------------|
| **Price** | $0/mo | $39/mo | $249/mo | $999/mo | $4,999/mo |
| **Requests / month** | 3,000 | 15,000 | 60,000 | 300,000 | 3,000,000 |
| **Requests / day** | 100 | 500 | 2,000 | 10,000 | 100,000 |
| **Requests / minute** | 10 | 20 | 60 | 300 | 1,000 |
| **Unique recipes / month** | 25 | 150 | 1,000 | 5,000 | Unlimited |
| **AI generations / day** | 1 | 3 | 10 | 25 | 100 |
| **AI generations / month** | 5 | 50 | 200 | 500 | 2,000 |
| **Caching rights** | None | 24-hour local | 7-day local | Permanent | Unlimited + white-label |
| **Commercial use** | Yes | Yes | Yes | Yes | Yes |
| **Attribution required** | No | No | No | No | No |
| **Card required** | **No** | Yes | Yes | Yes | Yes |

> Legacy Hobby / Starter / Pro plans are grandfathered for existing subscribers and no longer offered.

## Understanding the Usage Model

Recipe API meters three things:

### Requests (rate limiting)
Every API call counts toward your per-minute, daily, and monthly request limits — including free discovery endpoints.

### Unique recipes (content metering)
Fetching a **full recipe** (`GET /api/v1/recipes/:slug`) counts that recipe toward your unique-recipes allowance for the period. Re-fetching the same recipe in the same period does not consume more.

### AI generations (premium)
`POST /api/v1/generate` creates a brand-new structured recipe with full nutrition. Metered separately per day and per month — and **every plan, including Free, now includes generations**.

| Endpoint type | Request cost | Unique recipe | Generation |
|---------------|--------------|---------------|------------|
| `/api/v1/recipes` (search/browse) | 1 request | — | — |
| `/api/v1/categories`, `/cuisines`, `/dietary-flags`, `/ingredients` | 1 request | — | — |
| `/api/v1/recipes/:slug` (full recipe) | 1 request | 1 (first fetch per period) | — |
| `/api/v1/generate` | 1 request | — | 1 |

## Billing Periods

- **Free**: rolling 30-day window — usage counters reset automatically 30 days after they start.
- **Paid**: aligned to your Stripe billing cycle; counters reset each cycle.

## Data Ownership

### Free — evaluate
No persistent caching; ideal for testing, hackathons, and proof-of-concepts before committing.

### Indie — build
24-hour local caching. Right-sized for side projects and early-stage apps.

### Growth — ship
7-day local caching, 1,000 unique recipes/month, 200 generations/month.

### Scale — own
Permanent caching rights: recipes you've fetched are yours to store.

### Enterprise — white-label
Unlimited caching and white-label rights, 100k requests/day. Custom terms available.

## Getting Started

1. Sign up at [recipe-api.com/signup](https://recipe-api.com/signup) — email only, **no card**
2. Click the activation link; your API key is shown once
3. Start with Free to evaluate; upgrade from your [portal](https://recipe-api.com/portal) when you need more

## Tips for Staying Within Limits

### Use discovery endpoints freely
Search and browse (`/api/v1/recipes` list responses) return lightweight summaries and never consume unique-recipe allowance — filter hard before fetching full recipes.

### Fetch full recipes deliberately
Only `GET /api/v1/recipes/:slug` consumes unique-recipe allowance. Fetch the full object when you actually need ingredients/instructions/nutrition.

### Cache within your rights
On Indie and above, cache full recipes locally for the permitted window instead of re-fetching.
