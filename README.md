# Prismy Agent Skill

An agent skill for [Claude Code](https://docs.anthropic.com/en/docs/claude-code/skills), [Cursor](https://cursor.com/docs/context/skills), [GitHub Copilot](https://github.com/features/copilot), and similar AI coding assistants. Helps work with i18n strings in projects that use Prismy for AI-powered localization.

## Why This Skill Exists

1. **Prevent accidental AI translations** — Without this skill, AI assistants often translate strings directly into target languages, bypassing Prismy entirely. This skill ensures the AI only writes source-language strings and defers all translation to Prismy.

2. **Contextual wording consistency** — Before writing any user-facing copy, the AI fetches your project's glossary and wording instructions from Prismy. Every string respects your approved terminology, tone of voice, and product context — not generic defaults.

3. **Integrate into your commit and deployment flow** — At commit time, this skill prompts the user to either:
   - Run `prismy generate` locally to generate translations immediately, or
   - Push the branch and review/generate translations from the Prismy UI

## Install

```bash
npx skills add prismy-io/prismy-agent-skill
```

## Prerequisites

The [Prismy CLI](https://www.npmjs.com/package/prismy-cli) must be installed and authenticated:

```bash
npm install -g prismy-cli
prismy auth <your-api-key>
```

Get your API key from [Prismy Settings](https://app.prismy.io/settings).

## How it works

When you modify localization files, your AI assistant will automatically run `prismy generate` to create translations for all target languages. The CLI compares your branch against main to detect new or changed keys.

## More info

- [Prismy](https://prismy.io)
- [Documentation](https://docs.prismy.io)
- [CLI Reference](https://docs.prismy.io/tech/cli)

## License

MIT
