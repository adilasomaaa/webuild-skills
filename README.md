# webuild-skills

Claude Code skills for building themes for **Webuild** public portals.

| Plugin | What it does |
| --- | --- |
| `webuild-theme` | Workflows, data contract and design system (anti-slop, typography, spacing, UI elements, responsive) for themes built from [webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter). |

## Install

```
/plugin marketplace add adilasomaaa/webuild-skills
/plugin install webuild-theme@webuild-skills
```

The same skill also ships inside
[webuild-theme-starter](https://github.com/adilasomaaa/webuild-theme-starter) at
`.claude/skills/webuild-theme/`, so a theme created from the template has it without installing
anything. Install this plugin when you want the skill available outside a theme repo.

Related: [webuild-theme-kit](https://github.com/adilasomaaa/webuild-theme-kit) — contracts,
`theme:check` validator and workbench.

## License

MIT
