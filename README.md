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
本组件库是 @donglegeyu/company-ui（需 >= 1.2.36；内置默认 Logo 自 1.2.39），基于 Ant Design 6 深度定制，品牌主色 #F95914。

如果你可以安装 skills，请运行：
npx skills add donglegeyu/company-ui-skills

在编写任何代码之前，请先阅读：
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms.txt`（规则与数据契约，必读）
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/design.md`（设计语言，按需）
- `https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/llms-full.txt`（全部组件文档，用到哪个组件查哪个章节，不必通读）

关键规则：
1) 始终使用 CompanyXxx 组件（如 CompanyButton，而非 Button），从 @donglegeyu/company-ui 导入。白名单例外（无 Company 封装，直接用 antd）：Space / Tag / Form / Row / Col / Popover / Menu / App / theme / ConfigProvider
2) 禁止硬编码颜色 — 使用 theme.useToken()
3) 禁止使用 !important
4) 禁止使用 any 类型
5) 禁止默认导出 — 用 export function / export const
6) 业务模板优先于基础组件：后台骨架用 AdminLayoutTemplate，列表页用 SmartListTemplate（不用 CompanyTable 手动拼），表单页用 FormPageTemplate，详情页用 DetailPageTemplate
7) 后台骨架必须用 AdminLayoutTemplate 一个组件（logo、brandName、domains、businessMenus、userInfo、onLogout、pages 约 7 个核心 props），禁止手动拼装 CompanySidebar + CompanyMultiTabs
8) 高频数据契约：分页接口 PageResponse 字段是 list（不是 records）；SidebarDomain 字段是 domainName（不是 name）；入口用 ThemeProvider 包裹
9) 不确定组件 props 时，先查已安装包的类型定义（node_modules/@donglegeyu/company-ui/dist/es/index.d.ts），不要凭记忆写；未安装或无法读文件时，查 llms-full.txt 中该组件的章节
10) AdminLayoutTemplate 内容区零间距已固化在组件内部 — contentStyle 禁止加 padding/margin/background（业务模板自带全部内间距）
11) AdminLayoutTemplate 左侧导航必须与官方布局模板 demo 一致：显式传 firstMenus（首页+收藏）、systemBottomMenus（系统设置）、homeTab/homeContent（成对），且每个一级 businessMenus 项必须带合法 icon（SvgIcon 精选图标名，禁止编造，否则渲染空白）。业务菜单超过 6 个时多出的自动进「更多应用」抽屉（visibleMenuCount 默认 6）。照抄 llms.txt / SKILL.md 中的 demo 同款导航脚手架，只改 key/label/path
12) logo 可省略：@1.2.39+ 内置默认 Logo（随包 data URI，零网络）。传项目路径覆盖；传 "" 回退 brandName 首字母。禁止照抄文档 demo 的 /logo-dl.svg（文档站专属资产，真实项目 404）
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
