---
name: "company-ui-for-agents"
description: "AI Agent 消费 @donglegeyu/company-ui 组件库的总入口。强制 AI 使用 CompanyXxx 组件（而非 antd 原生）、遵循品牌令牌 #F95914、遵守类型安全和样式规范。在任何使用 company-ui 的项目中生成 React 代码时必须加载。"
---

<!-- company-ui-for-agents skill
     version: 1.2.0
     updated: 2026-10-23
     source: https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/skills/company-ui/SKILL.md
     remote-version: https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/skills/company-ui/VERSION
-->

# Company UI for Agents

> **Skill 版本**：`1.2.0`（快照式安装，不随源仓库自动更新——见下方「〇、版本自检」）
>
> **版本基准**：`@donglegeyu/company-ui@1.2.41`（AdminLayoutTemplate 自 1.2.36 起提供；内置默认 Logo 自 1.2.39 起提供），基于 Ant Design 6.x，品牌主色 `#F95914`。
>
> 本 Skill 是 AI Agent 消费组件库的「驾照」——确保生成的代码合规、可运行、品牌一致。

## 〇、版本自检（每次会话首次使用本 Skill 时执行一次）

本 Skill 采用**快照式安装**（复制到本机 skills 目录后不随源仓库自动更新）。为防止「规范源已升级、本机快照停在旧版」的规范演进漂移，**每次会话首次使用本 Skill 时先做一次版本自检**——一次 curl（≤3 秒超时），失败静默跳过、不阻塞任务：

1. 读取本文件头部注释的 `version:`（即本机安装版本，已随文件加载进上下文，无需重读文件）
2. 拉取远端最新版本：`curl -fsSL --max-time 3 https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/skills/company-ui/VERSION`
3. **远端更新** → 向用户输出一句升级提示（命令见下），然后**按本文件内容继续当前任务**（旧版规则仍远好于没有规则）
4. **版本一致或网络失败** → 静默继续，不输出任何内容

升级命令（整文件覆盖为最新版，新会话生效）：

```bash
curl -fsSL -o ~/.trae-cn/skills/company-ui-for-agents/SKILL.md \
  https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/skills/company-ui/SKILL.md
```

> **给维护者**：任何 SKILL.md 内容变更，必须（1）同步 bump 头部 `version:` 注释与「Skill 版本」引用行，（2）同步更新仓库根的 `VERSION` 文件（两者数值一致），（3）在尾部「版本历史」追加一行，（4）保持本地 `.trae/skills/` 与 GitHub 双源同步。

## 何时调用

- 用户项目已安装 `@donglegeyu/company-ui`，需要生成 React/TypeScript 代码
- 用户提到 CompanyButton / CompanyTable / CompanySelect / SmartListTemplate 等组件名
- 用户要求生成后台管理页面、列表页、表单页、详情页
- 用户要求使用公司组件库、企业 UI 规范
- 任何涉及 antd 组件的代码生成场景（应自动替换为 CompanyXxx）

***

## 一、七条红线（违反即不合格）

### 1. 禁止使用 antd 原生组件

```tsx
// ❌ 错误
import { Button, Input, Select } from 'antd';

// ✅ 正确
import { CompanyButton, CompanyInput, CompanySelect } from '@donglegeyu/company-ui';
```

**白名单**（antd 原生组件库未封装，可直接用）：`Space`、`Tag`、`Form`、`Row`、`Col`、`Popover`、`Menu`、`App`、`theme`（仅用于 `useToken`）、`ConfigProvider`（仅用于局部覆盖）。

### 2. 禁止硬编码颜色

```tsx
// ❌ 错误
<div style={{ color: '#F95914' }}>品牌色文字</div>

// ✅ 正确（运行时消费 token）
import { theme } from 'antd';
const { token } = theme.useToken();
<div style={{ color: token.colorPrimary }}>品牌色文字</div>

// ✅ 也可用 designRules 常量
import { designRules } from '@donglegeyu/company-ui';
<div style={{ color: designRules.colors.brand }}>品牌色文字</div>
```

### 3. 禁止使用 `!important`

```tsx
// ❌ 错误
<div className="my-style" /> // CSS: .my-style { color: red !important; }

// ✅ 正确：用 ConfigProvider 组件级覆盖
<ConfigProvider theme={{ components: { Button: { defaultColor: '#ff4d4f' } } }}>
  <CompanyButton>红色文字按钮</CompanyButton>
</ConfigProvider>

// ✅ 正确：用内联 style（权重天然高于 class）
<CompanyButton style={{ marginRight: 8 }}>间距按钮</CompanyButton>
```

### 4. 禁止使用 `any` 类型

```tsx
// ❌ 错误
function handleData(data: any) { ... }

// ✅ 正确
interface UserData {
  id: number;
  name: string;
  status: 'active' | 'inactive';
}
function handleData(data: UserData) { ... }
```

### 5. 禁止默认导出

```tsx
// ❌ 错误
export default function MyComponent() { ... }

// ✅ 正确
export function MyComponent() { ... }
// 或
export const MyComponent = () => { ... };
```

### 6. 业务模板组件优先于基础组件

AI 在生成页面时，必须优先使用业务模板组件，仅在业务模板无法满足时才退回到共享组件，最后才用基础组件手动拼装。

```tsx
// ❌ 错误：用基础组件手动拼列表页
import { CompanyTable, CompanyPagination, CompanyButton } from '@donglegeyu/company-ui';
<CompanyTable columns={cols} dataSource={data} />
<CompanyPagination total={total} />

// ✅ 正确：用业务模板组件一步到位
import { SmartListTemplate } from '@donglegeyu/company-ui';
<SmartListTemplate title="用户列表" fields={fields} dataSource={dataSource} />
```

| 优先级 | 组件层 | 示例 | 何时使用 |
|---|---|---|---|
| 1（最高） | 业务模板 | `AdminLayoutTemplate` / `SmartListTemplate` / `FormPageTemplate` / `DetailPageTemplate` / `StatisticsListPage` | 后台骨架 / 标准列表页 / 表单页 / 详情页 / 统计页 |
| 2 | 共享组件 | `ActionCell` / `BaseInfoForm` / `PageTitle` / `SectionTitle` | 业务模板不覆盖的局部场景 |
| 3（最低） | 基础组件 | `CompanyTable` / `CompanyForm` / `CompanyButton` | 基础组件无法满足时的手动拼装 |

### 7. 图标一律使用内置图标键（禁止 antd 图标进菜单）

`AdminLayoutTemplate` / `CompanySidebar` 等含图标组件的菜单项**一律用 `icon` 字符串键**（如 `icon: 'setting'`），键名以组件库内置图标集为准（完整清单见「页面骨架规则」的图标清单，也可查文档站 `/icon-library` 精选集）。**禁止**把 `@ant-design/icons` 组件（如 `<SettingOutlined />`）传给 `iconNode`——antd 图标为填充风格、尺寸体系不同，与本库线性图标混用会导致视觉不一致（源码渲染优先级 `iconNode || icon`，传了会**完全遮蔽**库图标系统）。

