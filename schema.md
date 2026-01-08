# Data Schema

This document describes the complete data structure returned by Recipe API endpoints, based on real API responses.

## Recipe Object (Full)

When you fetch a recipe via `GET /api/v1/recipes/:id` or the public `GET /api/v1/dinner` endpoint, you receive the complete recipe object.

### Real Example

```json
{
  "id": "a066f472-ed0c-46ea-8e2c-a0053c3183a8",
  "name": "Texas Chili con Carne",
  "description": "A thick, beef-based stew featuring tender cubes of meat in a rich sauce made from reconstituted whole chilies and aromatic spices without beans or tomatoes.",
  "category": "Dinner",
  "cuisine": "American",
  "difficulty": "Intermediate",
  "tags": [
    "Beef",
    "Slow-Cooked",
    "High-Protein",
    "Southwestern"
  ],
  "meta": {
    "active_time": "PT20M",
    "passive_time": "PT1H40M",
    "total_time": "PT2H",
    "overnight_required": false,
    "yields": "4 servings",
    "yield_count": 4,
    "serving_size_g": 300
  },
  "dietary": {
    "flags": [
      "Gluten-Free",
      "Dairy-Free",
      "Egg-Free",
      "Nut-Free",
      "Peanut-Free",
      "Soy-Free"
    ],
    "not_suitable_for": []
  },
  "storage": {...},
  "equipment": [...],
  "ingredients": [...],
  "instructions": [...],
  "troubleshooting": [...],
  "chef_notes": [...],
  "cultural_context": "...",
  "nutrition": {...}
}
```

---

## Field Reference

### Basic Metadata

| Field | Type | Description |
|-------|------|-------------|
| `id` | string (UUID) | Unique recipe identifier |
| `name` | string | Recipe title |
| `description` | string | Brief description of the dish |
| `category` | string | Primary category (Breakfast, Lunch, Dinner, etc.) |
| `cuisine` | string | Cuisine type (American, Italian, Mexican, etc.) |
| `difficulty` | string | Skill level: `Beginner`, `Intermediate`, or `Advanced` |
| `tags` | string[] | Searchable tags and keywords |

---

### Meta

Timing and yield information.

