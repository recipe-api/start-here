# Data Schema

This document describes the complete data structure returned by Recipe API endpoints.

## Recipe Object (Full)

When you fetch a recipe via `GET /api/v1/recipes/:id`, you receive the complete recipe object:

```json
{
  "id": "c0582be3-10bf-4077-ba54-b6ff6f236e53",
  "name": "Classic Chicken Parmesan",
  "description": "Crispy breaded chicken cutlets topped with marinara and melted mozzarella.",
  "category": "Dinner",
  "cuisine": "Italian",
  "difficulty": "Intermediate",
  "tags": ["chicken", "italian", "comfort food", "baked"],
  "yields": {
    "amount": 4,
    "unit": "servings"
  },
  "timing": {
    "active": "PT30M",
    "passive": "PT25M",
    "total": "PT55M"
  },
  "ingredients": [...],
  "instructions": [...],
  "nutrition": {...},
  "equipment": [...],
  "tips": [...],
  "storage": {...},
  "chef_notes": "..."
}
```

## Field Reference

### Basic Metadata

| Field | Type | Description |
|-------|------|-------------|
| `id` | string (UUID) | Unique recipe identifier |
| `name` | string | Recipe title |
| `description` | string | Brief description of the dish |
| `category` | string | Primary category (Breakfast, Lunch, Dinner, etc.) |
| `cuisine` | string | Cuisine type (Italian, Mexican, Thai, etc.) |
| `difficulty` | string | Skill level: `Beginner`, `Intermediate`, or `Advanced` |
| `tags` | string[] | Searchable tags and keywords |

### Yields

Describes how much the recipe makes.