```tsx
// ❌ 错误：iconNode 塞 antd 图标，遮蔽内置图标集
import { SettingOutlined } from '@ant-design/icons';
{ key: 'system', label: '系统设置', iconNode: <SettingOutlined /> }

// ✅ 正确：icon 字符串键（内置图标集已覆盖常见业务菜单）
{ key: 'system', label: '系统设置', icon: 'setting' }

// ✅ 更简：业务菜单可不写 icon，组件按菜单名关键词自动匹配
{ key: 'finance', label: '财务管理', children: [...] }  // → 自动匹配 wallet
```

**常用业务图标键**：`home` `setting` `commodity` `buy` `order` `file-cabinet` `box` `wallet` `table` `comments` `people-top-card` `tag` `category-management` `all-application` `folder-open`。
`iconNode` 仅在内置图标集**确实没有**所需图标时使用（传了即优先生效、不再使用内置图标集）。传入未注册的 `icon` 键时开发环境会控制台警告并自动回退，不会静默空白。

***

## 二、安装与初始化

### 安装

```bash
npm install @donglegeyu/company-ui
# peer deps（如未安装）
npm install antd@^6.0.0 @ant-design/icons@^5.0.0 react@^19.0.0 react-dom@^19.0.0
```

### 入口包裹

```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';
import { ThemeProvider } from '@donglegeyu/company-ui';
import App from './App';

const root = createRoot(document.getElementById('root')!);
root.render(
  <React.StrictMode>
    <ThemeProvider>
      <App />
    </ThemeProvider>
  </React.StrictMode>
);
```

`ThemeProvider` 内部用 `ConfigProvider theme={companyTheme}` 包裹，所有 CompanyXxx 组件自动继承品牌 Token。

***

## 三、组件全清单

### 基础组件（72 个，按分组）

所有基础组件从 `@donglegeyu/company-ui` 具名导出，命名规则 `Company + 组件名`。

#### 通用 General

| 组件 | 导出名 | 用途 |
|---|---|---|
| 按钮 | `CompanyButton` | 主操作触发器，5 种 variant |
| 悬浮按钮 | `CompanyFloatButton` | 页面级悬浮操作 |
| 图标 | `CompanyIcon` | 语义化 SVG 图标 |
| 排版 | `CompanyTypography` | 标题/段落/文本/链接 |

#### 布局 Layout

| 组件 | 导出名 | 用途 |
|---|---|---|
| 分割线 | `CompanyDivider` | 区域分隔 |
| 弹性布局 | `CompanyFlex` | Flex 布局容器 |
| 栅格行 | `CompanyRow` | 栅格行 |
| 栅格列 | `CompanyCol` | 栅格列 |
| 布局 | `CompanyLayout` | 页面布局（Header/Sider/Content/Footer） |
| 瀑布流 | `CompanyMasonry` | 瀑布流网格 |
| 间距 | `CompanySpace` | 元素间距（垂直/水平） |
| 分隔面板 | `CompanySplitter` | 可调整大小的分割面板 |

#### 导航 Navigation

| 组件 | 导出名 | 用途 |
|---|---|---|
| 锚点 | `CompanyAnchor` | 锚点导航 |
| 面包屑 | `CompanyBreadcrumb` | 面包屑导航 |
| 下拉菜单 | `CompanyDropdown` | 下拉菜单 |
| 导航菜单 | `CompanyMenu` | 导航菜单 |
| 分页 | `CompanyPagination` | 分页导航 |
| 步骤条 | `CompanySteps` | 步骤进度 |
| 标签页 | `CompanyTabs` | Tab 切换 |

#### 数据录入 Data Entry

| 组件 | 导出名 | 用途 |
|---|---|---|
| 自动完成 | `CompanyAutoComplete` | 自动补全输入 |
| 级联选择 | `CompanyCascader` | 级联选择 |
| 多选框 | `CompanyCheckbox` | 多选 |
| 颜色选择器 | `CompanyColorPicker` | 颜色选择 |
| 日期选择框 | `CompanyDatePicker` | 日期选择 |
| 表单 | `CompanyForm` | 表单容器（含校验） |
| 输入框 | `CompanyInput` | 文本输入 |
| 数字输入框 | `CompanyInputNumber` | 数字输入 |
| 提及 | `CompanyMentions` | @ 提及 |
| 单选框 | `CompanyRadio` | 单选 |
| 评分 | `CompanyRate` | 星级评分 |
| 选择器 | `CompanySelect` | 下拉选择 |
| 滑动输入条 | `CompanySlider` | 滑块输入 |
| 开关 | `CompanySwitch` | 开关切换 |
| 时间选择框 | `CompanyTimePicker` | 时间选择 |
| 穿梭框 | `CompanyTransfer` | 穿梭框 |
| 树选择 | `CompanyTreeSelect` | 树形选择 |
| 上传 | `CompanyUpload` | 文件上传 |

#### 数据展示 Data Display

| 组件 | 导出名 | 用途 |
|---|---|---|
| 头像 | `CompanyAvatar` | 用户头像 |
| 徽标数 | `CompanyBadge` | 计数徽标 |
| 日历 | `CompanyCalendar` | 日历 |
| 卡片 | `CompanyCard` | 内容卡片 |
| 走马灯 | `CompanyCarousel` | 轮播 |
| 折叠面板 | `CompanyCollapse` | 折叠面板 |
| 描述列表 | `CompanyDescriptions` | 键值对描述 |
| 空状态 | `CompanyEmpty` | 空状态占位 |
| 图片 | `CompanyImage` | 图片预览 |
| 列表 | `CompanyList` | 列表展示 |
| 气泡卡片 | `CompanyPopover` | 气泡卡片 |
| 二维码 | `CompanyQRCode` | 二维码 |
| 分段控制器 | `CompanySegmented` | 分段控制 |
| 统计数值 | `CompanyStatistic` | 统计数字 |
| 表格 | `CompanyTable` | 数据表格 |
| 标签 | `CompanyTag` | 状态标签 |
| 时间轴 | `CompanyTimeline` | 时间轴 |
| 文字提示 | `CompanyTooltip` | Tooltip |
| 漫游式引导 | `CompanyTour` | 引导教程 |
| 树形控件 | `CompanyTree` | 树形控件 |

#### 反馈 Feedback

| 组件 | 导出名 | 用途 |
|---|---|---|
| 警告提示 | `CompanyAlert` | 警告条 |
| 抽屉 | `CompanyDrawer` | 侧边抽屉 |
| 全局提示 | `CompanyMessage` | 全局 Toast |
| 对话框 | `CompanyModal` | 模态框 |
| 通知提醒框 | `CompanyNotification` | 通知 |
| 气泡确认框 | `CompanyPopconfirm` | 确认气泡 |
| 进度条 | `CompanyProgress` | 进度条 |
| 结果 | `CompanyResult` | 结果页 |
| 骨架屏 | `CompanySkeleton` | 骨架屏 |
| 加载中 | `CompanySpin` | 加载旋转 |
| 水印 | `CompanyWatermark` | 水印 |

