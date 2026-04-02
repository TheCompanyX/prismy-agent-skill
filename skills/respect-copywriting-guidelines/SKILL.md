---
name: respect-copywriting-guidelines
description: >
  Fetches and applies a project's glossary and wording instructions from Prismy before writing
  any user-facing copy. Triggers on glossary, wording, tone of voice, copywriting, user-facing strings,
  copy, microcopy, UI text, Prismy, i18n, locale files, AI instructions, product context.
  Ensures all user-facing text uses approved terminology and follows the project's tone and style rules.
---

# Respecting Copywriting Guidelines

Fetches and applies a project's glossary and wording instructions from Prismy before writing any user-facing copy.

## Rules (strict, no exceptions)

1. **ALWAYS** fetch glossary terms and AI instructions before writing or editing any user-facing copy.
2. **ALWAYS** use exact glossary terms. Never substitute synonyms or alternatives.
3. **ALWAYS** follow the wording rules (tone, style, phrasing) returned by `prismy ai-instructions`.

## Guidelines (use judgment)

- One glossary fetch per locale per session is sufficient.
- When in doubt about tone or phrasing, re-read the AI instructions.
- This applies to ALL user-facing text: locale file values, hardcoded strings in components, UI labels, error messages shown to users, placeholder text, tooltips.

## Workflow

Before writing or editing user-facing copy, follow this checklist:

```
Copywriting Guidelines Progress:
- [ ] Step 1: Check prerequisites
- [ ] Step 2: Fetch glossary
- [ ] Step 3: Fetch AI instructions
- [ ] Step 4: Write or edit copy using approved terms and tone
- [ ] Step 5: Review copy against glossary before finalizing
```

### Step 1: Check prerequisites

```bash
prismy --version
```

If not installed: `npm install -g prismy-cli`
If not authenticated: ask the user for their API key, then run `prismy auth <key>`.

### Step 2: Fetch glossary

```bash
prismy glossary --language <source-language>
```

The glossary contains approved terms that must be used exactly as listed.

### Step 3: Fetch AI instructions

```bash
prismy ai-instructions
```

Returns product context (what the product does, who it is for) and wording rules (tone, style, phrasing). Follow these strictly.

### Step 4: Write or edit copy

Use the approved glossary terms and follow the tone and style from the AI instructions for all user-facing text.

### Step 5: Review against glossary

Before finalizing, verify that every user-facing string uses the exact terms from the glossary.

## Examples

**Correct:** glossary says "workspace" -> use "workspace" everywhere

**Incorrect:** glossary says "workspace" -> write "project" because it feels more natural
