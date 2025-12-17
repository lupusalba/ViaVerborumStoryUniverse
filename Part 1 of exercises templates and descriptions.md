**Part 1 of 3**, exercises with **multi-language support** across Spanish, French, Italian, Portuguese, etc.

---

# 🧩 **Template 1 — Form Drill (`type: "form_drill"`)**

### Purpose

Fill-in-the-blank conjugation or morphology exercise for one verb in one tense.  
Adaptable to any language pair.

### Structure

```json
{
  "set_id": "ex-pres-ser-drill-001",
  "type": "form_drill",
  "meta": { ... },
  "constraints": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"form_drill"`.|✅|
|`meta`|object|Defines level, focus, locale.|✅|
|`meta.cefr`|array|CEFR levels (e.g. `["A1","A2"]`).|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }` or other codes.|✅|
|`meta.focus`|object|`{ "lemma": "ser", "tense": "presente", "persons": ["yo","tú"] }`.|✅|
|`constraints`|object|Accent, retry, etc.|optional|
|`items`|array|Exercise entries.|✅|
|`items[].id`|string|Entry ID.|✅|
|`items[].prompt_primary`|string|Sentence with blank in **primary** language.|✅|
|`items[].answer`|string|Correct missing form.|✅|
|`items[].alt_answers`|array|Accepted variants.|optional|
|`items[].feedback_primary`|string|Hint or correction text in **primary** language.|optional|
|`items[].translation_secondary`|string|Optional support translation.|optional|

### Front-End Use

- Render `prompt_primary` as fill-input.
    
- Compare input to `answer` / `alt_answers`.
    
- Display `feedback_primary` and, if present, `translation_secondary` below for bilingual hinting.
    

### Exclude

- Any fields named `prompt_es`, `prompt_en`, etc.
    
- No `tokens` or `context`; this template handles direct blanks only.
    

---

# 🧩 **Template 2 — Build-a-Line (`type: "build_line"`)**

### Purpose

Drag-and-drop or tap-to-order sentence builder for grammar + word order.

### Structure

```json
{
  "set_id": "ex-pres-estar-build-001",
  "type": "build_line",
  "meta": { ... },
  "ui": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"build_line"`.|✅|
|`meta`|object|Level + focus + locale.|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.focus`|object|`{ "lemma": "estar", "tense": "presente" }`.|✅|
|`ui`|object|`{ "mode": "drag_tokens" }`.|optional|
|`items`|array|Sentences to arrange.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].tokens_primary`|array|Array of tokens (words/chunks) in **primary** language.|✅|
|`items[].answer_order`|array|Correct token indices.|✅|
|`items[].feedback_primary`|string|Correct full sentence.|optional|
|`items[].translation_secondary`|string|Optional translation.|optional|

### Front-End Use

- Display `tokens_primary` as draggable cards.
    
- Verify order via `answer_order`.
    
- Show `feedback_primary` and optional `translation_secondary` on check.
    

### Exclude

- No `prompt_primary` or `answer` fields.
    
- Avoid embedding translations directly into `tokens_primary`.
    

---

# 🧩 **Template 3 — Choose the Tense (`type: "choose_tense"`)**

### Purpose

Learner selects which tense or construction fits given time cues.

### Structure

```json
{
  "set_id": "ex-fut-choose-001",
  "type": "choose_tense",
  "meta": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"choose_tense"`.|✅|
|`meta`|object|Locale + scope.|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.tense_scope`|array|e.g. `["presente","futuro_cercano","preterito"]`.|✅|
|`meta.focus`|object|`{ "lemma": "ir" }`.|optional|
|`items`|array|Question set.|✅|
|`items[].id`|string|ID.|✅|
|`items[].stem_primary`|string|Context sentence with blank (primary language).|✅|
|`items[].options_primary`|array|Possible verb phrases/forms.|✅|
|`items[].correct`|string|Correct choice from options.|✅|
|`items[].explain_primary`|string|Explanation in primary language.|optional|
|`items[].translation_secondary`|string|Optional translation/aid.|optional|

### Front-End Use

- Render as multiple choice.
    
- Highlight time cues (e.g., _mañana_).
    
- Show `explain_primary` for grammar reasoning and optional bilingual aid.
    

### Exclude

- No `tokens_primary` or reorder fields.
    
- Keep monolingual for immersion if `translation_secondary` null.