#### 其他 Other

| 组件 | 导出名 | 用途 |
|---|---|---|
| 固钉 | `CompanyAffix` | 固定定位 |
| 包裹组件 | `CompanyApp` | App 上下文 |
| 边框流光 | `CompanyBorderBeam` | 动画边框 |
| 全局化配置 | `CompanyConfigProvider` | 全局配置 |

### 共享组件（14 个）

| 组件 | 导出名 | 用途 |
|---|---|---|
| 操作按钮组 | `ActionCell` | 表格操作列，超 2 个自动折叠 |
| 多列表单 | `BaseInfoForm` | 字段驱动的多列表单 |
| 色阶 | `ColorScale` | 颜色渐变选择器 |
| 省略文本 | `EllipsisText` | 文本溢出 + Tooltip |
| 底部操作栏 | `FormFooterActions` | 表单底部固定操作栏 |
| 视图保存抽屉 | `FilterOptionsDrawer` | 筛选视图保存 |
| 页面标题 | `PageTitle` | 标准页头 |
| 分组标题 | `SectionTitle` | 区块分隔标题 |
| 无障碍检查 | `AccessibilityChecker` | 无障碍审计 |
| SVG 图标 | `SvgIcon` | SVG 图标组件 |
| 图标画廊 | `IconGallery` | 图标浏览器 |
| IconPark 画廊 | `IconParkGallery` | IconPark 图标浏览 |
| 图标探索器 | `IconExplorer` | 图标搜索 |
| IconPark Hook | `useIconPark` | IconPark 图标 Hook |

### 业务模板组件（10 个）

| 组件 | 导出名 | 用途 |
|---|---|---|
| 布局模板 | `AdminLayoutTemplate` | ⭐ 后台骨架一步到位（侧边栏+页签+内容区，页签/高亮/间距全托管） |
| 智能列表模板 | `SmartListTemplate` | 筛选+表格+分页+列设置+视图切换 |
| 统计列表页 | `StatisticsListPage` | 统计卡片+高级筛选+表格 |
| 表单页模板 | `FormPageTemplate` | 分组多列表单+底部操作栏 |
| 详情页模板 | `DetailPageTemplate` | 信息卡片+标签+多 Tab |
| 侧边导航 | `CompanySidebar` | 左侧双层导航 |
| 多页签栏 | `CompanyMultiTabs` | 浏览器式页签栏 |
| 筛选表单 | `FilterForm` | 独立筛选表单 |
| 列设置面板 | `ColumnSettingsPanel` | 列可见性/顺序设置 |
| 图标选择器 | `IconSelect` | 图标选择器 |

***

## 四、令牌使用规范

### 品牌关键 Token 速查

| Token | 值 | 用途 |
|---|---|---|
| `colorPrimary` | `#F95914` | 主色（按钮/链接/选中态） |
| `colorPrimaryHover` | `#E04E0E` | 主色悬停 |
| `colorPrimaryBg` | `#FFF1E6` | 选中行浅色背景 |
| `colorSuccess` | `#00B42A` | 成功 |
| `colorWarning` | `#FF7D00` | 警告 |
| `colorError` | `#F53F3F` | 错误 |
| `colorBgContainerDisabled` | `#f5f5f5` | 禁用背景（纯灰） |
| `borderRadius` | `6` | 圆角 |
| `controlHeight` | `32` | 控件高度 |
| `fontSize` | `14` | 字号 |

### 运行时读取 Token

```tsx
import { theme } from 'antd';

function MyComponent() {
  const { token } = theme.useToken();

  return (
    <div style={{
      color: token.colorPrimary,
      padding: token.spacing,        // 8
      borderRadius: token.borderRadius, // 6
      fontSize: token.fontSize,      // 14
    }}>
      品牌色文字
    </div>
  );
}
```

### 静态读取常量

```tsx
import { designRules } from '@donglegeyu/company-ui';

designRules.colors.brand        // '#F95914'
designRules.colors.brandLight   // '#FFF1E6'
designRules.borderRadius.base   // 8
designRules.size.height.base    // 36
designRules.size.spacing.base   // 8
designRules.shadows.small       // '0 2px 8px rgba(0,0,0,0.08)'
```

***

## 五、业务模板数据契约

### PageResponse（标准分页响应）

```ts
interface PageResponse<T> {
  list: T[];       // 注意：是 list，不是 records 或 dataList
  total: number;
  pageNum: number;  // 注意：是 pageNum，不是 current！
  pageSize: number;
}
```

⚠️ **双命名陷阱**：antd 的 `pagination` prop 用 `current`（`pagination={{ current: 1 }}` 是对的），但**数据契约 `PageResponse` 用 `pageNum`**——接口返回值/mock 数据里写 `current` 会导致分页失效。两套命名并存，别混。

### FieldDefinition（SmartListTemplate 字段定义）

```ts
interface FieldDefinition {
  key: string;
  label: string;
  type?: 'input' | 'select' | 'date' | 'daterange' | 'item';
  placeholder?: string;
  options?: { label: string; value: string | number }[];
  width?: number;
  fixed?: 'left' | 'right';
  ellipsis?: boolean;
  hideInFilter?: boolean;
  sortable?: boolean;  // 可选开启（默认 false）——列头排序；业务默认不带，页面确有需要才加
}
```

- `type: 'item'` → 不出现在筛选区，仅作表格列（通常用于操作列，key 必须是 `'action'`）

- `hideInFilter: true` → 字段仍作表格列，但不进筛选区

### 插槽纪律（titleExtra / toolbarActions 等）

业务模板的扩展插槽是**固定的几个**。自定义内容只能放进这些插槽——禁止在模板外再包一层 UI，也**禁止自创用户没要的内容**：

| 插槽 | 语义 | 允许的内容 |
|---|---|---|
| `titleExtra` | 标题的延伸/补充（状态标签、计数） | antd `Tag` / `Space` 包 Tag——**仅当用户需求明确提到计数/状态时才传** |
| `titleRightActions` / `titleActions` | 标题栏右侧独立操作区 | CompanyButton 按钮组 |
| `toolbarActions` / `toolbarRightActions` | 工具栏左侧/右侧操作 | CompanyButton 按钮组 |

