---
name: "company-ui"
description: "AI Agent 消费 @donglegeyu/company-ui 组件库的总入口。强制 AI 使用 CompanyXxx 组件（而非 antd 原生）、遵循品牌令牌 #F95914、遵守类型安全和样式规范。在任何使用 company-ui 的项目中生成 React 代码时必须加载。"
---

# Company UI for Agents

> **版本基准**：`@donglegeyu/company-ui@1.2.34`，基于 Ant Design 6.x，品牌主色 `#F95914`。
>
> 本 Skill 是 AI Agent 消费组件库的「驾照」——确保生成的代码合规、可运行、品牌一致。
>
> **在线文档**：https://nrpdemo.zrhsh.com  |  **LLMs.txt**：https://nrpdemo.zrhsh.com/llms.txt  |  **design.md**：https://nrpdemo.zrhsh.com/design.md

## 何时调用

- 用户项目已安装 `@donglegeyu/company-ui`，需要生成 React/TypeScript 代码
- 用户提到 CompanyButton / CompanyTable / CompanySelect / SmartListTemplate 等组件名
- 用户要求生成后台管理页面、列表页、表单页、详情页
- 用户要求使用公司组件库、企业 UI 规范
- 任何涉及 antd 组件的代码生成场景（应自动替换为 CompanyXxx）

---

## 一、五条红线（违反即不合格）

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

---

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

---

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

### 业务模板组件（9 个）

| 组件 | 导出名 | 用途 |
|---|---|---|
| 智能列表模板 | `SmartListTemplate` | 筛选+表格+分页+列设置+视图切换 |
| 统计列表页 | `StatisticsListPage` | 统计卡片+高级筛选+表格 |
| 表单页模板 | `FormPageTemplate` | 分组多列表单+底部操作栏 |
| 详情页模板 | `DetailPageTemplate` | 信息卡片+标签+多 Tab |
| 侧边导航 | `CompanySidebar` | 左侧双层导航 |
| 多页签栏 | `CompanyMultiTabs` | 浏览器式页签栏 |
| 筛选表单 | `FilterForm` | 独立筛选表单 |
| 列设置面板 | `ColumnSettingsPanel` | 列可见性/顺序设置 |
| 图标选择器 | `IconSelect` | 图标选择器 |

---

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
      padding: token.spacing,
      borderRadius: token.borderRadius,
      fontSize: token.fontSize,
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

---

## 五、业务模板数据契约

### PageResponse（标准分页响应）

```ts
interface PageResponse<T> {
  list: T[];       // 注意：是 list，不是 records 或 dataList
  total: number;
  pageNum: number;
  pageSize: number;
}
```

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
  sortable?: boolean;
}
```

- `type: 'item'` → 不出现在筛选区，仅作表格列（通常用于操作列，key 必须是 `'action'`）
- `hideInFilter: true` → 字段仍作表格列，但不进筛选区

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

---

## 六、高频反模式（AI 最容易犯的 10 个错）

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

---

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
  SmartListTemplate,
  StatisticsListPage,
  FormPageTemplate,
  DetailPageTemplate,
  CompanySidebar,
  CompanyMultiTabs,
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

---

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
