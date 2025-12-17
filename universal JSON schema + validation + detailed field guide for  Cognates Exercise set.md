Excellent — here’s a **universal JSON schema + detailed field guide** for your _Cognates Exercise_ set.  
This design works for **any language pair**, not just Spanish↔English — later you can use it for **French, Italian, Portuguese**, etc.

---

# 🧩 **Universal Cognates Exercise Template (`type: "cognate_match"`)**

### Purpose

To practice **recognizing cognates and false friends** between a **primary** and **secondary** language.  
Each entry can store complete cognates, stems, endings, and false-friend flags.

---

### 🧠 JSON Structure

```json
{
  "set_id": "ex-cognates-v1",
  "type": "cognate_match",
  "meta": { ... },
  "items": [ ... ]
}
```

---

### 🔹 **`meta` Object**

Defines language pairing and dataset behavior.

|Field|Type|Description|Required|Notes|
|---|---|---|---|---|
|`set_id`|string|Unique identifier for this dataset.|✅|e.g., `ex-cognates-v1`|
|`type`|string|Always `"cognate_match"`.|✅|Determines which UI component renders this exercise.|
|`meta`|object|Container for language and dataset info.|✅||
|`meta.primary_language`|object|Info about main study language.|✅|`{ "code": "es", "name": "Spanish", "role": "primary" }`|
|`meta.secondary_language`|object|Info about paired support language.|✅|`{ "code": "en", "name": "English", "role": "secondary" }`|
|`meta.cefr`|array|CEFR range if applicable.|optional|e.g., `["A1","B1"]`|
|`meta.include_false_friends`|boolean|Whether dataset includes false friends.|✅|UI can highlight them differently.|
|`meta.version`|integer|Schema version.|✅|For future compatibility.|
|`meta.notes`|string|Human-readable description.|optional|For generation context (e.g., “Spanish–English Latin cognates”).|

---

### 🔹 **`items` Array**

Each entry is one **word pair** (true or false cognate).

|Field|Type|Description|Required|Example / Notes|
|---|---|---|---|---|
|`id`|string|Unique item ID.|✅|e.g., `"cog001"`.|
|`is_false_friend`|boolean|Marks if the pair is misleading.|✅|`true` for “embarazada / embarrassed”.|
|`primary_word_full`|string|Full word in primary language.|✅|`"nación"`.|
|`primary_stem`|string|Stem of the primary word.|✅|`"nac"`.|
|`primary_ending`|string|Suffix of the primary word.|✅|`"ión"`.|
|`secondary_word_full`|string|Full word in secondary language.|✅|`"nation"`.|
|`secondary_stem`|string|Stem of the secondary word.|✅|`"nat"`.|
|`secondary_ending`|string|Ending of the secondary word.|✅|`"ion"`.|
|`similarity_score`|number|Optional numeric similarity (0–1).|optional|For future AI grading/analytics.|
|`explanation`|string|Short note on relation or trap meaning.|optional|“Both from Latin _natio_, same meaning.”|
|`example_sentence_primary`|string|Sentence using primary word.|optional|`"La nación es grande."`|
|`example_sentence_secondary`|string|Sentence using secondary word.|optional|`"The nation is great."`|

---

### 🔹 **Front-End / App Behavior**

- Display as **match pairs**, **fill blanks**, or **drag-pair linking**.
    
- Highlight `is_false_friend = true` pairs with warning color or badge.
    
- Optionally auto-generate stem/ending highlighting visually.
    
- `meta.primary_language` and `meta.secondary_language` drive labels dynamically — so the same JSON works for any language pair.
    

---

### 🔹 **What to Exclude**

- ❌ No hard-coded “Spanish” or “English” labels — use `meta.primary_language.name`.
    
- ❌ No tense or person fields (not verb-focused).
    
- ❌ Avoid extra translation fields — each pair defines both words explicitly.
    

---

### ✅ **Sample Minimal Entry**

```json
{
  "set_id": "ex-cognates-v1",
  "type": "cognate_match",
  "meta": {
    "primary_language": { "code": "es", "name": "Spanish", "role": "primary" },
    "secondary_language": { "code": "en", "name": "English", "role": "secondary" },
    "include_false_friends": true,
    "version": 1
  },
  "items": [
    {
      "id": "cog001",
      "is_false_friend": false,
      "primary_word_full": "nación",
      "primary_stem": "nac",
      "primary_ending": "ión",
      "secondary_word_full": "nation",
      "secondary_stem": "nat",
      "secondary_ending": "ion",
      "similarity_score": 0.94,
      "explanation": "True cognate, same meaning in both languages."
    },
    {
      "id": "cog002",
      "is_false_friend": true,
      "primary_word_full": "embarazada",
      "primary_stem": "embaraz",
      "primary_ending": "ada",
      "secondary_word_full": "embarrassed",
      "secondary_stem": "embarrass",
      "secondary_ending": "ed",
      "similarity_score": 0.71,
      "explanation": "False friend — 'embarazada' means 'pregnant', not 'embarrassed'."
    }
  ]
}
```

