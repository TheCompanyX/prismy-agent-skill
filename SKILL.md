---
name: prismy-agent-skill
description: >
  Manages the Prismy localization workflow when adding, editing, or reviewing user-facing strings or locale files (.json, .yaml, .ts). 
  Triggers on i18n tasks, translation key management, locale file edits, or any mention of Prismy.
  Fetches project glossary and wording instructions before writing
  copy.
  Prevents direct AI translation and defers all target-language generation to the Prismy CLI.
  Detects hardcoded user-facing strings that
  should be extracted to locale files.
  Integrates with the commit and pull request flow.
---

# Prismy Localization Skill

Helps developers manage translations in projects that use [Prismy](https://prismy.io) for AI-powered localization. Only write source-language strings. Prismy handles all target languages automatically.

## Rules (strict, no exceptions)

1. **NEVER** translate strings into target languages. Only write source-language content.
2. **NEVER** edit target-language locale files directly.
3. **ALWAYS** fetch glossary and AI instructions before writing any user-facing copy.
4. **ALWAYS** use exact glossary terms. Do not substitute synonyms or alternatives.

## Guidelines (use judgment)

- Prefer running `prismy generate` locally, but defer to user preference.
- Key naming should follow conventions found in existing locale files.
- One glossary fetch per locale per session is sufficient.
- When in doubt about tone or wording, re-read the AI instructions.

## Workflow

When adding or modifying user-facing strings, copy and follow this checklist:

```
Localization Progress:
- [ ] Step 1: Check prerequisites (prismy-cli installed and authenticated)
- [ ] Step 2: Read prismy.json for source language and file paths
- [ ] Step 3: Fetch glossary and AI instructions
- [ ] Step 4: Add or modify keys in source locale files only
- [ ] Step 5: Scan changed files for hardcoded strings (see Hardcoded String Detection)
- [ ] Step 6: Run `prismy generate` or defer to PR-based generation
- [ ] Step 7: Validate CLI output
- [ ] Step 8: Commit all updated files together
- [ ] Step 9: Share Prismy review link with the team
```

### Step 1: Check prerequisites

```bash
prismy --version
```

If not installed: `npm install -g prismy-cli`
If not authenticated: ask the user for their API key, then run `prismy auth <key>`.
For auth details, see [cli-reference.md](cli-reference.md).

### Step 2: Read configuration

If `prismy.json` exists at the project root, read it to understand:

- `mainLanguage`: the source language (only edit these files)
- `mainBranch`: the branch to compare against
- `filesToSync`: where locale files live and their format

### Step 3: Fetch wording guidelines

Before editing any locale file or writing user-facing strings, fetch the project's approved terminology and tone of voice.

**Fetch glossary terms:**

```bash
prismy glossary --language <source-language>
```

The glossary contains approved terms that must be used exactly as listed.

**Fetch AI instructions:**

```bash
prismy ai-instructions
```

Returns product context (what the product does, who it is for) and wording rules (tone, style, phrasing). Follow these strictly.

### Step 4: Write source-language strings only

Add or modify keys in the source locale files. Never create or edit target-language files.

### Step 5: Scan for hardcoded strings

Before committing, review all changed files for user-facing strings that should be extracted to locale files. See **Hardcoded String Detection** below.

### Step 6: Generate translations

Determine the user's preference:

**Generate translations locally?**
Run `prismy generate` before committing. All target-language files are updated immediately.

**Generate translations via PR?**
Commit and push as-is. Prismy will comment on the PR with a link to generate translations.

For full CLI options (e.g. `--base-branch`, `--repo-name`), see [cli-reference.md](cli-reference.md).

### Step 7: Validate CLI output

After running `prismy generate`:

1. Check CLI output for errors or warnings.
2. If it reports missing keys or authentication failures, address them before committing.
3. Verify that only target-language files were modified. Source files should remain unchanged by the CLI.
4. If the CLI modified source files unexpectedly, revert those changes.

### Step 8: Commit

Commit source locale files and generated target-language files together in a single commit.

### Step 9: Share review link

After pushing, share the Prismy link so the product or business team can review and adjust wording:

```
https://app.prismy.io/translations?branch=<branch>&repo=<repo>
```

## Hardcoded String Detection

Before committing, review all changed files for user-facing strings that should be extracted to locale files.

**Scan:** Check modified components, views, and templates for hardcoded text visible to users (labels, messages, placeholders, tooltips, button text, headings, descriptions, error messages shown to users).

**Ignore:** Log messages, error codes, environment variables, CSS class names, test assertions, URLs, technical identifiers, developer-facing comments, and constants not displayed to users.

**When found:**

1. List the hardcoded strings and their locations.
2. If more than 3 are found, present the list and ask the user which ones to extract before proceeding.
3. For each string to extract:
   - Add it to the source locale file with an appropriate key
   - Replace the hardcoded string with the i18n lookup call used in the codebase (e.g. `t()`, `useTranslation()`, `$t()`, `intl.formatMessage()`)
   - Follow existing patterns in the codebase for key naming
4. Re-run the glossary check to ensure new strings respect approved terminology.

## Examples

**Correct: adding a new key in the source locale file**

```json
// en.json (source language)
{
  "dashboard.welcome": "Welcome back, {{name}}"
}
```

Do NOT create or edit `fr.json`, `es.json`, etc. Prismy handles those.

**Incorrect: translating directly into a target language**

```json
// fr.json - NEVER do this
{
  "dashboard.welcome": "Bon retour, {{name}}"
}
```

**Correct: extracting a hardcoded string**

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

## CLI Reference

For full CLI usage, authentication setup, and command options, see [cli-reference.md](cli-reference.md).

## Troubleshooting

Auth issues? Direct the user to https://docs.prismy.io/tech/cli
