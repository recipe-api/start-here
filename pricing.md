# Pricing

Recipe API offers four tiers designed to scale from evaluation to production. The key differentiator is **data ownership** - higher tiers grant permanent rights to cache and use the data you fetch.

## Plan Comparison

| Feature | Free | Hobby | Starter | Pro |
|---------|------|-------|---------|-----|
| **Price** | $0/month | $9/month | $59/month | $199/month |
| **Monthly Recipes** | 100 | 500 | 2,500 | 10,000 |
| **Daily Requests** | 100 | 500 | 2,000 | 50,000 |
| **Requests/Minute** | 10 | 30 | 100 | 500 |
| **Caching Rights** | Evaluation only | 24-hour local | Permanent ownership | Unlimited + white-label |
| **Commercial Use** | Yes | Yes | Yes | Yes |
| **Attribution Required** | No | No | No | No |

## Understanding Credits vs Requests

Recipe API has two usage metrics:

### Requests (Rate Limiting)
Total API calls per day/minute. Applies to all endpoints including free discovery endpoints.

### Credits (Recipe Fetching)
Only consumed when fetching full recipe details via `/api/v1/recipes/:id`. Each recipe costs 1 credit.

| Endpoint Type | Request Cost | Credit Cost |
|---------------|--------------|-------------|
| `/api/v1/recipes` (search/browse) | 1 request | 0 credits |
| `/api/v1/categories` | 1 request | 0 credits |
| `/api/v1/cuisines` | 1 request | 0 credits |
| `/api/v1/dietary-flags` | 1 request | 0 credits |
| `/api/v1/recipes/:id` (full details) | 1 request | 1 credit |

## Data Ownership Explained

This is what sets Recipe API apart from other data providers.

### Free Tier
- **Evaluation only** - data cannot be stored persistently
- Ideal for testing the API before committing
- Perfect for hackathons and proof-of-concepts

### Hobby Tier
- **24-hour local caching** - store recipe data for up to 24 hours
- Good for personal projects with real-time fetching
- Recipes must be re-fetched after cache expires

### Starter Tier
- **Permanent data ownership** - once you fetch a recipe, it's yours forever
- Build your own local database of recipes
- No need to re-fetch previously retrieved recipes
- Ideal for startups and growing applications

### Pro Tier
- **Everything in Starter, plus:**
- **White-label rights** - rebrand recipes as your own content
- **Unlimited caching** - no restrictions on how you store or serve data
- Perfect for agencies, enterprise apps, and high-volume services

## Which Plan Should You Choose?

### Choose Free if:
- You're evaluating the API
- Building a prototype or demo
- Learning how the API works

### Choose Hobby if:
- Running a personal project
- Building a small app with live data
- Don't need to cache recipes long-term

### Choose Starter if:
- Building a production application
- Want to build a local recipe database
- Need permanent access to fetched recipes
- Running a small business or startup

### Choose Pro if:
- High-volume application (>2,500 recipes/month)
- Need white-label/rebranding rights
- Enterprise or agency use case
- Building a recipe platform or service

## Cost Optimization Tips

### 1. Cache Aggressively (Starter/Pro)
Once you fetch a recipe, store it locally. You never need to pay for it again.

```javascript
async function getRecipe(id) {
  // Check local cache first
  const cached = await db.recipes.findById(id);
  if (cached) return cached;

  // Fetch from API and cache
  const recipe = await api.getRecipe(id);
  await db.recipes.insert(recipe);
  return recipe;
}
```

### 2. Use Discovery Endpoints Freely
Search, browse, and filter endpoints are free. Use them to let users explore before committing a credit.

### 3. Batch Your Fetches
If building a database, fetch recipes in planned batches rather than on-demand. This helps track usage.

### 4. Monitor Usage
Check the response headers or dashboard to track remaining credits:

```
X-Monthly-Remaining: 2450
X-Daily-Remaining: 1823
```

## Overage & Limits

- **No automatic overage charges** - when you hit your limit, requests return HTTP 429
- **Credits reset monthly** on your billing date
- **Daily rate limits reset** at midnight UTC
- Upgrade anytime to increase limits immediately

## Billing FAQ

**Can I change plans mid-cycle?**
Yes. Upgrades take effect immediately with prorated billing. Downgrades apply at the next billing cycle.

**What happens if I exceed my limit?**
API returns HTTP 429 with `UNIQUE_RECIPE_LIMIT_EXCEEDED` or `RATE_LIMITED`. No surprise charges.

**Do unused credits roll over?**
No. Credits reset monthly on your billing date.

**Is there an annual discount?**
Contact paul@recipe-api.com for annual billing options.

**Can I get a custom enterprise plan?**
Yes. Contact us for custom limits, SLAs, and pricing for high-volume use cases.

## Get Started

1. Sign up at [recipe-api.com/signup](https://recipe-api.com/signup)
2. Start with the Free tier to evaluate
3. Upgrade when you're ready for production

---

Questions? Email paul@recipe-api.com
