# Recipe API

A production-ready REST API providing structured access to **25,000+ recipes** with USDA-verified nutrition data. No attribution required.

## Why Recipe API?

- **Comprehensive Data**: Full recipes with ingredients, step-by-step instructions, nutrition facts, equipment lists, and chef notes
- **USDA Nutrition**: 32 nutrients per serving, sourced from the USDA database
- **No Attribution**: Use recipes in your app without crediting the source
- **Permanent Ownership**: Higher tiers let you cache and own the data forever
- **Developer-Friendly**: Clean REST endpoints, consistent JSON responses, typed SDKs

## Quick Links

| Resource | URL |
|----------|-----|
| Sign Up | https://recipe-api.com/signup |
| Dashboard | https://recipe-api.com/portal |
| API Documentation | https://recipe-api.com/docs |
| OpenAPI Spec | https://recipe-api.com/openapi.json |

## Getting Started

### 1. Get an API Key

Sign up at [recipe-api.com](https://recipe-api.com/signup) to get your free API key. Keys use the format `rapi_<48 hex characters>`.

### 2. Choose Your Language

Pick a starter kit for your preferred language:

| Language | Repository | Description |
|----------|------------|-------------|
| TypeScript | [recipe-api-typescript](https://github.com/recipe-api/recipe-api-typescript) | Node.js starter with full type definitions |
| Python | [recipe-api-python](https://github.com/recipe-api/recipe-api-python) | Python 3 starter with requests library |
| Go | [recipe-api-golang](https://github.com/recipe-api/recipe-api-golang) | Go starter with typed structs |

### 3. Make Your First Request

Test the API without authentication using the public dinner endpoint:

```bash
curl https://recipe-api.com/api/v1/dinner
```

Or check the service health:

```bash
curl https://recipe-api.com/health
```

## API Overview

### Base URL

```
https://recipe-api.com
```

### Authentication

Pass your API key in the `X-API-Key` header:

```bash
curl -H "X-API-Key: rapi_your_key_here" \
  https://recipe-api.com/api/v1/recipes
```

### Endpoints

#### Public (No Authentication)

| Endpoint | Description |
|----------|-------------|
| `GET /api/v1/dinner` | Returns a sample recipe for testing |
| `GET /health` | Service health check |

#### Discovery (Auth Required, Free)

| Endpoint | Description |
|----------|-------------|
| `GET /api/v1/recipes` | Search and browse recipes (paginated) |
| `GET /api/v1/categories` | List recipe categories |
| `GET /api/v1/cuisines` | List available cuisines |
| `GET /api/v1/dietary-flags` | List dietary filters |

#### Recipe Details (Auth Required, 1 Credit)

| Endpoint | Description |
|----------|-------------|
| `GET /api/v1/recipes/:id` | Get full recipe by UUID |

### Search & Filtering

The `/api/v1/recipes` endpoint supports powerful filtering:

```bash
# Search by keyword
curl -H "X-API-Key: $API_KEY" \
  "https://recipe-api.com/api/v1/recipes?q=chicken+parmesan"

# Filter by cuisine and difficulty
curl -H "X-API-Key: $API_KEY" \
  "https://recipe-api.com/api/v1/recipes?cuisine=Italian&difficulty=Intermediate"

# Filter by nutrition
curl -H "X-API-Key: $API_KEY" \
  "https://recipe-api.com/api/v1/recipes?max_calories=500&min_protein=20"

# Combine multiple dietary flags
curl -H "X-API-Key: $API_KEY" \
  "https://recipe-api.com/api/v1/recipes?dietary=Vegetarian,Gluten-Free"
```

#### Available Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `q` | string | Full-text search query |
| `category` | string | Filter by category (e.g., "Breakfast", "Dinner") |
| `cuisine` | string | Filter by cuisine (e.g., "Italian", "Mexican") |
| `difficulty` | string | Filter by difficulty: Beginner, Intermediate, Advanced |
| `dietary` | string | Comma-separated dietary flags (e.g., "Vegetarian,Dairy-Free") |
| `min_calories` | number | Minimum calories per serving |
| `max_calories` | number | Maximum calories per serving |
| `min_protein` | number | Minimum protein (grams) per serving |
| `max_protein` | number | Maximum protein (grams) per serving |
| `page` | number | Page number (default: 1) |
| `per_page` | number | Results per page (max: 100) |

## Understanding Credits

Recipe API uses a credit system to balance fair usage:

| Action | Cost |
|--------|------|
| Browse, search, list metadata | **Free** |
| Fetch full recipe details | **1 credit** |

Credits reset monthly. Check your usage in the [dashboard](https://recipe-api.com/portal) or via response headers.

## Rate Limits

Rate limits vary by plan:

| Plan | Requests/Day | Requests/Minute |
|------|--------------|-----------------|
| Free | 100 | 10 |
| Hobby | 500 | 30 |
| Starter | 2,000 | 100 |
| Pro | 50,000 | 500 |

When rate limited, you'll receive HTTP 429 with a `Retry-After` header.

## Error Handling

All errors return JSON with `code` and `message` fields:

```json
{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Daily request limit exceeded. Resets at midnight UTC."
  }
}
```

| HTTP Status | Error Code | Description |
|-------------|------------|-------------|
| 401 | `UNAUTHORIZED` | Missing or invalid API key |
| 404 | `NOT_FOUND` | Recipe or resource not found |
| 429 | `RATE_LIMITED` | Too many requests |
| 429 | `UNIQUE_RECIPE_LIMIT_EXCEEDED` | Monthly recipe limit reached |

## Additional Documentation

- [Pricing Guide](./pricing.md) - Detailed plan comparison and data ownership rights
- [Data Schema](./schema.md) - Complete recipe data structure and field definitions

## Commercial Use

- **No attribution required** on any plan
- **Commercial use permitted** on all plans
- **Caching strongly encouraged** - build your own database
- Data resale as a competing API is prohibited
- Full terms at https://recipe-api.com/terms

## Support

- **Email**: paul@recipe-api.com
- **GitHub**: https://github.com/recipe-api
- **Documentation**: https://recipe-api.com/docs

---

Built for developers who need reliable recipe data without the hassle.