三条规则：
1. **禁止自创内容**：像「共 X 条 / 启用 X / 禁用 X」这类统计标签是**自定义 titleExtra，非模板内置**——用户需求里没提计数就完全不传 titleExtra（默认：不传）
2. 工具栏/操作区按钮一律 `CompanyButton` + **文字标签**（「新增」「导出」），与官方 demo 一致；**禁止**图标按钮、禁止在按钮组外加 Divider/自定义间距
3. 行内操作用 `ActionCell`（`{ key, label, onClick, danger?, confirm? }`）配 `bodyCell` 渲染——禁止每行手拼 `<Space>` + CompanyButton。**删除按钮标准：`confirm: true, confirmTitle` 二次确认，不用 `danger`**（全库统一）

### ColumnField（列设置）

```ts
interface ColumnField {
  key: string;
  label: string;
  visible: boolean;
  width?: number;
  fixed?: 'left' | 'right';
}
```

### FormField（BaseInfoForm 表单字段）

```ts
interface FormField {
  name: string;
  label: string;
  type: 'input' | 'select' | 'input-number' | 'date' | 'textarea';
  required?: boolean;
  rules?: Rule[];
  options?: { label: string; value: string | number }[];
}
```

### DetailFieldItem / DetailTagItem / DetailTabItem（DetailPageTemplate）

```ts
interface DetailFieldItem { label: string; value: string; }
interface DetailTagItem { text: string; variant: 'success' | 'warning' | 'error' | 'volcano'; }
interface DetailTabItem { key: string; label: string; children: React.ReactNode; }
```

### StatisticCardConfig / StatisticsFilterField / StatisticsColumn（StatisticsListPage）

```ts
interface StatisticCardConfig { key: string; title: string; value: string | number; }
interface StatisticsFilterField {
  key: string;
  label: string;
  type: 'input' | 'select' | 'date';
  placeholder?: string;
  options?: { label: string; value: string | number }[];
}
interface StatisticsColumn {
  key: string;
  label: string;
  visible?: boolean;
  width?: number;
  render?: (record: Record<string, unknown>) => React.ReactNode;
}
```

### FormPageTemplate 速查（表单页：新建/编辑）

模板提供：页标题 + 返回箭头 + 卡片包裹内容区 + **固定底部操作栏（提交/取消）**。表单体是配置驱动：`SectionTitle` + `BaseInfoForm` + `FormField[]`。**禁止**手拼 CompanyForm 行和底部按钮栏——模板已内置。

| Props | 类型 | 说明 |
|---|---|---|
| title | string | **必传**，页面标题 |
| showBack / onBack | boolean / () => void | 标题旁返回箭头 |
| onSubmit / onCancel | () => void | 绑定到底部固定操作栏 |
| submitText / cancelText | string | 默认「提交 / 取消」 |
| submitLoading | boolean | 提交按钮 loading |
| footerPosition | 'fixed' \| 'static' | 默认 fixed |
| showFooter / footer | boolean / ReactNode | footer 传入则完全替换默认操作栏 |
| topContent | ReactNode | 内容区顶部插槽（Steps、提示条） |
| contentWrapped | boolean | 默认 true（CompanyCard 包裹）；自定义内容设 false |

最小示例：

```tsx
import { useState } from 'react';
import { FormPageTemplate, SectionTitle, BaseInfoForm } from '@donglegeyu/company-ui';
import type { FormField } from '@donglegeyu/company-ui';

const baseFields: FormField[] = [
  { name: 'name', label: '商品名称', type: 'input', required: true },
  { name: 'category', label: '商品分类', type: 'select', options: [
    { label: '电子产品', value: 'electronics' },
    { label: '服装', value: 'clothing' },
  ]},
];

export function ProductCreatePage() {
  const [data, setData] = useState<Record<string, string | number | boolean | null | undefined>>({});
  return (
    <FormPageTemplate title="新增商品" showBack onSubmit={() => { /* save */ }}>
      <SectionTitle title="基础信息" />
      <BaseInfoForm value={data} fields={baseFields} onChange={setData} />
    </FormPageTemplate>
  );
}
```

分组表单：children 里重复「`<SectionTitle title="组名" />` + `<BaseInfoForm ... />`」组合即可。

### DetailPageTemplate 速查（详情页：查看）

模板提供：标题栏（返回 + 头像 + 主体名称 + 标签 + 操作区）+ 信息栅格 + Tab 面板。

| Props | 类型 | 说明 |
|---|---|---|
| title | string | **必传**，页面标题 |
| showBack / onBack | boolean / () => void | 返回箭头 |
| avatar / primaryName | ReactNode | 标题旁的主体信息块 |
| tags | DetailTagItem[] | 主体名旁的状态标签 |
| infoFields | DetailFieldItem[] | 键值对信息栅格 |
| infoColumns | number | 栅格列数，如 3 |
| tabs | DetailTabItem[] | 信息区下方的 Tab 面板 |
| activeTab / onTabChange | string / (key) => void | 受控 Tab |
| titleActions | ReactNode | 解耦操作区（编辑 / 删除按钮） |

示例（覆盖全部核心 props：avatar / tags / tabs / titleActions；15 行表格完整视觉版见 llms-full.txt）：

```tsx
import { Row, Col } from 'antd';
import { DetailPageTemplate, CompanyTable, CompanyTag, CompanyButton, SectionTitle } from '@donglegeyu/company-ui';
import type { DetailFieldItem, DetailTagItem, DetailTabItem } from '@donglegeyu/company-ui';

const infoFields: DetailFieldItem[] = [
  { label: '工单编号', value: 'SC20240615001' },
  { label: '燃气编码', value: 'GAS-888234' },
  { label: '客户名称', value: '张三' },
  { label: '联系电话', value: '138****8888' },
  { label: '所属公司', value: '城南燃气分公司' },
  { label: '详细地址', value: '某某小区 12 栋 3 单元 501' },
];
const tags: DetailTagItem[] = [
  { text: '已安检', variant: 'success' },
  { text: '居民用户', variant: 'default' },
  { text: '已上传', variant: 'volcano' },
];
const tabs: DetailTabItem[] = [
  {
    key: 'detail',
    label: '详情',
    children: (
      <div style={{ padding: '0 0 16px' }}>
        <SectionTitle title="个人信息" />
        <Row gutter={[24, 16]}>
          <Col xs={24} sm={12} md={8} lg={6}>出生日期：1985-06-15</Col>
          <Col xs={24} sm={12} md={8} lg={6}>身份证号：3201**********1234</Col>
          <Col xs={24} sm={24} md={24} lg={12}>常住地址：江苏省南京市玄武区中山路100号</Col>
        </Row>
      </div>
    ),
  },
  {
    key: 'result',
    label: '安检结果',
    children: (
      <CompanyTable
        dataSource={[
          { id: 1, projectName: '燃气表检查', status: '合格' },
          { id: 2, projectName: '管道密封性', status: '合格' },
          { id: 3, projectName: '报警器检查', status: '不合格' },
        ]}
        columns={[
          { title: '检查项目', dataIndex: 'projectName' },
          { title: '状态', dataIndex: 'status', render: (text: string) => <CompanyTag>{text}</CompanyTag> },
        ]}
        pagination={false}
        rowKey="id"
      />
    ),
  },
];

export function CheckDetailPage() {
  return (
    <DetailPageTemplate
      title="安检详情"
      showBack
      avatar="张"
      primaryName="张三"
      tags={tags}
      infoFields={infoFields}
      infoColumns={3}
      tabs={tabs}
      titleActions={<CompanyButton type="primary">编辑</CompanyButton>}
    />
  );
}
```

