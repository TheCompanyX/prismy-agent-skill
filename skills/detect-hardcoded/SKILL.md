---
name: detect-hardcoded
description: >
  Scans changed files for user-facing hardcoded strings that should be extracted to i18n locale files.
  Triggers on hardcoded strings, i18n, internationalization, locale files, extract strings, user-facing text,
  translation keys, t(), useTranslation, $t, intl.formatMessage, string extraction, code review.
  Identifies hardcoded text in components and templates, then extracts it to source locale files.
---

# Detecting Hardcoded Strings

Scans changed files for user-facing hardcoded strings that should be extracted to i18n locale files.

## Rules (strict, no exceptions)

1. **ALWAYS** scan modified components, views, and templates for hardcoded user-facing text before committing.
2. **NEVER** extract strings without confirming with the user if more than 3 hardcoded strings are found.

## What to scan for

Labels, messages, placeholders, tooltips, button text, headings, descriptions, error messages shown to users, aria-labels with human-readable text.

## What to ignore

Log messages, error codes, environment variables, CSS class names, test assertions, URLs, technical identifiers, developer-facing comments, constants not displayed to users, import paths, regex patterns.

## Workflow

Before committing changes, follow this checklist:

```
Hardcoded String Detection Progress:
- [ ] Step 1: Identify changed files
- [ ] Step 2: Scan for hardcoded user-facing strings
- [ ] Step 3: Present findings and get confirmation
- [ ] Step 4: Extract strings to source locale files
- [ ] Step 5: Verify extraction
```

### Step 1: Identify changed files

Identify all changed files in the current branch or commit that contain UI components, views, or templates.

### Step 2: Scan for hardcoded strings

Check each changed file for hardcoded user-facing text. Look for string literals in JSX/TSX, template literals in Vue/Svelte templates, and similar patterns.

### Step 3: Present findings and get confirmation

- If more than 3 hardcoded strings are found, present the full list with file and line locations and ask the user which ones to extract.
- If 3 or fewer are found, propose extraction and ask for confirmation.

### Step 4: Extract strings

For each string to extract:

1. Add it to the source locale file with an appropriate key following existing naming conventions.
2. Replace the hardcoded string with the i18n function call used in the codebase. Detect from existing code which pattern is used: `t()`, `useTranslation()`, `$t()`, `intl.formatMessage()`, etc.

### Step 5: Verify extraction

Verify the extraction didn't break any imports or existing functionality.

## Examples

**Correct extraction:**

```jsx
// Before (hardcoded)
<button>Save changes</button>

// After (extracted)
<button>{t("actions.save_changes")}</button>
```

Then add to `en.json`:

```json
{
  "actions.save_changes": "Save changes"
}
```

**What NOT to extract:**

```js
// These stay as-is:
console.log("Debug: form submitted");
const API_URL = "https://api.example.com";
expect(result).toBe("success");
```
