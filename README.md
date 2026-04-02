# Prismy Agent Skill

Agent skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code/skills), [Cursor](https://cursor.com/docs/context/skills), [GitHub Copilot](https://github.com/features/copilot), and similar AI coding assistants. Helps work with i18n strings in projects that use Prismy for AI-powered localization.

This repo contains 4 independent, composable skills. Install all of them or pick only the ones you need.

## Skills

| Skill | Description |
| ----- | ----------- |
| **i18n-translate** | Manages the Prismy CLI workflow for generating translations after source locale files are modified. |
| **i18n-respect-copywriting-guidelines** | Fetches glossary and wording instructions from Prismy before writing any user-facing copy. |
| **i18n-detect-hardcoded** | Scans changed files for user-facing hardcoded strings that should be extracted to locale files. |
| **i18n-enforcing-source-language-only** | Prevents the AI agent from directly editing target-language locale files. |

## Install

```bash
# Install all skills
npx skills add prismy-io/prismy-agent-skill

# Install specific skills
npx skills add prismy-io/prismy-agent-skill --skill i18n-translate
npx skills add prismy-io/prismy-agent-skill --skill i18n-respect-copywriting-guidelines
npx skills add prismy-io/prismy-agent-skill --skill i18n-detect-hardcoded
npx skills add prismy-io/prismy-agent-skill --skill i18n-enforcing-source-language-only

# Common combination: translation workflow + copywriting + source-only guard
npx skills add prismy-io/prismy-agent-skill \
  --skill i18n-translate \
  --skill i18n-respect-copywriting-guidelines \
  --skill i18n-enforcing-source-language-only
```

## Prerequisites

The [Prismy CLI](https://www.npmjs.com/package/prismy-cli) must be installed and authenticated:

```bash
npm install -g prismy-cli
prismy auth <your-api-key>
```

Get your API key from [Prismy Settings](https://app.prismy.io/settings).

## More info

- [Prismy](https://prismy.io)
- [Documentation](https://docs.prismy.io)
- [CLI Reference](https://docs.prismy.io/tech/cli)

## License

MIT
