# company-ui-skills

AI Agent Skill for `@donglegeyu/company-ui` — an enterprise component library based on Ant Design 6 with brand color `#F95914`.

## Install

```bash
npx skills add donglegeyu/company-ui-skills
```

## What it does

This Skill ensures AI coding agents (Trae, Cursor, Claude Code, Copilot, etc.) correctly use the `@donglegeyu/company-ui` component library:

- **96 components**: 72 basic components + 14 shared components + 10 business templates

- **Brand tokens**: `colorPrimary: #F95914`, disabled state `#f5f5f5`, borderRadius `6`

- **6 red-line rules**: no antd native components / no hardcoded colors / no `!important` / no `any` / no default exports / business templates first

- **Admin skeleton as ONE component**: `AdminLayoutTemplate` fixes sidebar + tabs + content into one template (tab state, menu sync, spacing contract all managed internally — do NOT manually assemble CompanySidebar + CompanyMultiTabs)

- **Skeleton spacing contract**: zero-spacing content wrapper is built into AdminLayoutTemplate, template-managed padding

- **Business template data contracts**: PageResponse, FieldDefinition, ColumnField, FormField, etc.

## Quick Start for AI Agents

Paste this prompt into your AI agent (Trae, Cursor, Claude Code, Copilot, etc.):

```markdown
本组件库是 @donglegeyu/company-ui，基于 Ant Design 6 深度定制，品牌主色 #F95914。

如果你可以安装 skills，请运行：
npx skills add donglegeyu/company-ui-skills

在编写任何代码之前，请先阅读以下在线文档：
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms.txt`
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/design.md`
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms-full.txt`

关键规则：
1) 始终使用 CompanyXxx 组件（如 CompanyButton，而非 Button），从 @donglegeyu/company-ui 导入
2) 禁止硬编码颜色 — 使用 theme.useToken()
3) 禁止使用 !important
4) 禁止使用 any 类型
5) 禁止默认导出
6) 业务模板组件优先于基础组件（列表页用 SmartListTemplate，不用 CompanyTable 手动拼）
7) 后台页面骨架必须用 AdminLayoutTemplate 一个组件（传 logo、brandName、domains、businessMenus、userInfo、onLogout、pages 约 7 个核心 props），禁止手动拼装 CompanySidebar + CompanyMultiTabs
8) SidebarDomain 字段是 domainName（不是 name），先查 index.d.ts 确认 props 名称
9) AdminLayoutTemplate 内容区零间距已固化在组件内部 — contentStyle 禁止加 padding/margin/background（业务模板自带全部内间距）
10) CompanyLayout 的 Content 本身零间距——占位样式在内部 div 上，AI 照抄也对
```

## Documentation

- **Online docs**: <https://nrpdemo.zrhsh.com>

- **LLMs.txt**: <https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms.txt>

- **design.md**: <https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/design.md>

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
