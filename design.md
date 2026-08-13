# Company UI Design Language

> This document is built for AI agents and design tools. It describes the visual language, design tokens, and component archetypes of Company UI — an enterprise component library based on Ant Design 6 with a custom brand theme.

## Quick Facts

| Property | Value |
|---|---|
| Library | `@donglegeyu/company-ui` |
| Version | 1.2.34 |
| Base | Ant Design 6.x |
| Brand Primary Color | `#F95914` (vitality orange) |
| Font Size | 14px (base) |
| Border Radius | 6px (base) |
| Control Height | 32px (base) |
| Theme System | Ant Design 3-layer Token (Seed / Map / Alias) |

## Installation

```bash
npm install @donglegeyu/company-ui
```

Peer dependencies: `antd >=6.0.0`, `react >=16.8.0`, `@ant-design/icons >=5.0.0`.

## Minimal Setup

```tsx
import React from 'react';
import { ThemeProvider } from '@donglegeyu/company-ui';
import { CompanyButton } from '@donglegeyu/company-ui';

export default function App() {
  return (
    <ThemeProvider>
      <CompanyButton type="primary">Brand Button</CompanyButton>
    </ThemeProvider>
  );
}
```

`ThemeProvider` wraps `ConfigProvider` with `companyTheme` — all components inherit the brand tokens automatically.

## Critical Rules for AI Agents

1. **NEVER use antd native components directly.** Always use `CompanyXxx` (e.g. `CompanyButton`, not `Button`).
2. **NEVER hardcode colors.** Use `theme.useToken()` from antd to read tokens at runtime, or reference `designRules` constants.
3. **NEVER use `!important`** in component styles. Use ConfigProvider component-level overrides or CSS nesting.
4. **NEVER use `any` type.** Define explicit TypeScript interfaces for all props and return values.
5. **NEVER use default exports** for new components. Always use named exports.
6. Import from `@donglegeyu/company-ui`, not from `antd` directly (except for types and whitelisted components: `Space`, `Tag`, `Form`, `Row`, `Col`, `Popover`, `Menu`, `App`).

## Brand Color System

### Primary Brand Color

| Token | Value | Usage |
|---|---|---|
| `colorPrimary` | `#F95914` | Primary buttons, links, selected states, focus rings |
| `colorPrimaryHover` | `#E04E0E` | Hover state for primary elements |
| `colorPrimaryActive` | `#D94D0D` | Active/pressed state |
| `colorPrimaryBg` | `#FFF1E6` | Light background for selected/hover rows |
| `colorPrimaryBgHover` | `#FFE4CC` | Hover background for primary-tinted areas |
| `colorPrimaryBorder` | `#F95914` | Border color for primary-tinted elements |

### Status Colors

| Token | Value | Usage |
|---|---|---|
| `colorSuccess` | `#00B42A` | Success messages, completed status |
| `colorWarning` | `#FF7D00` | Warning messages, pending status |
| `colorError` | `#F53F3F` | Error messages, danger actions |
| `colorInfo` | `#F95914` | Info messages (uses brand color) |

### Neutral Colors

| Token | Value | Usage |
|---|---|---|
| `colorTextBase` | `#1D2129` | Primary text |
| `colorTextSecondary` | `rgba(0, 0, 0, 0.65)` | Secondary text |
| `colorTextTertiary` | `rgba(0, 0, 0, 0.45)` | Tertiary/placeholder text |
| `colorTextQuaternary` | `rgba(0, 0, 0, 0.25)` | Disabled text |
| `colorBgBase` | `#ffffff` | Base background |
| `colorBgContainer` | `#ffffff` | Container background (cards, inputs) |
| `colorBgElevated` | `#ffffff` | Elevated background (modals, dropdowns) |
| `colorBgLayout` | `#f5f5f5` | Page layout background |
| `colorBgContainerDisabled` | `#f5f5f5` | Disabled background (pure gray, no blue tint) |
| `colorBorder` | `#E5E6EB` | Default border |
| `colorBorderSecondary` | `#f0f0f0` | Secondary border (dividers) |
| `colorTextLightSolid` | `#ffffff` | Text on dark/primary backgrounds |

### Preset Colors (13)

Used by `Tag`, `Badge`, `Calendar`, and other components that support color presets.

| Preset | Value |
|---|---|
| blue | `#1677FF` |
| purple | `#722ED1` |
| cyan | `#13C2C2` |
| green | `#52C41A` |
| magenta | `#EB2F96` |
| pink | `#EB2F96` |
| red | `#F5222D` |
| orange | `#FA8C16` |
| yellow | `#FADB14` |
| volcano | `#FA541C` |
| geekblue | `#2F54EB` |
| lime | `#A0D911` |
| gold | `#FAAD14` |

## Design Token System (3-Layer)

