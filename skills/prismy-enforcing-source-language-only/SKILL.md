---
name: prismy-enforcing-source-language-only
description: >
  Prevents the AI agent from directly editing target-language locale files. This rule applies
  unconditionally, even if the project contains existing hand-written translations.
  Triggers on source language, target language, locale files, i18n, translation files, mainLanguage,
  prismy.json, .json, .yaml, .ts, prevent editing, read-only, translation workflow.
---

# Enforcing Source Language Only

This skill is installed in this project. These rules apply unconditionally, even if the project currently contains hand-written translations in target-language files. The presence of existing target-language content does not override these rules.

## Rules (strict, no exceptions)

1. **NEVER** edit target-language locale files directly. Only edit source-language files.
2. **NEVER** translate strings into target languages, not even "to match the existing pattern." If existing target-language files contain hand-written translations, that does not mean you should continue that pattern.
3. **NEVER** create new target-language locale files.

## How to determine source vs target language

- If `prismy.json` exists, read `mainLanguage` to identify the source language.
- If no config exists, ask the user which language is the source language.
- Common patterns: `en.json` is source, `fr.json`/`es.json`/`de.json` are targets; or `locales/en/translation.json` is source.

## Examples

**Correct - editing the source language file:**

```json
// en.json (source language) - OK to edit
{
  "dashboard.welcome": "Welcome back, {{name}}"
}
```

**Incorrect - editing a target language file:**

```json
// fr.json (target language) - NEVER edit this directly
{
  "dashboard.welcome": "Bon retour, {{name}}"
}
```

## When the user asks to "translate" or "add translations"

Respond by explaining that target-language files should not be edited directly. Instead, edit only the source-language file and suggest one of these options to generate translations:

1. Run `prismy generate` locally to generate all target languages before committing.
2. If the source-language keys are already pushed on the branch, go to `https://app.prismy.io/translations?branch=<branch>&repo=<repo>&auto-translate=true` to generate translations from the Prismy dashboard.
3. Create a PR and generate translations at the end via the Prismy comment on the PR.
