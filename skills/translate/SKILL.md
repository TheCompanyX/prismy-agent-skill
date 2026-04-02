---
name: translate
description: >
  Manages the Prismy CLI workflow for generating translations after locale files are modified.
  Triggers on prismy generate, prismy CLI, translation generation, locale files, i18n, commit flow,
  target languages, .json, .yaml, .ts file changes.
  Runs prismy generate after source-language edits and integrates into the commit flow.
---

# Generating Prismy Translations

Manages the Prismy CLI workflow for generating translations after source-language locale files are modified. Only write source-language strings. Prismy handles all target languages automatically.

## Rules (strict, no exceptions)

1. **NEVER** translate strings into target languages manually. Defer all translation to `prismy generate`.
2. **ALWAYS** run `prismy generate` after modifying source locale files.

## Workflow

When source locale files are modified, copy and follow this checklist:

```
Translation Generation Progress:
- [ ] Step 1: Check prerequisites
- [ ] Step 2: Read prismy.json configuration
- [ ] Step 3: Run prismy generate
- [ ] Step 4: Validate CLI output
- [ ] Step 5: Commit all files together
- [ ] Step 6: Share Prismy review link
```

### Step 1: Check prerequisites

```bash
prismy --version
```

If not installed: `npm install -g prismy-cli`
If not authenticated: ask the user for their API key, then run `prismy auth <key>`.

For full CLI options, see [cli-reference.md](cli-reference.md).

### Step 2: Read configuration

If `prismy.json` exists at the project root, read it to understand:

- `mainLanguage`: the source language (only edit these files)
- `mainBranch`: the branch to compare against
- `filesToSync`: where locale files live and their format

### Step 3: Run prismy generate

```bash
prismy generate
```

For options like `--base-branch` or `--repo-name`, see [cli-reference.md](cli-reference.md).

### Step 4: Validate CLI output

After running `prismy generate`:

1. Check CLI output for errors or warnings.
2. If it reports missing keys or authentication failures, address them before committing.
3. Verify that only target-language files were modified. Source files should remain unchanged by the CLI.
4. If the CLI modified source files unexpectedly, revert those changes.

### Step 5: Commit

Commit source locale files and generated target-language files together in a single commit.

### Step 6: Share review link

After pushing, share the Prismy link so the product or business team can review and adjust wording:

```
https://app.prismy.io/translations?branch=<branch>&repo=<repo>
```

## CLI Reference

For full CLI usage, authentication setup, and command options, see [cli-reference.md](cli-reference.md).

## Troubleshooting

Auth issues? Direct the user to https://docs.prismy.io/tech/cli
