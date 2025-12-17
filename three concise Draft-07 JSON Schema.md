Here are **three concise Draft-07 JSON Schemas** for the **primary/secondary** versions of:

1. `mixed_tense_quiz`
    
2. `binary_choice` (existential cards)
    
3. `context_switch`
    

Each is self-contained and validation-ready.

---

## 1) `mixed_tense_quiz.schema.json`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://languagehub.ai/schema/mixed_tense_quiz.schema.json",
  "title": "Mixed Tense Quiz",
  "type": "object",
  "required": ["set_id", "type", "meta", "items"],
  "additionalProperties": false,
  "properties": {
    "set_id": { "type": "string" },
    "type": { "type": "string", "const": "mixed_tense_quiz" },
    "meta": {
      "type": "object",
      "required": ["cefr", "locale", "tense_scope", "focus"],
      "additionalProperties": false,
      "properties": {
        "cefr": {
          "type": "array",
          "items": { "type": "string", "enum": ["A1","A2","B1","B2","C1","C2"] },
          "minItems": 1
        },
        "locale": {
          "type": "object",
          "required": ["primary", "secondary"],
          "additionalProperties": false,
          "properties": {
            "primary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" },
            "secondary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" }
          }
        },
        "tense_scope": { "type": "array", "items": { "type": "string" }, "minItems": 1 },
        "focus": {
          "type": "object",
          "required": ["lemmas"],
          "additionalProperties": false,
          "properties": {
            "lemmas": { "type": "array", "items": { "type": "string" }, "minItems": 1 }
          }
        }
      }
    },
    "grading": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "accent_insensitive": { "type": "boolean" },
        "partial_credit": { "type": "boolean" },
        "retry_policy": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "max_attempts": { "type": "integer", "minimum": 1 },
            "show_answer_after": { "type": "integer", "minimum": 1 }
          }
        }
      }
    },
    "items": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": ["id", "prompt_primary", "answer"],
        "additionalProperties": false,
        "properties": {
          "id": { "type": "string" },
          "prompt_primary": { "type": "string" },
          "hint_primary": { "type": "string" },
          "answer": { "type": "string" },
          "alt_answers": { "type": "array", "items": { "type": "string" } },
          "feedback_primary": { "type": "string" },
          "postfix_primary": { "type": "string" },
          "translation_secondary": { "type": "string" }
        }
      }
    }
  }
}
```

---

## 2) `binary_choice.schema.json` (Existential Cards)

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://languagehub.ai/schema/binary_choice.schema.json",
  "title": "Binary Choice (Existential or Fixed Pair)",
  "type": "object",
  "required": ["set_id", "type", "meta", "items"],
  "additionalProperties": false,
  "properties": {
    "set_id": { "type": "string" },
    "type": { "type": "string", "const": "binary_choice" },
    "meta": {
      "type": "object",
      "required": ["cefr", "locale", "focus"],
      "additionalProperties": false,
      "properties": {
        "cefr": {
          "type": "array",
          "items": { "type": "string", "enum": ["A1","A2","B1","B2","C1","C2"] },
          "minItems": 1
        },
        "locale": {
          "type": "object",
          "required": ["primary", "secondary"],
          "additionalProperties": false,
          "properties": {
            "primary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" },
            "secondary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" }
          }
        },
        "focus": {
          "type": "object",
          "required": ["lemma", "mode", "form_labels"],
          "additionalProperties": false,
          "properties": {
            "lemma": { "type": "string" },
            "mode": { "type": "string" },
            "form_labels": {
              "type": "array",
              "items": { "type": "string" },
              "minItems": 2,
              "maxItems": 2
            }
          }
        }
      }
    },
    "items": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": ["id", "prompt_primary", "options_primary", "correct"],
        "additionalProperties": false,
        "properties": {
          "id": { "type": "string" },
          "prompt_primary": { "type": "string" },
          "options_primary": {
            "type": "array",
            "items": { "type": "string" },
            "minItems": 2,
            "maxItems": 2
          },
          "correct": { "type": "string" },
          "feedback_primary": { "type": "string" },
          "translation_secondary": { "type": "string" }
        },
        "allOf": [
          {
            "properties": {
              "correct": { "enum": { "$data": "1/options_primary" } }
            }
          }
        ]
      }
    }
  }
}
```

---

## 3) `context_switch.schema.json`

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://languagehub.ai/schema/context_switch.schema.json",
  "title": "Context Switch (Meaning Contrast)",
  "type": "object",
  "required": ["set_id", "type", "meta", "items"],
  "additionalProperties": false,
  "properties": {
    "set_id": { "type": "string" },
    "type": { "type": "string", "const": "context_switch" },
    "meta": {
      "type": "object",
      "required": ["cefr", "locale", "focus"],
      "additionalProperties": false,
      "properties": {
        "cefr": {
          "type": "array",
          "items": { "type": "string", "enum": ["A1","A2","B1","B2","C1","C2"] },
          "minItems": 1
        },
        "locale": {
          "type": "object",
          "required": ["primary", "secondary"],
          "additionalProperties": false,
          "properties": {
            "primary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" },
            "secondary": { "type": "string", "pattern": "^[a-z]{2}(-[A-Z]{2})?$" }
          }
        },
        "focus": {
          "type": "object",
          "required": ["lemmas"],
          "additionalProperties": false,
          "properties": {
            "lemmas": { "type": "array", "items": { "type": "string" }, "minItems": 1 },
            "tense_scope": { "type": "array", "items": { "type": "string" } }
          }
        }
      }
    },
    "items": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "object",
        "required": ["id", "context_primary", "options_primary", "correct"],
        "additionalProperties": false,
        "properties": {
          "id": { "type": "string" },
          "context_primary": { "type": "string" },
          "options_primary": {
            "type": "array",
            "items": { "type": "string" },
            "minItems": 2,
            "maxItems": 4
          },
          "correct": { "type": "string" },
          "explain_primary": { "type": "string" },
          "translation_secondary": { "type": "string" }
        },
        "allOf": [
          {
            "properties": {
              "correct": { "enum": { "$data": "1/options_primary" } }
            }
          }
        ]
      }
    }
  }
}
```

---

### Notes

- The `$data` reference used to ensure `correct` ∈ `options_primary` is supported by validators like **Ajv** (`ajv-keywords` plugin). If your validator doesn’t support `$data`, remove the `allOf` block and enforce the check in code.
    
- Language codes accept `xx` or `xx-XX` (e.g., `es`, `pt-BR`).
    
- You can expand any schema later (e.g., add `tags`, `audio_url`) while keeping backward compatibility.