### StatisticsListPage 速查（统计列表页：统计卡片 + 表格）

模板提供：统计卡片行 + 顶部搜索 + 高级筛选抽屉 + 表格 + 分页。

| Props | 类型 | 说明 |
|---|---|---|
| columns | StatisticsColumn[] | **必传**。列标题字段是 `label`，**antd 的 `title` 不生效** |
| fetchData | (params) => Promise<PageResponse<T>> | 严格 PageResponse 契约（`list` 不是 `records`） |
| statisticCards | StatisticCardConfig[] | 统计卡片行，不传则隐藏 |
| filterFields | StatisticsFilterField[] | 高级筛选抽屉字段 |
| searchKey | string | 顶部搜索框提交的参数名，默认 'keyword' |
| actionColumn | { width?, title?, render } | 自动追加固定右侧的操作列 |
| onRefresh | (filterParams) => void | 传入才显示刷新按钮 |
| defaultPageSize / rowKey / size / scrollY | — | 表格行为 |

最小示例：

```tsx
import { StatisticsListPage } from '@donglegeyu/company-ui';
import type { StatisticsColumn, StatisticCardConfig, PageResponse } from '@donglegeyu/company-ui';

const statisticCards: StatisticCardConfig[] = [
  { key: 'total', title: '安检总数', value: 1280 },
  { key: 'passed', title: '已通过', value: 1024 },
];
const columns: StatisticsColumn<Record<string, unknown>>[] = [
  { key: 'deviceName', label: '设备名称' },
  { key: 'result', label: '安检结果' },
  { key: 'checkTime', label: '安检时间', width: 180 },
];

export function CheckResultPage() {
  return (
    <StatisticsListPage
      title="安检结果查询"
      statisticCards={statisticCards}
      columns={columns}
      searchKey="keyword"
      fetchData={async (): Promise<PageResponse<Record<string, unknown>>> =>
        ({ list: [], total: 0, pageNum: 1, pageSize: 10 })}
    />
  );
}
```

### 页面骨架规则（后台管理系统标准结构）

AI 生成完整后台页面时，**必须使用** **`AdminLayoutTemplate`**——它把「左侧导航 CompanySidebar + 顶部页签 CompanyMultiTabs + 内容区」整条骨架固化成一个组件：页签状态、菜单高亮联动、间距契约全部由组件内部托管。**禁止手动拼装 CompanySidebar + CompanyMultiTabs**（仅当需要完全自定义布局时才作为高级用法）。不允许只生成内容区而遗漏骨架。

#### 内容区页面类型选择

| 页面类型 | 使用组件 | 典型场景 |
|---|---|---|
| 列表页 | `SmartListTemplate` | 用户管理 / 订单列表 / 数据查询 |
| 统计列表页 | `StatisticsListPage` | 仪表盘 / 运营数据 / 报表 |
| 表单页 | `FormPageTemplate` | 新建 / 编辑 / 配置 |
| 详情页 | `DetailPageTemplate` | 用户详情 / 订单详情 |

#### 骨架布局契约（间距已固化在组件内部，无需关心）

```text
┌──────────────┬────────────────────────────────────────┐
│              │ CompanyMultiTabs（高 40px，组件内部自管） │
│  Company     ├────────────────────────────────────────┤
│  Sidebar     │ 内容区（组件内部：零间距滚动容器）        │
│ （AdminLayout│  flex:1; overflow:auto; min-width:0     │
│  Template    │ ┌────────────────────────────────────┐ │
│  内部自管）   │ │ 业务模板（自带全部内间距）             │ │
│              │ │  · padding: 0 12px（左右）           │ │
│  域切换       │ │  · 48px 页头（标题+计数+视图切换）     │ │
│  一级菜单     │ │  · background: --color-bg-layout    │ │
│  二级菜单     │ │  · 筛选区/表格/分页间距内部处理         │ │
│  收藏夹       │ └────────────────────────────────────┘ │
│  用户信息     │                                        │
└──────────────┴────────────────────────────────────────┘
```

**间距契约一句话**：骨架层零间距、模板层全托管——这两层都在 `AdminLayoutTemplate` 内部实现，业务代码不需要也不应该写任何 wrapper 样式。

#### 最小骨架示例（pages 配置式，点菜单自动开页签）

**目标：左侧导航与官方「布局模板」代码演示长得一样。** 一级图标栏结构（首页 / 收藏 / 底部超级搜索）由组件**默认值**提供，与官方 demo 相同——**无需显式传 `firstMenus` / `systemBottomMenus`**（除非用户明确要求定制）。AI 只需提供 `businessMenus`（icon 可省略自动匹配）+ `homeTab`/`homeContent` 成对传入：

```tsx
import { AdminLayoutTemplate, SmartListTemplate, CompanyMessage } from '@donglegeyu/company-ui';
import type { SidebarDomain, SidebarMenuItem, SidebarUserInfo, FieldDefinition } from '@donglegeyu/company-ui';

const domains: SidebarDomain[] = [{ id: 1, domainName: '华东大区' }];
const userInfo: SidebarUserInfo = { id: 1, username: 'admin', nickname: '系统管理员' };

// icon 可省略：组件按 label 关键词自动匹配（见下方「自动匹配机制」）
const businessMenus: SidebarMenuItem[] = [
  { key: 'user', label: '用户管理', children: [
    { key: 'user-list', label: '用户列表', path: '/user/list' },
  ] },
  { key: 'role', label: '角色权限', children: [
    { key: 'role-list', label: '角色列表', path: '/role/list' },
  ] },
  { key: 'dept', label: '部门管理', children: [
    { key: 'dept-list', label: '部门列表', path: '/dept/list' },
  ] },
];
const fields: FieldDefinition[] = [
  { key: 'username', label: '用户名', type: 'input' },
  { key: 'action', label: '操作', type: 'item', width: 180 },
];

export function AdminPage() {
  return (
    <AdminLayoutTemplate
      style={{ height: '100vh' }}
      // 一级图标栏（首页+收藏）与底部「超级搜索」由默认值提供（demo 同款），勿自创
      // logo 可省略：@1.2.39+ 内置默认 Logo（随包 data URI）；传项目路径覆盖；传 "" 回退品牌名首字母
      brandName="物资管理系统"
      domains={domains}
      businessMenus={businessMenus}
      userInfo={userInfo}
      onLogout={() => CompanyMessage.info('退出登录')}
      onDomainChange={(d) => console.log('switch domain', d.domainName)}
      homeTab={{ key: 'home', label: '首页' }}
      homeContent={<div style={{ padding: 24, fontSize: 14 }}>首页</div>}
      pages={[
        { key: 'user-list', label: '用户列表', path: '/user/list',
          component: <SmartListTemplate title="用户列表" fields={fields} dataSource={[]} /> },
      ]}
    />
  );
}
```