```json
{
  "yields": {
    "amount": 4,
    "unit": "servings"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `amount` | number | Quantity produced |
| `unit` | string | Unit of measurement (servings, pieces, cups, etc.) |

### Timing

All times use ISO 8601 duration format.

```json
{
  "timing": {
    "active": "PT30M",
    "passive": "PT25M",
    "total": "PT55M"
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `active` | string (ISO 8601) | Hands-on preparation time |
| `passive` | string (ISO 8601) | Inactive time (baking, marinating, resting) |
| `total` | string (ISO 8601) | Total time from start to finish |

**ISO 8601 Duration Examples:**
- `PT30M` = 30 minutes
- `PT1H` = 1 hour
- `PT1H30M` = 1 hour 30 minutes
- `PT2H45M` = 2 hours 45 minutes

### Ingredients

Ingredients are organized into groups, each with a list of items.

```json
{
  "ingredients": [
    {
      "group": "Chicken",
      "items": [
        {
          "name": "boneless skinless chicken breasts",
          "quantity": 4,
          "unit": "pieces",
          "preparation": "pounded to 1/2-inch thickness",
          "substitutions": [
            {
              "name": "chicken thighs",
              "notes": "Increase cooking time by 5 minutes"
            }
          ]
        },
        {
          "name": "all-purpose flour",
          "quantity": 0.5,
          "unit": "cup",
          "preparation": "for dredging"
        }
      ]
    },
    {
      "group": "Topping",
      "items": [
        {
          "name": "marinara sauce",
          "quantity": 2,
          "unit": "cups"
        },
        {
          "name": "mozzarella cheese",
          "quantity": 8,
          "unit": "oz",
          "preparation": "shredded"
        }
      ]
    }
  ]
}
```

#### Ingredient Group

| Field | Type | Description |
|-------|------|-------------|
| `group` | string | Group name (e.g., "Sauce", "Dough", "Topping") |
| `items` | IngredientItem[] | List of ingredients in this group |

#### Ingredient Item

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Ingredient name |
| `quantity` | number | Yes | Amount needed |
| `unit` | string | Yes | Unit of measurement |
| `preparation` | string | No | Prep instructions (diced, minced, etc.) |
| `substitutions` | Substitution[] | No | Alternative ingredients |

#### Substitution

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Alternative ingredient name |
| `notes` | string | Adjustment notes for using this substitution |

### Instructions

Step-by-step cooking instructions with structured metadata.

```json
{
  "instructions": [
    {
      "step": 1,
      "text": "Preheat oven to 425°F (220°C). Line a baking sheet with parchment paper.",
      "action": "preheat",
      "temperature": {
        "value": 425,
        "unit": "F"
      }
    },
    {
      "step": 2,
      "text": "Season chicken breasts with salt and pepper. Dredge in flour, shaking off excess.",
      "action": "season"
    },
    {
      "step": 3,
      "text": "Heat oil in a large skillet over medium-high heat. Cook chicken until golden brown, about 3-4 minutes per side.",
      "action": "cook",
      "doneness_cue": "golden brown on both sides"
    },
    {
      "step": 4,
      "text": "Transfer chicken to prepared baking sheet. Top each piece with marinara and mozzarella.",
      "action": "assemble"
    },
    {
      "step": 5,
      "text": "Bake until cheese is melted and bubbly, about 15-20 minutes.",
      "action": "bake",
      "temperature": {
        "value": 425,
        "unit": "F"
      },
      "doneness_cue": "cheese melted and bubbly, internal temp 165°F"
    }
  ]
}
```

#### Instruction Step

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `step` | number | Yes | Step number (1-indexed) |
| `text` | string | Yes | Human-readable instruction |
| `action` | string | No | Action keyword (preheat, chop, mix, bake, etc.) |
| `temperature` | Temperature | No | Cooking temperature if applicable |
| `doneness_cue` | string | No | Visual/physical indicator of completion |

#### Temperature

| Field | Type | Description |
|-------|------|-------------|
| `value` | number | Temperature value |
| `unit` | string | Unit: `F` (Fahrenheit) or `C` (Celsius) |

### Nutrition

Comprehensive nutrition data per serving, sourced from USDA.

```json
{
  "nutrition": {
    "serving_size": "1 piece",
    "calories": 485,
    "macros": {
      "total_fat": { "value": 22, "unit": "g", "daily_value": 28 },
      "saturated_fat": { "value": 8, "unit": "g", "daily_value": 40 },
      "trans_fat": { "value": 0, "unit": "g" },
      "cholesterol": { "value": 145, "unit": "mg", "daily_value": 48 },
      "sodium": { "value": 890, "unit": "mg", "daily_value": 39 },
      "total_carbohydrate": { "value": 24, "unit": "g", "daily_value": 9 },
      "dietary_fiber": { "value": 2, "unit": "g", "daily_value": 7 },
      "total_sugars": { "value": 4, "unit": "g" },
      "added_sugars": { "value": 1, "unit": "g", "daily_value": 2 },
      "protein": { "value": 48, "unit": "g", "daily_value": 96 }
    },
    "vitamins": {
      "vitamin_a": { "value": 520, "unit": "mcg", "daily_value": 58 },
      "vitamin_c": { "value": 8, "unit": "mg", "daily_value": 9 },
      "vitamin_d": { "value": 0.2, "unit": "mcg", "daily_value": 1 },
      "vitamin_e": { "value": 2.1, "unit": "mg", "daily_value": 14 },
      "vitamin_k": { "value": 12, "unit": "mcg", "daily_value": 10 },
      "thiamin": { "value": 0.3, "unit": "mg", "daily_value": 25 },
      "riboflavin": { "value": 0.4, "unit": "mg", "daily_value": 31 },
      "niacin": { "value": 18, "unit": "mg", "daily_value": 113 },
      "vitamin_b6": { "value": 1.2, "unit": "mg", "daily_value": 71 },
      "folate": { "value": 45, "unit": "mcg", "daily_value": 11 },
      "vitamin_b12": { "value": 1.1, "unit": "mcg", "daily_value": 46 },
      "pantothenic_acid": { "value": 2.1, "unit": "mg", "daily_value": 42 }
    },
    "minerals": {
      "calcium": { "value": 380, "unit": "mg", "daily_value": 29 },
      "iron": { "value": 2.5, "unit": "mg", "daily_value": 14 },
      "magnesium": { "value": 52, "unit": "mg", "daily_value": 12 },
      "phosphorus": { "value": 580, "unit": "mg", "daily_value": 46 },
      "potassium": { "value": 620, "unit": "mg", "daily_value": 13 },
      "zinc": { "value": 3.2, "unit": "mg", "daily_value": 29 },
      "copper": { "value": 0.15, "unit": "mg", "daily_value": 17 },
      "manganese": { "value": 0.4, "unit": "mg", "daily_value": 17 },
      "selenium": { "value": 45, "unit": "mcg", "daily_value": 82 }
    }
  }
}
```

#### Nutrition Overview

| Field | Type | Description |
|-------|------|-------------|
| `serving_size` | string | Human-readable serving size |
| `calories` | number | Calories per serving |
| `macros` | object | Macronutrient breakdown |
| `vitamins` | object | Vitamin content |
| `minerals` | object | Mineral content |

#### Nutrient Value

Each nutrient follows this structure:

| Field | Type | Description |
|-------|------|-------------|
| `value` | number | Amount of the nutrient |
| `unit` | string | Unit (g, mg, mcg) |
| `daily_value` | number | Percentage of daily recommended value (when available) |

#### Complete Nutrient List (32 nutrients)

**Macronutrients:**
- total_fat, saturated_fat, trans_fat
- cholesterol, sodium
- total_carbohydrate, dietary_fiber, total_sugars, added_sugars
- protein

**Vitamins:**
- vitamin_a, vitamin_c, vitamin_d, vitamin_e, vitamin_k
- thiamin, riboflavin, niacin, vitamin_b6, folate, vitamin_b12
- pantothenic_acid

**Minerals:**
- calcium, iron, magnesium, phosphorus, potassium
- zinc, copper, manganese, selenium

### Equipment

List of required cooking equipment.

```json
{
  "equipment": [
    "Large oven-safe skillet",
    "Baking sheet",
    "Parchment paper",
    "Meat thermometer",
    "Tongs"
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `equipment` | string[] | List of required tools and equipment |

### Tips & Troubleshooting

Common issues and solutions.

```json
{
  "tips": [
    {
      "issue": "Breading falls off during cooking",
      "solution": "Make sure chicken is completely dry before dredging. Press breading firmly and let rest 5 minutes before cooking."
    },
    {
      "issue": "Chicken is dry",
      "solution": "Don't overcook. Use a meat thermometer and remove from oven at 165°F internal temperature."
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `issue` | string | Common problem description |
| `solution` | string | How to fix or prevent the issue |

### Storage

Storage and reheating instructions.

```json
{
  "storage": {
    "refrigerator": {
      "duration": "3-4 days",
      "instructions": "Store in airtight container. Keep sauce separate if possible."
    },
    "freezer": {
      "duration": "2-3 months",
      "instructions": "Freeze without cheese topping. Add fresh cheese when reheating."
    },
    "reheating": "Reheat in 375°F oven for 15-20 minutes until heated through. Broil last 2 minutes for crispy top."
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `refrigerator.duration` | string | How long it keeps refrigerated |
| `refrigerator.instructions` | string | Refrigerator storage tips |
| `freezer.duration` | string | How long it keeps frozen |
| `freezer.instructions` | string | Freezer storage tips |
| `reheating` | string | Reheating instructions |

### Chef Notes

Additional tips from the recipe developer.

```json
{
  "chef_notes": "For extra crispy chicken, use panko breadcrumbs and add 2 tablespoons of grated Parmesan to the breading mixture. Let the breaded chicken rest for 10 minutes before frying to help the coating adhere better."
}
```

| Field | Type | Description |
|-------|------|-------------|
| `chef_notes` | string | Professional tips and variations |

---

## Recipe Summary (List/Search)

When browsing or searching via `GET /api/v1/recipes`, you receive a condensed version:

```json
{
  "data": [
    {
      "id": "c0582be3-10bf-4077-ba54-b6ff6f236e53",
      "name": "Classic Chicken Parmesan",
      "description": "Crispy breaded chicken cutlets topped with marinara and melted mozzarella.",
      "category": "Dinner",
      "cuisine": "Italian",
      "difficulty": "Intermediate",
      "total_time": "PT55M",
      "calories": 485,
      "tags": ["chicken", "italian", "comfort food", "baked"]
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 156,
    "total_pages": 8
  }
}
```

### Summary Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Recipe UUID (use to fetch full details) |
| `name` | string | Recipe title |
| `description` | string | Brief description |
| `category` | string | Primary category |
| `cuisine` | string | Cuisine type |
| `difficulty` | string | Skill level |
| `total_time` | string | Total cooking time (ISO 8601) |
| `calories` | number | Calories per serving |
| `tags` | string[] | Searchable tags |

### Pagination Meta

| Field | Type | Description |
|-------|------|-------------|
| `page` | number | Current page number |
| `per_page` | number | Results per page |
| `total` | number | Total matching recipes |
| `total_pages` | number | Total available pages |

**Note:** Search results are capped at 500 total results (paginated).

---

## Metadata Endpoints

### Categories

`GET /api/v1/categories`

```json
{
  "data": [
    { "name": "Appetizer", "count": 1250 },
    { "name": "Breakfast", "count": 2100 },
    { "name": "Brunch", "count": 890 },
    { "name": "Dessert", "count": 3200 },
    { "name": "Dinner", "count": 8500 },
    { "name": "Lunch", "count": 4200 },
    { "name": "Side Dish", "count": 2800 },
    { "name": "Snack", "count": 1560 }
  ]
}
```

### Cuisines

`GET /api/v1/cuisines`

```json
{
  "data": [
    { "name": "American", "count": 5200 },
    { "name": "Chinese", "count": 1800 },
    { "name": "French", "count": 1200 },
    { "name": "Indian", "count": 1500 },
    { "name": "Italian", "count": 2800 },
    { "name": "Japanese", "count": 980 },
    { "name": "Mexican", "count": 2100 },
    { "name": "Thai", "count": 750 }
  ]
}
```

### Dietary Flags

`GET /api/v1/dietary-flags`

```json
{
  "data": [
    { "name": "Dairy-Free", "count": 4200 },
    { "name": "Gluten-Free", "count": 5800 },
    { "name": "Keto", "count": 1200 },
    { "name": "Low-Carb", "count": 2400 },
    { "name": "Nut-Free", "count": 6100 },
    { "name": "Paleo", "count": 980 },
    { "name": "Vegan", "count": 2800 },
    { "name": "Vegetarian", "count": 4500 }
  ]
}
```

---

## TypeScript Types

For TypeScript projects, here's the complete type definition:

```typescript
interface Recipe {
  id: string;
  name: string;
  description: string;
  category: string;
  cuisine: string;
  difficulty: 'Beginner' | 'Intermediate' | 'Advanced';
  tags: string[];
  yields: {
    amount: number;
    unit: string;
  };
  timing: {
    active: string;   // ISO 8601 duration
    passive: string;  // ISO 8601 duration
    total: string;    // ISO 8601 duration
  };
  ingredients: IngredientGroup[];
  instructions: InstructionStep[];
  nutrition: Nutrition;
  equipment: string[];
  tips: TroubleshootingTip[];
  storage: StorageInfo;
  chef_notes: string;
}

interface IngredientGroup {
  group: string;
  items: IngredientItem[];
}

interface IngredientItem {
  name: string;
  quantity: number;
  unit: string;
  preparation?: string;
  substitutions?: Substitution[];
}

interface Substitution {
  name: string;
  notes: string;
}

interface InstructionStep {
  step: number;
  text: string;
  action?: string;
  temperature?: Temperature;
  doneness_cue?: string;
}

interface Temperature {
  value: number;
  unit: 'F' | 'C';
}

interface Nutrition {
  serving_size: string;
  calories: number;
  macros: Record<string, NutrientValue>;
  vitamins: Record<string, NutrientValue>;
  minerals: Record<string, NutrientValue>;
}

interface NutrientValue {
  value: number;
  unit: string;
  daily_value?: number;
}

interface TroubleshootingTip {
  issue: string;
  solution: string;
}

interface StorageInfo {
  refrigerator: {
    duration: string;
    instructions: string;
  };
  freezer: {
    duration: string;
    instructions: string;
  };
  reheating: string;
}
```

---

## OpenAPI Specification

For the complete API specification including all endpoints and schemas, see:

https://recipe-api.com/openapi.json
