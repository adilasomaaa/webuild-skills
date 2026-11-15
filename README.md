# webuild-skills

Agent skills for building themes for **Webuild** public portals. Works with Claude Code, Codex,
Antigravity, Cursor, Gemini CLI and any agent that supports the open `SKILL.md` format.

| Skill | What it does |
| --- | --- |
| [`webuild-theme`](skills/webuild-theme/SKILL.md) | Workflows, data contract and design system (anti-slop, typography, spacing, UI elements, responsive) for themes built from [webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter). |

## Install

Run from the root of your theme repo:

```bash
# Interactive: pick your agents
npx skills add adilasomaaa/webuild-skills

# Or target agents directly
npx skills add adilasomaaa/webuild-skills -a claude-code -a codex -a antigravity -y
```

| Agent | `-a` | Installed to |
| --- | --- | --- |
| Claude Code | `claude-code` | `.claude/skills/webuild-theme/` |
| Codex | `codex` | `.agents/skills/webuild-theme/` |
| Antigravity | `antigravity` | `.agents/skills/webuild-theme/` |
| Cursor | `cursor` | `.agents/skills/webuild-theme/` |
| Gemini CLI | `gemini-cli` | `.agents/skills/webuild-theme/` |

Add `-g` to install for your user instead of the project, `--copy` to copy files instead of
symlinking (useful when committing the skill so your team shares it), and run `npx skills update`
to get new versions. Requires Node.js 22.20+. See [vercel-labs/skills](https://github.com/vercel-labs/skills).

### Claude Code plugin (alternative)

```
/plugin marketplace add adilasomaaa/webuild-skills
/plugin install webuild-theme@webuild-skills
```

## Related

- [webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter): theme template, plus the [theme building guide](https://github.com/adilasomaaa/webuild-theme-starter/tree/main/docs/guide/pembuatan-tema) (Indonesian)
- [webuild/theme-kit](https://packagist.org/packages/webuild/theme-kit): contracts, `theme:check` validator and workbench

## License

MIT