**Demo 同款导航核对清单**（生成后自查）：

1. 一级图标栏从上到下：Logo/品牌 → 业务域 → `首页`(home) → `收藏`(favorites) → 业务菜单图标（最多 `visibleMenuCount=6` 个，多出进「更多应用」抽屉）→ 底部 `超级搜索`(search) → 用户信息卡。**其中「首页/收藏/超级搜索」由组件默认值提供（即官方 demo 结构），无需传参**；业务图标来自 `businessMenus[].icon`（省略时自动匹配）
2. `homeTab` + `homeContent` 成对传入（无默认值，漏传则点「首页」无落地页签）
3. 图标名只能用 `SvgIcon` 精选字典里真实存在的名字，**禁止编造**（编造的图标名会渲染为空白）

**自动匹配机制**：菜单不传 `icon` 时，组件按 `label` 关键词自动匹配内置图标（如「用户/会员/客户」→ `people-top-card`、「商品/物料」→ `commodity`、「订单/销售」→ `order`、「库存/仓库」→ `file-cabinet`、「财务/账单」→ `wallet`、「报表/统计」→ `table`），无命中兜底 `folder-open`。传了无效 `icon` 名时开发环境有控制台警告并回退关键词匹配。**因此业务菜单可以完全不写 icon**。

**图标清单**（SvgIcon 精选字典全集 61 个，已对照组件库「图标库」模块 /icon-library 逐字核实；名字必须完全一致，**禁止编造**——未命中精选的名字回退全量 sprite，普通项目未加载 sprite 时渲染空白）：

| 分类 | 图标名 |
|---|---|
| 系统导航（13） | `home` `setting` `logout` `favorites` `star` `star-fill` `pin` `pushpin` `bookmark` `tag` `config` `tool` `radar` |
| 业务通用（18） | `people-top-card` `doc-search` `order` `shopping-cart-del` `buy` `commodity` `file-cabinet` `message-security` `comments` `table` `box` `database-config` `wallet` `scan-setting` `category-management` `user-positioning` `all-application` `folder-open` |
| 操作交互（17） | `search` `edit` `writing-fluently` `more-one` `more` `more-three` `check-one` `close` `add-one` `minus-one` `delete-two` `save` `preview-open` `preview-close-one` `copy` `zoom-in` `zoom-out` |
| 布局排版（8） | `drag` `density` `more-app` `hamburger-button` `waterfalls-h` `list` `pic` `equalizer` |
| 箭头方向（5） | `down` `up` `left` `right` `refresh` |

一级 `businessMenus` 显式传 `icon` 时的推荐值：用户管理 `people-top-card`、角色/权限 `config`、部门/分类 `category-management`、商品 `commodity`、订单 `order`、采购 `buy`、库存 `file-cabinet`、财务 `wallet`、报表 `doc-search`、系统 `database-config`、消息 `comments`、工具 `tool`。⚠️ 注意：省略 `icon` 时自动匹配的结果可能与上表不同（源码关键词表：角色/权限→`message-security`、报表/统计→`table`、系统/设置→`setting`）——想要上表图标就必须显式传。
全量 2658 个 IconPark 图标只能通过 `href` + 已加载的 sprite 引用，**不要**把那些名字当菜单 `icon` 用。

这就是全部——没有手动页签状态、没有菜单高亮联动、没有内容区 wrapper（零间距容器在组件内部）。

#### AdminLayoutTemplate 核心 Props（businessMenus 唯一必填，其余业务建议必传）

| Prop | 类型 | 说明 |
|---|---|---|
| `businessMenus` | `SidebarMenuItem[]` | ⚠️ **唯一类型必填**。业务菜单全集（驱动「更多应用」抽屉 + 二级菜单） |
| `brandName` | `string` | 建议必传：品牌名称（无 Logo 时显示首字母），如 `"物资管理系统"` |
| `domains` | `SidebarDomain[]` | 建议必传：业务域下拉数据（缺了下拉空白） |
| `userInfo` | `SidebarUserInfo` | 建议必传：左下角用户信息卡 |
| `onLogout` | `() => void` | 建议必传：退出登录回调（缺了无退出功能） |
| `pages` | `AdminLayoutPageItem[]` | 建议必传：页面配置（key 与菜单联动），点菜单自动开页签并渲染对应 component |
| `onDomainChange` | `(domain: SidebarDomain) => void` | 建议传：切换业务域回调（业务侧在此重新拉取菜单） |
| `logo` | `string` | 顶部 Logo 路径（**@1.2.39+ 可省略**：内置默认 Logo 随包分发，data URI 零网络）。传项目自己的路径覆盖；传 `""` 回退 `brandName` 首字母。**禁止照抄文档 demo 的 `/logo-dl.svg`**（文档站专属资产，真实项目 404） |

**高级 Props 全部可选**（`tabs` / `activeTabKey` / `defaultActiveKey` / `defaultFavorites` / `customNavMenus` / `homeTab` / `homeContent` / `tabExtra` 等）：页签状态默认组件内部自管——点菜单自动开页签/激活、关页签自动切相邻页签。仅当需要与路由同步时才传受控 props。

**导航结构 Props（可选，默认值即官方 demo 结构）**：

| Prop | 类型 | 默认值 | 说明 |
|---|---|---|---|
| `firstMenus` | `SidebarMenuItem[]` | 首页 + 收藏 | 最左一级图标栏（含图标，如 `{ key: 'home', label: '首页', icon: 'home' }`） |
| `systemBottomMenus` | `SidebarMenuItem[]` | 超级搜索 | 一级图标栏底部固定菜单（默认 `{ key: 'super-search', label: '超级搜索', icon: 'search' }`，即 demo 同款；勿自创「系统设置」等项） |
| `visibleMenuCount` | `number` | `6` | 一级业务导航最多可见数量，超出自动收进「更多应用」抽屉 |
| `customNavMenus` | `SidebarMenuItem[]` | 取 `businessMenus` 前 `visibleMenuCount` 项 | 受控快捷导航（需持久化用户自定义排序时才传，配套 `onCustomNavUpdate`） |
| `homeTab` / `homeContent` | `MultiTabHomeConfig` / `ReactNode` | - | 首页页签配置与首页内容（**建议成对传**，无默认值，漏传点「首页」无落地页签） |

> ⚠️ 生成的页面导航结构与官方 demo 不一致时，**优先排查**：`homeTab`/`homeContent` 是否漏传、`businessMenus[].icon` 是否传了编造的名字、是否多余传了自创的 `firstMenus`/`systemBottomMenus`——典型现象：业务菜单超过 6 个时多出的菜单默认收进「更多应用」抽屉；没传 `homeTab`/`homeContent` 时一级栏「首页」图标点不出内容。

