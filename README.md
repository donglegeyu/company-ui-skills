# company-ui-skills

AI Agent Skill for `@donglegeyu/company-ui` — an enterprise component library based on Ant Design 6 with brand color `#F95914`.

## Install

```bash
npx skills add donglegeyu/company-ui-skills
```

## What it does

This Skill ensures AI coding agents (Trae, Cursor, Claude Code, Copilot, etc.) correctly use the `@donglegeyu/company-ui` component library:

- **95 components**: 72 basic components + 14 shared components + 9 business templates
- **Brand tokens**: `colorPrimary: #F95914`, disabled state `#f5f5f5`, borderRadius `6`
- **5 red-line rules**: no antd native components / no hardcoded colors / no `!important` / no `any` / no default exports
- **Business template data contracts**: PageResponse, FieldDefinition, ColumnField, FormField, etc.

## Documentation

- **Online docs**: https://nrpdemo.zrhsh.com
- **LLMs.txt**: https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms.txt
- **design.md**: https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/design.md

## Structure

```
company-ui-skills/
├── skills/
│   └── company-ui/
│       └── SKILL.md    # The Skill file loaded by AI agents
└── README.md
```

## License

MIT
