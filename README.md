# webuild-skills

Agent plugins and skills for **Webuild** public portals. Works with Claude Code, Codex,
Antigravity, Cursor, Gemini CLI and any agent that supports the open `SKILL.md` format.

| Plugin / Skill | What it does |
| --- | --- |
| [`webuild-theme`](skills/webuild-theme/SKILL.md) | Workflows, data contract and design system (anti-slop, typography, spacing, UI elements, responsive) for themes built from [webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter). |
| [`webuild-content`](plugins/webuild-content/README.md) | Bundled plugin containing the `webuild-content` MCP server (`${WEBUILD_MCP_URL}` + `${WEBUILD_MCP_TOKEN}`), `/webuild-content` workflow skill, and coding-vs-CMS guard rule. |

## Install

### Theme skill (`webuild-theme`)

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

### Claude Code plugins (Marketplace)

```
/plugin marketplace add adilasomaaa/webuild-skills

# Theme development skill
/plugin install webuild-theme@webuild-skills

# CMS content management (bundles MCP server + /webuild-content skill + guard rule)
/plugin install webuild-content@webuild-skills
```

### Antigravity (`webuild-content` plugin)

1. Set your MCP endpoint and token in your shell profile (`~/.zshrc` or `~/.bashrc`), then reload and restart Antigravity:

   ```bash
   export WEBUILD_MCP_URL="https://webuild.example.com/mcp/content"
   export WEBUILD_MCP_TOKEN="wbmcp_YOUR_TOKEN_HERE"
   ```

2. Install the plugin via `agy` CLI or copy it into `~/.gemini/config/plugins/`:

   ```bash
   # Using Antigravity CLI (agy)
   git clone --depth 1 https://github.com/adilasomaaa/webuild-skills.git /tmp/webuild-skills
   agy plugin install /tmp/webuild-skills/plugins/webuild-content
   rm -rf /tmp/webuild-skills

   # Or copy manually (without agy CLI)
   git clone --depth 1 https://github.com/adilasomaaa/webuild-skills.git /tmp/webuild-skills
   mkdir -p ~/.gemini/config/plugins
   cp -R /tmp/webuild-skills/plugins/webuild-content ~/.gemini/config/plugins/webuild-content
   rm -rf /tmp/webuild-skills
   ```

3. Verify **WeBuild Content** is enabled in **Customizations > Plugins** (or `agy plugin list`), then run `/webuild-content` in a new chat.

### OpenAI Codex CLI (`webuild-content`)

1. Add the MCP server in `~/.codex/config.toml` and export your token:

   ```toml
   [mcp_servers.webuild-content]
   url = "https://webuild.example.com/mcp/content"
   bearer_token_env_var = "WEBUILD_MCP_TOKEN"
   ```

   ```bash
   export WEBUILD_MCP_TOKEN="wbmcp_YOUR_TOKEN_HERE"
   ```

2. *(Optional, recommended)* Install the `/webuild-content` workflow skill:

   ```bash
   npx skills add adilasomaaa/webuild-skills --skill webuild-content -a codex -g -y
   ```

See [`plugins/webuild-content/README.md`](plugins/webuild-content/README.md) for full details (including Gemini CLI and direct MCP setup without plugins).

## Related

- [webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter): theme template, plus the [theme building guide](https://github.com/adilasomaaa/webuild-theme-starter/tree/main/docs/guide/pembuatan-tema) (Indonesian)
- [webuild/theme-kit](https://packagist.org/packages/webuild/theme-kit): contracts, `theme:check` validator and workbench

## License

MIT