**内容模式二选一**：

1. `pages` 配置式（推荐）：key 与菜单联动，点菜单自动渲染对应 component
2. `children` 兜底：直接放业务模板（`pages` 无匹配页签时渲染）

#### 数据契约（AdminLayoutTemplate props 用）

```ts
// ⚠️ 字段名必须和这里一致，否则渲染空白
interface SidebarDomain { id: number | string; domainName: string; }  // ⚠️ 是 domainName，不是 name！
interface SidebarMenuItem { key: string; label: string; path?: string; icon?: string; iconNode?: React.ReactNode; /* 优先于 icon，完全遮蔽库图标——仅限内置集没有的图标，禁传 antd 图标 */ children?: SidebarMenuItem[]; }
interface SidebarUserInfo { id?: number | string; username?: string; nickname?: string; realName?: string; avatar?: string; }
interface SidebarFavoriteItem { menuKey: string; menuLabel: string; menuPath?: string; sort?: number; }
interface MultiTabItem { key: string; label: React.ReactNode; path?: string; closable?: boolean; disabled?: boolean; }
interface AdminLayoutPageItem { key: string; label: string; path?: string; component?: React.ReactNode; closable?: boolean; }
```

#### 手动拼装（仅高级用法）

`CompanySidebar` + `CompanyMultiTabs` 仍独立导出，但**仅当 AdminLayoutTemplate 无法满足**（如完全自定义布局、页签与复杂路由系统深度耦合）时才手动拼装。手动拼装需要正确处理 CompanySidebar 的 24 个 props、页签增删联动和零间距内容区 wrapper——详见组件文档。**普通后台页面一律用 AdminLayoutTemplate。**

***

## 六、高频反模式（AI 最容易犯的 17 个错）

### 组件已自带能力（AI 勿手加）——先看这张表再写任何装饰

核心心智：**文档告诉你「该用什么组件」，这张表告诉你「哪些能力组件已自带、你只需传数据/配置」**。给已自带的能力再手加一层 = 视觉/行为不一致。

| 能力 | 组件已自带 | AI 勿手加 |
|---|---|---|
| 导航图标 | `SidebarMenuItem` 不传 `icon` 时**按 label 关键词自动匹配**内置图标（如「用户管理」→ `people-top-card`、「商品」→ `commodity`），兜底 `folder-open` | ❌ 勿传 `iconNode={<SettingOutlined/>}` 等 antd 图标——源码里 `iconNode || <SvgIcon>`，传了会**完全遮蔽**库图标系统，线性/填充风格混用 |
| 品牌 Logo | `@1.2.39+` 内置默认 Logo（随包 data URI） | ❌ 勿自传 `logo="/logo.svg"`——仅当用户明确要求自定义品牌时才传 |
| 导航结构 | `firstMenus` 默认（首页+收藏）+ `systemBottomMenus` 默认（超级搜索），即官方 demo 结构 | ❌ 勿自创 `systemBottomMenus=[{ key: 'system-settings', ... }]` 等导航项——默认值就是 demo 同款 |
| 后台骨架 | `AdminLayoutTemplate` 内置侧边栏+页签+内容区 | ❌ 勿手拼 `CompanySidebar + CompanyMultiTabs`（高级用法除外） |
| 内容区间距 | 业务模板自带内边距+背景色，`AdminLayoutTemplate` 内部零间距容器 | ❌ 勿在 `contentStyle`/外层容器加 padding/margin/background |
| 工具栏按钮 | 模板工具栏为**纯文字** `CompanyButton` | ❌ 勿给按钮加 antd icon，与官方 demo 不一致 |
| 头部统计/装饰 | 头部由业务模板管理 | ❌ 勿自创 `titleExtra` 统计标签（「共 X / 启用 X」）——用户没提计数就不传 |

**写任何装饰/图标/额外 UI 到业务模板前，先自问：这个能力组件是否已自带？若是，只传数据/配置，不手加。**

| # | 反模式 | 正确做法 |
|---|---|---|
| 1 | `import { Button } from 'antd'` | `import { CompanyButton } from '@donglegeyu/company-ui'` |
| 2 | `style={{ color: '#F95914' }}` | `style={{ color: token.colorPrimary }}`（用 `useToken()`） |
| 3 | `data.records` | `data.list`（PageResponse 的字段是 `list`） |
| 4 | `<Button type="primary">` | `<CompanyButton type="primary">` |
| 5 | `export default function Foo()` | `export function Foo()` 或 `export const Foo = () =>` |
| 6 | `function handle(data: any)` | 定义 `interface` 并使用 |
| 7 | CSS 里写 `!important` | 用 ConfigProvider 组件级覆盖或内联 style |
| 8 | `Space direction="vertical"` | `Space orientation="vertical"`（antd 6 废弃 `direction`） |
| 9 | `size="middle"` | `size="medium"`（antd 6 默认值改为 `medium`） |
| 10 | 忘记 `ThemeProvider` 包裹 | 入口必须 `<ThemeProvider><App /></ThemeProvider>` |
| 11 | 凭记忆写 props 名称 | 涉及业务模板组件（`SmartListTemplate`/`CompanySidebar`/`StatisticsListPage` 等）时，**先查阅组件库类型定义**（`node_modules/@donglegeyu/company-ui/dist/es/index.d.ts`）确认 props 名称，避免因版本差异导致 prop 拼写错误（如 `SidebarDomain` 实际字段是 `domainName` 而非 `name`） |
| 12 | 手动拼装 Filter + Table + Pagination | **禁止手动拼装列表页**！用 `SmartListTemplate` 一行搞定。也别用 `<CompanyInput placeholder="用户名：">` 当筛选项——placeholder 不是 label，用 `FieldDefinition.label` |
| 13 | 内容区外层加 `padding` / `margin` / `background` 导致间距叠加 | **业务模板自带内边距和背景**（`SmartListTemplate` 有 `padding: 0 12px` + `--color-bg-layout` 背景）。用 `AdminLayoutTemplate` 时零间距容器在组件内部、`contentStyle` 禁止加 padding/margin/background；手动拼装时内容区容器只写 `flex: 1; overflow: auto; min-width: 0` |
| 14 | 自创用户没要的插槽内容（如标题旁统计标签「共 X / 启用 X」）、工具栏用图标按钮 | **插槽纪律**：统计标签是自定义 titleExtra 而非模板内置，用户没提计数就不传；工具栏按钮一律 `CompanyButton` + 文字标签；行内操作用 `ActionCell` 配 `bodyCell`（详见「插槽纪律」一节） |
| 15 | `iconNode: <SettingOutlined />`（antd 图标进菜单） | `icon: 'setting'` 字符串键（键名查 `/icon-library` 精选集）；业务菜单可不写 icon，**组件按菜单名自动匹配**。iconNode 仅限内置图标集没有的图标，禁止传 `@ant-design/icons` 组件（视觉风格不一致） |
| 16 | `<CompanyTag color="blue">`（preset 色板名） | **preset 色板名（blue/purple/cyan/gold 等约 20 个）是 antd 静态硬编码，绕过主题治理**，渲染出默认蓝。标签颜色一律用语义名 `success` / `processing` / `warning` / `error` / `default`（走品牌化语义 token）或品牌 hex |
| 17 | 一个列表多个字段都用彩色标签（状态/等级/推送/类型全上标签） | **非必要不用标签**——标签样式一多列表显得杂乱。一般仅当前列表的**核心状态字段**用标签（1 个为宜），其余字段用纯文本/徽标等弱样式 |