Company UI follows Ant Design's 3-layer token architecture. All tokens are defined in `src/theme/GlobalTheme.ts`.

### Layer 1: Seed Tokens

These are the foundational values from which all other tokens derive.

```js
const seedToken = {
  // Brand
  colorPrimary: '#F95914',
  colorSuccess: '#00B42A',
  colorWarning: '#FF7D00',
  colorError: '#F53F3F',
  colorInfo: '#F95914',

  // Neutral
  colorTextBase: '#1D2129',
  colorBgBase: '#ffffff',
  colorBorder: '#E5E6EB',
  colorTextLightSolid: '#ffffff',

  // Border radius
  borderRadius: 6,
  borderRadiusXS: 2,
  borderRadiusSM: 4,
  borderRadiusLG: 8,
  borderRadiusXL: 10,

  // Control height
  controlHeight: 32,
  controlHeightXS: 24,
  controlHeightSM: 28,
  controlHeightLG: 44,

  // Font size
  fontSize: 14,
  fontSizeXS: 12,
  fontSizeSM: 13,
  fontSizeLG: 16,
  fontSizeXL: 18,

  // Spacing
  spacing: 8,
  spacingXS: 4,
  spacingSM: 6,
  spacingLG: 16,
  spacingXL: 24,

  // Shadow
  boxShadow: '0 2px 8px rgba(0,0,0,0.08)',
  boxShadowSecondary: '0 4px 16px rgba(0,0,0,0.12)',

  // Line
  lineWidth: 1,
  lineWidthBold: 2,

  // Motion
  motionDurationFast: '0.1s',
  motionDurationMid: '0.2s',
  motionDurationSlow: '0.3s',
};
```

### Layer 2: Map Tokens

Derived from Seed Tokens, these provide semantic mappings.

```js
const mapToken = {
  // Button padding
  buttonPaddingBlock: 4,
  buttonPaddingInline: 12,
  buttonPaddingBlockSM: 2,
  buttonPaddingInlineSM: 8,
  buttonPaddingBlockLG: 8,
  buttonPaddingInlineLG: 16,

  // Primary color derivatives
  colorPrimaryHover: '#E04E0E',
  colorPrimaryActive: '#D94D0D',
  colorPrimaryBg: '#FFF1E6',
  colorPrimaryBgHover: '#FFE4CC',
  colorPrimaryBorder: '#F95914',

  // Text colors
  colorTextQuaternary: 'rgba(0, 0, 0, 0.25)',
  colorTextTertiary: 'rgba(0, 0, 0, 0.45)',
  colorTextSecondary: 'rgba(0, 0, 0, 0.65)',

  // Background colors
  colorBgContainer: '#ffffff',
  colorBgElevated: '#ffffff',
  colorBgLayout: '#f5f5f5',
  colorBgSpotlight: 'rgba(0, 0, 0, 0.85)',
  colorBgContainerDisabled: '#f5f5f5',

  // Borders
  colorBorderSecondary: '#f0f0f0',
};
```

### Layer 3: Alias Tokens

Automatically computed by Ant Design from Seed + Map tokens. No manual definition needed. Examples: `colorPrimaryButtonBg`, `colorPrimaryText`, etc.

### Component-Level Token Overrides

Some components require explicit token overrides beyond the global tokens:

```js
components: {
  Button: {
    borderRadius: 6,
    controlHeight: 32,
    paddingInline: 12,
    paddingBlock: 4,
    defaultBgDisabled: '#f5f5f5',
    borderColorDisabled: '#d9d9d9',
    dashedBgDisabled: '#f5f5f5',
  },
  Input: {
    borderRadius: 6,
  },
  Select: {
    borderRadius: 6,
    controlHeight: 32,
  },
  Table: {
    borderRadius: 6,
    headerBorderRadius: 4,
    cellPaddingBlock: 7,
  },
}
```

## Design Rules Constants

For runtime access to design values (e.g. in non-antd contexts), use the `designRules` export:

```tsx
import { designRules } from '@donglegeyu/company-ui';

// Colors
designRules.colors.brand          // '#F95914'
designRules.colors.brandLight     // '#FFF1E6'
designRules.colors.brandDark      // '#E04E0E'
designRules.colors.success        // '#00B42A'
designRules.colors.warning        // '#FF7D00'
designRules.colors.error          // '#F53F3F'

// Border radius
designRules.borderRadius.base     // 8
designRules.borderRadius.sm       // 6
designRules.borderRadius.xs       // 2
designRules.borderRadius.lg       // 12
designRules.borderRadius.xl       // 16

// Sizes
designRules.size.height.base      // 36
designRules.size.height.sm        // 28
designRules.size.height.lg        // 44
designRules.size.font.base        // 14
designRules.size.spacing.base     // 8

// Shadows
designRules.shadows.small         // '0 2px 8px rgba(0,0,0,0.08)'
designRules.shadows.medium        // '0 4px 16px rgba(0,0,0,0.12)'

// Motion
designRules.motion.fast           // '0.1s'
designRules.motion.normal         // '0.2s'
designRules.motion.slow           // '0.3s'
```