```json
{
  "meta": {
    "active_time": "PT20M",
    "passive_time": "PT1H40M",
    "total_time": "PT2H",
    "overnight_required": false,
    "yields": "4 servings",
    "yield_count": 4,
    "serving_size_g": 300
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `active_time` | string (ISO 8601) | Hands-on preparation time |
| `passive_time` | string (ISO 8601) | Inactive time (baking, marinating, resting) |
| `total_time` | string (ISO 8601) | Total time from start to finish |
| `overnight_required` | boolean | Whether recipe requires overnight prep |
| `yields` | string | Human-readable yield description |
| `yield_count` | number | Numeric serving/portion count |
| `serving_size_g` | number | Serving size in grams |

**ISO 8601 Duration Examples:**
- `PT20M` = 20 minutes
- `PT1H` = 1 hour
- `PT1H40M` = 1 hour 40 minutes
- `PT2H` = 2 hours

---

### Dietary

Dietary flags and restrictions.

```json
{
  "dietary": {
    "flags": [
      "Gluten-Free",
      "Dairy-Free",
      "Egg-Free",
      "Nut-Free",
      "Peanut-Free",
      "Soy-Free"
    ],
    "not_suitable_for": []
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `flags` | string[] | Dietary categories the recipe satisfies |
| `not_suitable_for` | string[] | Dietary restrictions this recipe violates |

---

### Storage

Storage and reheating instructions.

```json
{
  "storage": {
    "refrigerator": {
      "notes": "Flavor improves after 24 hours.",
      "duration": "P4D"
    },
    "freezer": {
      "notes": "Thaw overnight in refrigerator before reheating.",
      "duration": "P3M"
    },
    "reheating": "Heat in a saucepan over medium-low heat, adding a splash of water if too thick.",
    "does_not_keep": false
  }
}
```

| Field | Type | Description |
|-------|------|-------------|
| `refrigerator.notes` | string | Refrigerator storage tips |
| `refrigerator.duration` | string (ISO 8601) | How long it keeps refrigerated |
| `freezer.notes` | string | Freezer storage tips |
| `freezer.duration` | string (ISO 8601) | How long it keeps frozen |
| `reheating` | string | Reheating instructions |
| `does_not_keep` | boolean | Whether recipe should be consumed immediately |

**ISO 8601 Duration for Days/Months:**
- `P4D` = 4 days
- `P3M` = 3 months

---

### Equipment

Required cooking equipment with alternatives.

```json
{
  "equipment": [
    {
      "name": "Blender",
      "required": true,
      "alternative": "Food processor or mortar and pestle"
    },
    {
      "name": "Heavy skillet",
      "required": true,
      "alternative": "Dutch oven or heavy-bottomed pot"
    },
    {
      "name": "Mixing bowl",
      "required": true,
      "alternative": null
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Equipment name |
| `required` | boolean | Whether this equipment is essential |
| `alternative` | string \| null | Substitute equipment that can be used |

---

### Ingredients

Ingredients organized into logical groups.

```json
{
  "ingredients": [
    {
      "group_name": "Chili Base",
      "items": [
        {
          "name": "red chilies",
          "quantity": 6,
          "unit": null,
          "preparation": "dried",
          "notes": "about 30g",
          "substitutions": [],
          "ingredient_id": "3c3f97d4-c951-43fd-865c-88fa8b445739",
          "nutrition_source": "USDA FoodData Central"
        },
        {
          "name": "stewing beef",
          "quantity": 910,
          "unit": "g",
          "preparation": "cut into 1.3cm (1/2 inch) cubes",
          "notes": null,
          "substitutions": [],
          "ingredient_id": "09f2eef4-739a-4fca-8b63-97d697257990",
          "nutrition_source": "USDA FoodData Central"
        },
        {
          "name": "salt",
          "quantity": null,
          "unit": null,
          "preparation": null,
          "notes": "to taste",
          "substitutions": [],
          "ingredient_id": "2631739c-2de4-4389-a6e3-ed3d06d0406f",
          "nutrition_source": "USDA FoodData Central"
        }
      ]
    },
    {
      "group_name": "Flavor Paste",
      "items": [...]
    }
  ]
}
```

#### Ingredient Group

| Field | Type | Description |
|-------|------|-------------|
| `group_name` | string | Group name (e.g., "Chili Base", "Flavor Paste", "Topping") |
| `items` | IngredientItem[] | List of ingredients in this group |

#### Ingredient Item

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Ingredient name |
| `quantity` | number \| null | Yes | Amount needed (null for "to taste") |
| `unit` | string \| null | Yes | Unit of measurement (null for count items like "2 cloves") |
| `preparation` | string \| null | No | Prep instructions (dried, diced, minced, etc.) |
| `notes` | string \| null | No | Additional notes ("about 30g", "to taste") |
| `substitutions` | Substitution[] | No | Alternative ingredients |
| `ingredient_id` | string (UUID) | Yes | Unique identifier for this ingredient |
| `nutrition_source` | string | Yes | Source of nutrition data |

---

### Instructions

Step-by-step cooking instructions with structured metadata.

```json
{
  "instructions": [
    {
      "step_number": 1,
      "phase": "prep",
      "text": "Tear the dried chilies into strips and place them in a bowl. Cover with 240ml (1 cup) of boiling water and soak for 30 minutes.",
      "structured": {
        "action": "SOAK",
        "temperature": null,
        "duration": "PT30M",
        "doneness_cues": null
      },
      "tips": []
    },
    {
      "step_number": 3,
      "phase": "cook",
      "text": "Heat olive oil in a heavy skillet over medium-high heat. Brown the beef cubes on all sides until a crust forms.",
      "structured": {
        "action": "SEAR",
        "temperature": null,
        "duration": null,
        "doneness_cues": {
          "visual": "Beef is deeply browned on all sides",
          "tactile": null
        }
      },
      "tips": []
    },
    {
      "step_number": 5,
      "phase": "cook",
      "text": "Reduce the heat to low, cover the skillet, and simmer for 1 hour.",
      "structured": {
        "action": "SIMMER",
        "temperature": {
          "celsius": 90,
          "fahrenheit": 194
        },
        "duration": "PT1H",
        "doneness_cues": null
      },
      "tips": []
    },
    {
      "step_number": 8,
      "phase": "finish",
      "text": "Remove and discard the bay leaves. Taste and add more salt or pepper if needed before serving.",
      "structured": {
        "action": "SERVE",
        "temperature": null,
        "duration": null,
        "doneness_cues": null
      },
      "tips": []
    }
  ]
}
```

#### Instruction Step

| Field | Type | Description |
|-------|------|-------------|
| `step_number` | number | Step number (1-indexed) |
| `phase` | string | Cooking phase: `prep`, `cook`, or `finish` |
| `text` | string | Human-readable instruction |
| `structured` | StructuredStep | Machine-readable step data |
| `tips` | string[] | Tips specific to this step |

#### Structured Step

| Field | Type | Description |
|-------|------|-------------|
| `action` | string | Action keyword: `SOAK`, `DRAIN`, `SEAR`, `BOIL`, `SIMMER`, `PUREE`, `SERVE`, etc. |
| `temperature` | Temperature \| null | Cooking temperature if applicable |
| `duration` | string \| null | Step duration (ISO 8601) |
| `doneness_cues` | DonenessCues \| null | Visual/physical indicators of completion |

#### Temperature

| Field | Type | Description |
|-------|------|-------------|
| `celsius` | number | Temperature in Celsius |
| `fahrenheit` | number | Temperature in Fahrenheit |

#### Doneness Cues

| Field | Type | Description |
|-------|------|-------------|
| `visual` | string \| null | What to look for visually |
| `tactile` | string \| null | What to feel for |

---

### Troubleshooting

Common issues and solutions.

```json
{
  "troubleshooting": [
    {
      "symptom": "Beef is tough or chewy",
      "likely_cause": "The meat has not simmered long enough to break down connective tissue.",
      "prevention": "Ensure the liquid is at a very low simmer and the lid is tightly sealed.",
      "fix": "Continue simmering in 15-minute increments until tender."
    },
    {
      "symptom": "Chili is too watery",
      "likely_cause": "Too much water added during blending or insufficient reduction.",
      "prevention": "Add water to the blender only 15ml at a time.",
      "fix": "Simmer uncovered for the final 15 minutes to evaporate excess moisture."
    }
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `symptom` | string | What went wrong |
| `likely_cause` | string | Why it happened |
| `prevention` | string | How to avoid it next time |
| `fix` | string | How to salvage the dish |

---

### Chef Notes

Array of professional tips and variations.

```json
{
  "chef_notes": [
    "For the deepest chili flavor, use a variety of dried chilies like ancho, guajillo, and pasilla. Toasting them briefly before soaking can enhance their aroma.",
    "Don't overcrowd the skillet when browning the beef; sear in batches if necessary to ensure a good crust, which adds significant flavor to the final dish.",
    "The simmering time is crucial for tenderizing the beef. Low and slow is key; the meat should be easily pierced with a fork.",
    "The consistency of the chili can be adjusted by the amount of soaking liquid you use and the final simmering time. If it's too thick, add a splash of water or beef broth; if too thin, simmer uncovered for a bit longer."
  ]
}
```

| Field | Type | Description |
|-------|------|-------------|
| `chef_notes` | string[] | Array of tips from the recipe developer |

---

### Cultural Context

Background information about the dish.

```json
{
  "cultural_context": "Texas Chili, or Chili con Carne, is a hearty stew deeply rooted in Texan culinary tradition. Its origins trace back to the mid-19th century, with early versions featuring simple ingredients like dried beef, chilies, and spices, cooked by cowboys and ranchers. The absence of beans and tomatoes is a defining characteristic, distinguishing it from other regional chili variations and emphasizing its pure, meat-and-chili flavor profile."
}
```

| Field | Type | Description |
|-------|------|-------------|
| `cultural_context` | string | Historical and cultural background of the dish |

---

### Nutrition

Comprehensive nutrition data per serving, sourced from USDA FoodData Central.

```json
{
  "nutrition": {
    "per_serving": {
      "calories": 569.02,
      "protein_g": 44.14,
      "carbohydrates_g": 5.59,
      "fat_g": 41.99,
      "saturated_fat_g": 16.48,
      "trans_fat_g": 2.39,
      "monounsaturated_fat_g": 20.66,
      "polyunsaturated_fat_g": 2.29,
      "fiber_g": 1.97,
      "sugar_g": 1.84,
      "sodium_mg": 350.75,
      "cholesterol_mg": 154.7,
      "potassium_mg": 903.3,
      "calcium_mg": 74.24,
      "iron_mg": 7.16,
      "magnesium_mg": 60.57,
      "phosphorus_mg": 434.12,
      "zinc_mg": 16.82,
      "vitamin_a_mcg": 101.01,
      "vitamin_c_mg": 11.51,
      "vitamin_d_mcg": 0.23,
      "vitamin_e_mg": 2.15,
      "vitamin_k_mcg": 14.73,
      "vitamin_b6_mg": 0.97,
      "vitamin_b12_mcg": 6.14,
      "thiamin_mg": 0.19,
      "riboflavin_mg": 0.38,
      "niacin_mg": 10.47,
      "folate_mcg": 12.53,
      "water_g": 154.87,
      "alcohol_g": null,
      "caffeine_mg": null
    },
    "sources": [
      "USDA FoodData Central"
    ]
  }
}
```

#### Nutrition Overview

| Field | Type | Description |
|-------|------|-------------|
| `per_serving` | object | All nutrients calculated per serving |
| `sources` | string[] | Data sources (typically "USDA FoodData Central") |

#### Complete Nutrient List (32 fields)

**Energy:**
| Field | Unit | Description |
|-------|------|-------------|
| `calories` | kcal | Energy per serving |

**Macronutrients:**
| Field | Unit | Description |
|-------|------|-------------|
| `protein_g` | g | Protein |
| `carbohydrates_g` | g | Total carbohydrates |
| `fat_g` | g | Total fat |
| `saturated_fat_g` | g | Saturated fat |
| `trans_fat_g` | g | Trans fat |
| `monounsaturated_fat_g` | g | Monounsaturated fat |
| `polyunsaturated_fat_g` | g | Polyunsaturated fat |
| `fiber_g` | g | Dietary fiber |
| `sugar_g` | g | Total sugars |

**Minerals:**
| Field | Unit | Description |
|-------|------|-------------|
| `sodium_mg` | mg | Sodium |
| `cholesterol_mg` | mg | Cholesterol |
| `potassium_mg` | mg | Potassium |
| `calcium_mg` | mg | Calcium |
| `iron_mg` | mg | Iron |
| `magnesium_mg` | mg | Magnesium |
| `phosphorus_mg` | mg | Phosphorus |
| `zinc_mg` | mg | Zinc |

**Vitamins:**
| Field | Unit | Description |
|-------|------|-------------|
| `vitamin_a_mcg` | mcg | Vitamin A |
| `vitamin_c_mg` | mg | Vitamin C |
| `vitamin_d_mcg` | mcg | Vitamin D |
| `vitamin_e_mg` | mg | Vitamin E |
| `vitamin_k_mcg` | mcg | Vitamin K |
| `vitamin_b6_mg` | mg | Vitamin B6 |
| `vitamin_b12_mcg` | mcg | Vitamin B12 |
| `thiamin_mg` | mg | Thiamin (B1) |
| `riboflavin_mg` | mg | Riboflavin (B2) |
| `niacin_mg` | mg | Niacin (B3) |
| `folate_mcg` | mcg | Folate |

**Other:**
| Field | Unit | Description |
|-------|------|-------------|
| `water_g` | g | Water content |
| `alcohol_g` | g \| null | Alcohol (null if none) |
| `caffeine_mg` | mg \| null | Caffeine (null if none) |

---

## Recipe Summary (List/Search)

When browsing or searching via `GET /api/v1/recipes`, you receive a condensed version with pagination metadata.

```json
{
  "data": [
    {
      "id": "a066f472-ed0c-46ea-8e2c-a0053c3183a8",
      "name": "Texas Chili con Carne",
      "description": "A thick, beef-based stew...",
      "category": "Dinner",
      "cuisine": "American",
      "difficulty": "Intermediate",
      "total_time": "PT2H",
      "calories": 569,
      "tags": ["Beef", "Slow-Cooked", "High-Protein", "Southwestern"]
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

## TypeScript Types

Complete type definitions for TypeScript projects:

```typescript
interface Recipe {
  id: string;
  name: string;
  description: string;
  category: string;
  cuisine: string;
  difficulty: 'Beginner' | 'Intermediate' | 'Advanced';
  tags: string[];
  meta: RecipeMeta;
  dietary: DietaryInfo;
  storage: StorageInfo;
  equipment: Equipment[];
  ingredients: IngredientGroup[];
  instructions: InstructionStep[];
  troubleshooting: TroubleshootingItem[];
  chef_notes: string[];
  cultural_context: string;
  nutrition: NutritionInfo;
}

interface RecipeMeta {
  active_time: string;      // ISO 8601 duration
  passive_time: string;     // ISO 8601 duration
  total_time: string;       // ISO 8601 duration
  overnight_required: boolean;
  yields: string;
  yield_count: number;
  serving_size_g: number;
}

interface DietaryInfo {
  flags: string[];
  not_suitable_for: string[];
}

interface StorageInfo {
  refrigerator: {
    notes: string;
    duration: string;       // ISO 8601 duration
  };
  freezer: {
    notes: string;
    duration: string;       // ISO 8601 duration
  };
  reheating: string;
  does_not_keep: boolean;
}

interface Equipment {
  name: string;
  required: boolean;
  alternative: string | null;
}

interface IngredientGroup {
  group_name: string;
  items: IngredientItem[];
}

interface IngredientItem {
  name: string;
  quantity: number | null;
  unit: string | null;
  preparation: string | null;
  notes: string | null;
  substitutions: Substitution[];
  ingredient_id: string;
  nutrition_source: string;
}

interface Substitution {
  name: string;
  notes: string;
}

interface InstructionStep {
  step_number: number;
  phase: 'prep' | 'cook' | 'finish';
  text: string;
  structured: StructuredStep;
  tips: string[];
}

interface StructuredStep {
  action: string;
  temperature: Temperature | null;
  duration: string | null;    // ISO 8601 duration
  doneness_cues: DonenessCues | null;
}

interface Temperature {
  celsius: number;
  fahrenheit: number;
}

interface DonenessCues {
  visual: string | null;
  tactile: string | null;
}

interface TroubleshootingItem {
  symptom: string;
  likely_cause: string;
  prevention: string;
  fix: string;
}

interface NutritionInfo {
  per_serving: NutritionPerServing;
  sources: string[];
}

interface NutritionPerServing {
  calories: number;
  protein_g: number;
  carbohydrates_g: number;
  fat_g: number;
  saturated_fat_g: number;
  trans_fat_g: number;
  monounsaturated_fat_g: number;
  polyunsaturated_fat_g: number;
  fiber_g: number;
  sugar_g: number;
  sodium_mg: number;
  cholesterol_mg: number;
  potassium_mg: number;
  calcium_mg: number;
  iron_mg: number;
  magnesium_mg: number;
  phosphorus_mg: number;
  zinc_mg: number;
  vitamin_a_mcg: number;
  vitamin_c_mg: number;
  vitamin_d_mcg: number;
  vitamin_e_mg: number;
  vitamin_k_mcg: number;
  vitamin_b6_mg: number;
  vitamin_b12_mcg: number;
  thiamin_mg: number;
  riboflavin_mg: number;
  niacin_mg: number;
  folate_mcg: number;
  water_g: number;
  alcohol_g: number | null;
  caffeine_mg: number | null;
}

// List/Search response
interface RecipeListResponse {
  data: RecipeSummary[];
  meta: PaginationMeta;
}

interface RecipeSummary {
  id: string;
  name: string;
  description: string;
  category: string;
  cuisine: string;
  difficulty: string;
  total_time: string;
  calories: number;
  tags: string[];
}

interface PaginationMeta {
  page: number;
  per_page: number;
  total: number;
  total_pages: number;
}

// Ingredient types
interface IngredientCategory {
  name: string;
  count: number;
}

interface IngredientCategoriesResponse {
  data: IngredientCategory[];
}

interface Ingredient {
  id: string;
  name: string;
  category: string;
  source: string;
}

interface IngredientsResponse {
  data: Ingredient[];
  meta: PaginationMeta;
}
```

---

## Ingredients Endpoints

### Ingredient Categories

`GET /api/v1/ingredient-categories`

Returns all ingredient categories with counts.

```json
{
  "data": [
    { "name": "Vegetables", "count": 1283 },
    { "name": "Beef", "count": 995 },
    { "name": "Baked Goods", "count": 617 },
    { "name": "Beverages", "count": 555 },
    { "name": "Prepared Foods", "count": 530 },
    { "name": "Sauces & Condiments", "count": 512 },
    { "name": "Lamb & Game", "count": 504 }
  ]
}
```

### Browse Ingredients

`GET /api/v1/ingredients`

Returns a paginated list of ingredients with optional filtering.

**Query Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `q` | string | - | Search by ingredient name |
| `category` | string | - | Filter by category |
| `page` | number | 1 | Page number |
| `per_page` | number | 50 | Results per page (max: 100) |

**Response:**

```json
{
  "data": [
    {
      "id": "001764f3-4d44-4dbc-801b-0e4f094c756d",
      "name": "Chickpeas, canned, drained",
      "category": "Legumes",
      "source": "USDA"
    },
    {
      "id": "0004d34a-afbe-4934-b94e-195e1602b23d",
      "name": "Spelt, cooked",
      "category": "Grains & Pasta",
      "source": "USDA"
    },
    {
      "id": "0009da79-3cdb-4838-bdab-2e65ee0a9ccd",
      "name": "Nashi pear",
      "category": "Fruits",
      "source": "Aggregated Public Sources"
    }
  ],
  "meta": {
    "total": 10441,
    "page": 1,
    "per_page": 50
  }
}
```

### Filtering Recipes by Ingredients

Use the `ingredients` query parameter on `/api/v1/recipes` to find recipes containing specific ingredients:

```bash
# Find recipes containing chickpeas and garlic
curl -H "X-API-Key: $API_KEY" \
  "https://recipe-api.com/api/v1/recipes?ingredients=001764f3-4d44-4dbc-801b-0e4f094c756d,30109e1e-d8e8-4f76-a6f4-82ac5b071fde"
```

The endpoint returns recipes containing **ALL** specified ingredients (AND logic).

---

## OpenAPI Specification

For the complete API specification including all endpoints and schemas, see:

https://recipe-api.com/openapi.json