***

## 七、完整导入清单速查

```tsx
// 基础组件（按需）
import {
  CompanyButton,
  CompanyInput,
  CompanySelect,
  CompanyTable,
  CompanyDatePicker,
  CompanyModal,
  CompanyDrawer,
  CompanyMessage,
  CompanyForm,
  CompanyTag,
  CompanySpace,
  // ... 其他 CompanyXxx
} from '@donglegeyu/company-ui';

// 共享组件
import {
  ActionCell,
  BaseInfoForm,
  EllipsisText,
  PageTitle,
  SectionTitle,
  FormFooterActions,
} from '@donglegeyu/company-ui';

// 业务模板
import {
  AdminLayoutTemplate,   // ⭐ 后台骨架（优先用，禁止手动拼 Sidebar + MultiTabs）
  SmartListTemplate,
  StatisticsListPage,
  FormPageTemplate,
  DetailPageTemplate,
  CompanySidebar,        // 高级用法：手动拼装骨架时才用
  CompanyMultiTabs,      // 高级用法：手动拼装骨架时才用
  FilterForm,
  ColumnSettingsPanel,
} from '@donglegeyu/company-ui';

// 主题
import { ThemeProvider, companyTheme, designRules } from '@donglegeyu/company-ui';

// 类型
import type {
  FieldDefinition,
  ColumnField,
  FormField,
  DetailFieldItem,
  DetailTagItem,
  DetailTabItem,
  StatisticCardConfig,
  StatisticsFilterField,
  StatisticsColumn,
  PageResponse,
  SidebarMenuItem,
  SidebarDomain,
  MultiTabItem,
  BaseInfoFormRef,
} from '@donglegeyu/company-ui';

// antd 白名单（仅这些可直接用）
import { Space, Tag, Form, Row, Col, theme } from 'antd';
```

***

## 八、防白屏清单

生成代码后必须逐项确认：

- [ ] 入口文件有 `createRoot().render()` 挂载到 DOM

- [ ] 应用被 `<ThemeProvider>` 包裹

- [ ] 读取嵌套对象用可选链 `data?.result?.list`

- [ ] 数组操作前兜底 `(list || []).map(...)`

- [ ] 异步操作用 `try...catch` 包裹

- [ ] 无 `any` 类型

- [ ] 无 antd 原生组件（白名单除外）

- [ ] 无硬编码颜色

- [ ] 无 `!important`

- [ ] 无默认导出

- [ ] 业务模板组件 props 已查阅类型定义确认（而非凭记忆）

***

## 九、生成后自动校验（治理闭环环节③）

生成代码不是终点，**校验通过才是**。七条红线与数据契约已做成可执行检查（`ai-lint.mjs`，零依赖纯 Node），生成后必须运行并修复至 0 error：

```bash
curl -O https://raw.githubusercontent.com/donglegeyu/company-ui-skills/main/scripts/ai-lint.mjs
node ai-lint.mjs src/
```

| 规则 | 级别 | 检查内容 |
|---|---|---|
| `no-antd-import` | error | 从 antd 导入白名单外组件（`import type` 类型导入豁免） |
| `no-hardcoded-color` | error | 硬编码 `#hex` / `rgb()` 颜色 |
| `no-important` | error | 样式 `!important` |
| `no-any` | error | `any` 类型 |
| `no-default-export` | error | 默认导出 |
| `no-antd-icon-node` | error | `iconNode` 传 antd 图标 |
| `page-response-contract` | warning | `.records` / `dataList`（PageResponse 字段是 `list`） |
| `prefer-template` | warning | CompanyTable+CompanyPagination 手动拼列表 / 手动拼 CompanySidebar+CompanyMultiTabs |
| `antd-6-api` | warning | `direction="vertical"`（用 orientation）/ `size="middle"`（用 medium） |
| `no-preset-tag-color` | warning | `<CompanyTag color="blue">` 等 preset 色板名（绕过主题治理，用语义名 success/processing/warning/error/default） |
| `excessive-tags` | warning | 单文件标签使用过多（≥6 处）——一般仅列表核心状态字段用标签，非核心字段用纯文本 |

**工作流：生成 → 校验 → 修复 → 复跑，直至 0 error**。warning 逐条确认，确属例外加豁免注释（行尾 `// lint-ai-disable-line`，文件头 `/* lint-ai-disable 规则名 */`）并注明原因——豁免是例外登记，不是绕过规范。治理闭环完整模型（五环 / 漂移四来源对策 / 主观意见三级分流 / 回写流程）见文档站「治理闭环 Governance」页。

***

## 十、与已有 Skill 的关系

| Skill | 定位 | 何时用 |
|---|---|---|
| `company-ui-for-agents`（本 Skill） | 组件库消费总入口，规则基座 | 任何使用 company-ui 的代码生成 |
| `pc-prototype-builder` | PC 原型一键生成 | 快速搭建后台系统原型 |
| `smart-list-template-builder` | 智能列表页还原 | 1:1 还原标准列表页 |
| `dumi-theme-customizer` | Dumi 主题定制 | 修改文档站点样式 |

本 Skill 是其他三个 Skill 的规则基础——`pc-prototype-builder` 和 `smart-list-template-builder` 的代码生成规则均源自本 Skill 的七条红线和组件清单。

***

## 版本历史

| 版本 | 日期 | 变更 |
|---|---|---|
| 1.2.0 | 2026-10-23 | 反模式 15→17 条（Tag preset 色板禁用 / 非必要不用标签）；校验器 v1.1.0 新增 `no-preset-tag-color`、`excessive-tags`；新增版本自检机制 |
| 1.1.0 | 2026-10-22 | 六条红线→七条（图标一律用内置图标键）；反模式 12→15 条；新增「九、生成后自动校验」章节（ai-lint.mjs，9 条规则）；AdminLayoutTemplate Props 口径源码级修正（businessMenus 唯一必填、底部默认「超级搜索」） |
| 1.0.0 | 2026-10-19 | 初版：六条红线、组件清单、令牌规范、数据契约、12 条反模式 |
