**Part 3 of 3**, exercises with **multi-language support** across Spanish, French, Italian, Portuguese, etc.

---

# 🧩 Template 7 — Mixed-Tense Quiz (`type: "mixed_tense_quiz"`)

### Purpose

One review set that mixes **multiple tenses** and **multiple lemmas**. Answers are typed or selected in the **primary** language; optional support text in **secondary**.

### Structure

```json
{
  "set_id": "ex-mixed-quiz-001",
  "type": "mixed_tense_quiz",
  "meta": { ... },
  "grading": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique quiz ID.|✅|
|`type`|string|`"mixed_tense_quiz"`.|✅|
|`meta`|object|Global settings.|✅|
|`meta.cefr`|array|CEFR tags (e.g., `["A1","A2"]`).|✅|
|`meta.locale`|object|Language codes: `{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.tense_scope`|array|Tenses included (labels you use across the app).|✅|
|`meta.focus`|object|Lemmas covered: `{ "lemmas": ["ser","estar","tener", "..."] }`.|✅|
|`grading`|object|UX and scoring policy.|optional|
|`grading.accent_insensitive`|boolean|Ignore diacritics in validation.|optional|
|`grading.partial_credit`|boolean|Allow partial points.|optional|
|`grading.retry_policy`|object|`{ "max_attempts": 2, "show_answer_after": 2 }`.|optional|
|`items`|array|Question list.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].prompt_primary`|string|Sentence with blank in **primary** language.|✅|
|`items[].hint_primary`|string|Minimal hint (lemma/person/tense).|optional|
|`items[].answer`|string|Correct target form (primary).|✅|
|`items[].alt_answers`|array|Acceptable variants.|optional|
|`items[].feedback_primary`|string|Short rule/explanation (primary).|optional|
|`items[].postfix_primary`|string|Text appended after typed form (e.g., after an auxiliary).|optional|
|`items[].translation_secondary`|string|Optional translation/gloss (secondary).|optional|

### Front-End Use

- Render as open input or MCQ; validate vs `answer`/`alt_answers`.
    
- Use `meta.tense_scope` and `meta.focus` for filtering, analytics, and color-coding.
    

### Exclude

- Any `*_es` or `*_en` fields.
    
- Token arrays (that’s for `reorder`/`build_line`).
    

---

# 🧩 Template 8 — Existential Cards (binary choice) (`type: "binary_choice"`)

> Generalizes “hay / hubo” beyond Spanish. Works with any **existential** or **fixed pair** contrast (e.g., present vs past forms) in the **primary** language.

### Structure

```json
{
  "set_id": "ex-existential-cards-001",
  "type": "binary_choice",
  "meta": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"binary_choice"`.|✅|
|`meta`|object|Global settings.|✅|
|`meta.cefr`|array|CEFR tags (e.g., `["A1"]`).|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.focus`|object|`{ "lemma": "existential-verb", "mode": "existential", "form_labels": ["present","past"] }`.|✅|
|`items`|array|Binary choice prompts.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].prompt_primary`|string|Context sentence in **primary** language.|✅|
|`items[].options_primary`|array|Exactly two strings (the contrasted forms in primary).|✅|
|`items[].correct`|string|One of `options_primary`.|✅|
|`items[].feedback_primary`|string|Short rule/explanation (primary).|optional|
|`items[].translation_secondary`|string|Optional translation/gloss (secondary).|optional|

### Front-End Use

- Render two large buttons; evaluate instantly.
    
- If `meta.focus.form_labels` provided, show subtle badges (e.g., “present”, “past”) in UI explanations.
    

### Exclude

- Hard-coded “Hay/Hubo” naming; keep it language-agnostic.
    
- Extra options (must be binary).
    

---

# 🧩 Template 9 — Context Switch (meaning contrast) (`type: "context_switch"`)

> General framework for contrasts like **ability vs intention**, **state vs location**, **completed vs ongoing**, etc. Options are **primary** language strings; rationale is in **primary** or concise icons.

### Structure

```json
{
  "set_id": "ex-context-switch-001",
  "type": "context_switch",
  "meta": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"context_switch"`.|✅|
|`meta`|object|Global settings.|✅|
|`meta.cefr`|array|CEFR tags (e.g., `["A1","A2"]`).|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.focus`|object|Verbs/constructions contrasted, e.g., `{ "lemmas": ["poder","ir"], "tense_scope": ["presente","futuro_cercano"] }`.|✅|
|`items`|array|Contextual decisions.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].context_primary`|string|Full sentence with a slot or implicit choice cue (primary).|✅|
|`items[].options_primary`|array|2–4 primary-language options (short phrases or full forms).|✅|
|`items[].correct`|string|Must match one of `options_primary`.|✅|
|`items[].explain_primary`|string|Reasoning/rule in primary (succinct).|optional|
|`items[].translation_secondary`|string|Optional translation or gloss (secondary).|optional|

### Front-End Use

- Present context, then options as buttons.
    
- After selection, show `explain_primary` (and optionally `translation_secondary`).
    
- Great for semantic disambiguation beyond verbs (can contrast aspect, register, etc.).
    

### Exclude

- Any language-specific keys.
    
- Overly long explanations (keep micro-hints; link to separate lesson cards if needed).
    

---

## Universal Notes (applies to all three templates)

- **Language-agnostic:** Always route UI labels from `meta.locale`.
    
- **Analytics:** Track accuracy by `lemma`, `tense`, and `item.id`.
    
- **I18N:** If you localize UI chrome (e.g., “Check answer”), keep content fields (`*_primary`, `*_secondary`) untouched.