## Differences from Ant Design Default Theme

This is the most critical table for AI agents. These tokens are **overridden** — do not use antd's default values.

| Token | antd Default | Company UI | Reason |
|---|---|---|---|
| `colorPrimary` | `#1677FF` (blue) | `#F95914` (orange) | Brand identity |
| `colorInfo` | `#1677FF` | `#F95914` | Info uses brand color |
| `colorBgContainerDisabled` | `rgba(0,0,0,0.04)` (blue tint) | `#f5f5f5` (pure gray) | Remove blue tint from disabled |
| `Button.defaultBgDisabled` | derived (blue tint) | `#f5f5f5` | Pure gray disabled |
| `Button.borderColorDisabled` | derived (blue tint) | `#d9d9d9` | Pure gray disabled |
| `Button.dashedBgDisabled` | derived (blue tint) | `#f5f5f5` | Pure gray disabled |
| `borderRadius` | `6` | `6` | Same |
| `controlHeight` | `32` | `32` | Same |
| `fontSize` | `14` | `14` | Same |

## Component Archetypes

### Basic Components (72+)

All antd 6 components are wrapped as `CompanyXxx` with brand theme injection. Each wrapper uses `ConfigProvider` with `companyTheme` to ensure brand tokens propagate.

**Static property components** (Radio, Checkbox, Select, etc.) use `Object.assign` to mount static properties (`.Group`, `.Button`, `.Option`), with each visual static property independently wrapped in `ConfigProvider`.

**Config marker components** (`Select.Option`, `TreeSelect.TreeNode`, `Form.Item`, `Form.useForm`) are passed through directly without extra `ConfigProvider` wrapping.

### Shared Components (14)

Business-level reusable components built on top of basic components:

| Component | Purpose |
|---|---|
| `ActionCell` | Table action column with auto-collapse overflow buttons |
| `BaseInfoForm` | Multi-column form with field-driven config |
| `EllipsisText` | Text overflow with tooltip |
| `PageTitle` | Standard page header |
| `SectionTitle` | Section divider with title |
| `FormFooterActions` | Sticky form footer with submit/cancel |
| `FilterOptionsDrawer` | Filter options in drawer |
| `ColorScale` | Color gradient picker |
| `AccessibilityChecker` | Accessibility audit tool |
| `SvgIcon` | SVG icon component with manifest |
| `IconGallery` | Icon browser gallery |
| `IconParkGallery` | IconPark icon browser |
| `IconExplorer` | Combined icon explorer |
| `useIconPark` | Hook for IconPark icons |

### Template Components (9)

Full page-level templates for common business scenarios:

| Component | Purpose |
|---|---|
| `SmartListTemplate` | Smart list page (filter + table + pagination + column settings + view switching) |
| `StatisticsListPage` | Statistics list page (stat cards + advanced filter + table) |
| `FormPageTemplate` | Form page (grouped multi-column + footer actions) |
| `DetailPageTemplate` | Detail page (info card + tags + multi-tab) |
| `CompanySidebar` | Left dual-layer navigation sidebar |
| `CompanyMultiTabs` | Browser-style multi-tab bar |
| `FilterForm` | Standalone filter form component |
| `ColumnSettingsPanel` | Column visibility/order settings panel |
| `IconSelect` | Icon picker with search |

## Key Data Contracts

### PageResponse (API standard)

```ts
interface PageResponse<T> {
  list: T[];
  total: number;
  pageNum: number;
  pageSize: number;
}
```

All paginated API responses must use this structure. The field is `list`, NOT `records` or `dataList`.

### FieldDefinition (SmartListTemplate)

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

### ColumnField (column settings)

```ts
interface ColumnField {
  key: string;
  label: string;
  visible: boolean;
  width?: number;
  fixed?: 'left' | 'right';
}
```

### FormField (BaseInfoForm)

```ts
interface FormField {
  name: string;
  label: string;
  type: 'input' | 'select' | 'input-number' | 'date' | 'textarea' | ...;
  required?: boolean;
  rules?: Rule[];
  options?: { label: string; value: string | number }[];
}
```

## Dark Mode

Company UI supports dark mode via `data-prefers-color="dark"` attribute on the `html` element. Ant Design's built-in dark algorithm handles token inversion automatically. Brand primary color `#F95914` remains the same in both modes.

Key dark mode behaviors:
- `colorBgContainer` → dark surface
- `colorBgLayout` → dark background
- `colorBorder` → dark border
- Brand orange stays unchanged
- Disabled states use dark gray instead of light gray
