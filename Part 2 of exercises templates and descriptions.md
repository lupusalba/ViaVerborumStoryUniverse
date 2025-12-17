**Part 2 of 3**, exercises with **multi-language support** across Spanish, French, Italian, Portuguese, etc.

---

# 🧩 Template 4 — Reorder (`type: "reorder"`)

### Purpose

Arrange **scrambled tokens** into a correct sentence in the **primary** language (word order, negation, clitics, etc.).

### Structure

```json
{
  "set_id": "ex-pres-multi-reorder-001",
  "type": "reorder",
  "meta": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique set ID.|✅|
|`type`|string|Must be `"reorder"`.|✅|
|`meta`|object|Level/locale/scope.|✅|
|`meta.cefr`|array|CEFR tags (e.g., `["A1"]`).|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }` (codes only).|✅|
|`meta.tense`|string|Target tense label used in UI/filtering.|✅|
|`meta.focus`|object|`{ "lemmas": ["ser","estar","tener"] }` (or single lemma).|✅|
|`items`|array|Questions.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].scrambled_primary`|array|Scrambled tokens **in primary** language.|✅|
|`items[].answer_primary`|array|Correct token order (same tokens).|✅|
|`items[].feedback_primary`|string|Optional explanation or full solution.|optional|
|`items[].translation_secondary`|string|Optional translation of the final sentence.|optional|

### Front-End Use

- Render `scrambled_primary` as draggable chips; validate against `answer_primary`.
    
- Show `feedback_primary`; optionally reveal `translation_secondary`.
    

### Exclude

- No `prompt_primary`, `answer` strings, or any `*_es`/`*_en` fields.
    

---

# 🧩 Template 5 — Mini-Cloze with Cognates (`type: "mini_cloze"`)

### Purpose

Short multi-line passage in **primary** language with **blanks** for target forms; surrounding text can emphasize **true cognates** and optionally **false-friend warnings**.

### Structure

```json
{
  "set_id": "ex-preterito-hacer-cloze-001",
  "type": "mini_cloze",
  "meta": { ... },
  "context_primary": [ ... ],
  "blanks": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique set ID.|✅|
|`type`|string|`"mini_cloze"`.|✅|
|`meta`|object|Level/locale/focus/cognates.|✅|
|`meta.cefr`|array|CEFR tags.|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.focus`|object|`{ "lemma": "hacer", "tense": "preterito" }` (or other parts of speech).|✅|
|`meta.cognates`|object|`{ "true": [...], "false_friends_blacklist": [...] }`.|optional|
|`context_primary`|array|2–5 lines of text in **primary** language (use “___” placeholders or positional blanks).|✅|
|`blanks`|array|Definitions for each blank.|✅|
|`blanks[].id`|string|Blank ID.|✅|
|`blanks[].answer`|string|Correct form in **primary**.|✅|
|`blanks[].accept`|array|Alt acceptable forms (orthography/variants).|optional|
|`translation_secondary`|string|Optional full translation of the passage.|optional|

### Front-End Use

- Render the passage with input fields where blanks occur.
    
- Optionally highlight words found in `meta.cognates.true`; flag tokens overlapping `false_friends_blacklist`.
    
- After submit, show solutions and brief rule hints.
    

### Exclude

- No `options_primary` or token reordering.
    
- Keep translations outside the lines (use `translation_secondary` if needed).
    

---

# 🧩 Template 6 — Match Meaning (`type: "match_meaning"`)

### Purpose

Match **primary**-language sentences to **secondary**-language meanings; checks comprehension while reinforcing target forms.

### Structure

```json
{
  "set_id": "ex-pres-ten-meaning-001",
  "type": "match_meaning",
  "meta": { ... },
  "items": [ ... ]
}
```

### Field Reference

|Field|Type|Description|Required|
|---|---|---|---|
|`set_id`|string|Unique ID.|✅|
|`type`|string|`"match_meaning"`.|✅|
|`meta`|object|Level/locale/focus.|✅|
|`meta.cefr`|array|CEFR tags.|✅|
|`meta.locale`|object|`{ "primary": "es", "secondary": "en" }`.|✅|
|`meta.focus`|object|`{ "lemma": "tener", "tense": "presente" }` (or broader).|✅|
|`items`|array|Match items.|✅|
|`items[].id`|string|Item ID.|✅|
|`items[].primary_sentence`|string|Sentence in **primary** language.|✅|
|`items[].secondary_options`|array|2–4 candidate meanings in **secondary** language.|✅|
|`items[].correct_index`|integer|Index into `secondary_options` (0-based).|✅|
|`items[].hint_primary`|string|Short hint/explanation in **primary** language.|optional|

### Front-End Use

- Multiple-choice or connect-pairs UI; score by `correct_index`.
    
- Keep options short, literal, and level-appropriate.
    
- Show `hint_primary` post-answer (optional).
    

### Exclude

- No hard-coded `en`/`es` keys.
    
- Don’t include `answer` strings (selection is by index).