---

Here’s the **JSON Schema (Draft-07)** specification for **Universal Cognates Exercise** format — ready for validation, code generation, or LLM-guided creation.

You can save this as `cognate_match.schema.json` and validate any JSON file against it.

---

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://languagehub.ai/schema/cognate_match.schema.json",
  "title": "Universal Cognates Exercise Schema",
  "description": "Schema for cognate recognition exercises usable across multiple language pairs (e.g., Spanish-English, French-English, etc.)",
  "type": "object",
  "required": ["set_id", "type", "meta", "items"],
  "properties": {
    "set_id": {
      "type": "string",
      "description": "Unique identifier for this exercise set, e.g. ex-cognates-v1"
    },
    "type": {
      "type": "string",
      "enum": ["cognate_match"],
      "description": "Exercise type identifier. Must be 'cognate_match'."
    },
    "meta": {
      "type": "object",
      "required": ["primary_language", "secondary_language", "include_false_friends", "version"],
      "properties": {
        "primary_language": {
          "type": "object",
          "required": ["code", "name", "role"],
          "properties": {
            "code": {
              "type": "string",
              "pattern": "^[a-z]{2}$",
              "description": "ISO 639-1 language code, e.g. 'es', 'fr', 'pt'"
            },
            "name": {
              "type": "string",
              "description": "Display name of the primary language"
            },
            "role": {
              "type": "string",
              "enum": ["primary"],
              "description": "Fixed as 'primary'"
            }
          },
          "additionalProperties": false
        },
        "secondary_language": {
          "type": "object",
          "required": ["code", "name", "role"],
          "properties": {
            "code": {
              "type": "string",
              "pattern": "^[a-z]{2}$",
              "description": "ISO 639-1 language code for paired language"
            },
            "name": {
              "type": "string",
              "description": "Display name of the secondary language"
            },
            "role": {
              "type": "string",
              "enum": ["secondary"],
              "description": "Fixed as 'secondary'"
            }
          },
          "additionalProperties": false
        },
        "cefr": {
          "type": "array",
          "items": {
            "type": "string",
            "enum": ["A1","A2","B1","B2","C1","C2"]
          },
          "description": "Optional CEFR difficulty range"
        },
        "include_false_friends": {
          "type": "boolean",
          "description": "If true, dataset includes false friends alongside true cognates"
        },
        "version": {
          "type": "integer",
          "minimum": 1,
          "description": "Dataset schema version"
        },
        "notes": {
          "type": "string",
          "description": "Optional free-text description for this dataset"
        }
      },
      "additionalProperties": false
    },
    "items": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": [
          "id",
          "is_false_friend",
          "primary_word_full",
          "primary_stem",
          "primary_ending",
          "secondary_word_full",
          "secondary_stem",
          "secondary_ending"
        ],
        "properties": {
          "id": {
            "type": "string",
            "description": "Unique identifier for this cognate entry"
          },
          "is_false_friend": {
            "type": "boolean",
            "description": "True if the words look similar but differ in meaning"
          },
          "primary_word_full": {
            "type": "string",
            "description": "Complete word in the primary language"
          },
          "primary_stem": {
            "type": "string",
            "description": "Root or base morpheme of the primary word"
          },
          "primary_ending": {
            "type": "string",
            "description": "Suffix or morphological ending of the primary word"
          },
          "secondary_word_full": {
            "type": "string",
            "description": "Complete word in the secondary language"
          },
          "secondary_stem": {
            "type": "string",
            "description": "Root or base morpheme of the secondary word"
          },
          "secondary_ending": {
            "type": "string",
            "description": "Suffix or morphological ending of the secondary word"
          },
          "similarity_score": {
            "type": "number",
            "minimum": 0,
            "maximum": 1,
            "description": "Optional numeric similarity (0–1). Computed by algorithm or LLM."
          },
          "explanation": {
            "type": "string",
            "description": "Optional note explaining true/false cognate relation"
          },
          "example_sentence_primary": {
            "type": "string",
            "description": "Optional example sentence in primary language"
          },
          "example_sentence_secondary": {
            "type": "string",
            "description": "Optional example sentence in secondary language"
          }
        },
        "additionalProperties": false
      }
    }
  },
  "additionalProperties": false
}
```

---

### ✅ **Implementation Notes**

- You can validate JSON with libraries like:
    
    - **Python:** `jsonschema.validate(instance, schema)`
        
    - **Node.js:** `ajv` or `z-schema`
        
- When adding new language pairs (e.g. FR↔EN, PT↔EN), only change the `meta.primary_language` and `meta.secondary_language` codes/names.
    
- To add new fields later (like `word_origin` or `etymology_source`), bump `meta.version` to maintain backward compatibility.