# tdocdesign.md
在 WorkBuddy / CodeBuddy 等 Agent 产品中，当用户选中本文件tdocdesign.md作为「设计参考」时，AI 应严格依据本文件生成 100% 还原腾讯文档（Tencent Docs）视觉风格的 HTML/React/Vue 页面。
# 腾讯文档（Tencent Docs）Design System · tdocdesign.md

> 适用范围：在 WorkBuddy / CodeBuddy 等 Agent 产品中，当用户选中本文件作为「设计参考」时，AI 应严格依据本文件生成 100% 还原腾讯文档（Tencent Docs）视觉风格的 HTML/React/Vue 页面。
>
> 数据来源：
> 1. 实地抓取 `docs.qq.com/desktop/`、`docs.qq.com/home`、`docs.qq.com/copilot`、`docs.qq.com/mall/index` 与文档详情页的 DOM、computed style 和 CSS 变量（dui Design System，腾讯文档官方组件库）。
> 2. **工作台首页（`/desktop/`）以官方 Figma 设计稿「WEB端/首页」（十大品类2025，文件 `QjDK8NOK6xz96R5qkT17x7`，节点 `2196-417771`）为权威源**——抓取节点 484 个 fill、TEXT 字符样式、cornerRadius、padding/itemSpacing 全量遍历，参见 `desktop-figma.png`。
>
> 所有色值、间距、圆角、阴影均为真实值。

---

## 0. 设计哲学（Design Philosophy）

腾讯文档的视觉语言可概括为四个关键词：

1. **克制的蓝（Restrained Blue）** —— 以「腾讯蓝 `#0D61F2 / #1E6FFF`」为唯一品牌焦点，大面积留白与浅灰底（`#F3F5F7`）作衬，按钮、链接、Tab 选中态都收敛在同一蓝系内，避免炫技。
2. **办公的灰（Office Gray）** —— 文字采用三级 alpha 黑（`rgba(0,0,0, .9 / .56 / .26)`），保证长文阅读舒适；卡片纯白 `#FFFFFF`，分隔线 `rgba(0,0,0,.08)`。
3. **AI 的流光（AI Conic Gradient）** —— 在「开物 AI / Copilot」类入口、智能徽标处，使用 `conic-gradient` + `linear-gradient(135deg, 蓝→青)` 表达"智能"，颜色取 `#0081FF → #00BEFF → #1E6FFF`。
4. **协作的暖（Collaborative Warm）** —— 多人协作标签、用户名 chip 使用饱和度更高的彩色徽签（青/紫/粉/黄），轻量的圆角 4px，让数据驱动的页面"有人味"。

**整体气质**：理性、清晰、办公友好；不浮夸、不拟物、不堆砌阴影；信息密度中高；讲究"一眼能扫到、再看不疲劳"。

---

## 1. 颜色系统（Color Tokens）

> 直接复制到 `:root` 即可使用，命名遵循腾讯文档官方 dui Design System。

### 1.1 品牌主色（Accent · Tencent Blue）

| Token | 值 | 用途 |
|---|---|---|
| `--accent-default` | `#0D61F2` | 主按钮底色、链接默认 |
| `--accent-hover` | `#206EF3` | 主按钮/链接 hover、Logo 主蓝 |
| `--accent-pressed` | `#347AF4` | 主按钮 active |
| `--accent-disabled` | `#C2D8FF` | 主按钮禁用底色（文字 `rgba(255,255,255,.5)`） |
| `--accent-bg-default` | `#E8F0FE` | 浅蓝填充（Tag、选中行底） |
| `--accent-bg-hover` | `#D6E4FD` | 浅蓝 hover |
| `--accent-bg-pressed` | `#BFD2FB` | 浅蓝 active |

> 在浅色模式下，**实际生产环境主按钮使用 `#1E6FFF`**（rgb(30,111,255)），与 token `#0D61F2` 同属一族，差异仅 4%。两者均可，本设计建议主按钮统一用 `#1E6FFF`，链接/文字蓝用 `#0D61F2`。

### 1.2 AI 强调色（AI Highlight · 开物 AI 专用）

| Token | 值 | 用途 |
|---|---|---|
| `--ai-cyan` | `#00C8FF` | AI 流光起点 |
| `--ai-blue` | `#1E6FFF` | AI 流光中段 |
| `--ai-deep` | `#0081FF` | AI 流光收尾 |
| `--ai-conic` | `conic-gradient(from 90deg, #00C8FF 0deg, #00C8FF 62deg, #1E6FFF 190deg, #1E6FFF 245deg, #00C8FF 360deg)` | AI 头像/徽标外圈流光 |
| `--ai-text-grad` | `linear-gradient(135deg, rgba(0,0,0,.9) 42%, #0081FF 48%, #00BEFF 52%, rgba(0,0,0,.9) 58%)` | "让创作即刻发生"等 AI 标语扫光文字 |

### 1.3 中性色（Neutral · 文字 / 边框 / 填充）

> 🔬 **实测补充**：来自 Figma「WEB端/首页」节点的 484 个 fill 统计，工作台真实使用的是**带蓝意的深灰文字色阶**（不是纯黑 alpha），生产环境优先使用这套硬编码值；rgba 体系仍可作为通用兜底。

```css
/* === 推荐 · 工作台真实文字色（Figma 直采） === */
--text-title:     #242424;   /* 一级标题 / 文档名 / 列表主信息（注意是带暖的深灰，不是纯黑） */
--text-body:      #454D5A;   /* 正文 / 导航项 / 次要标题 —— 带蓝意的深灰，腾讯文档签名色 */
--text-muted:     #81868F;   /* 辅助说明 / 时间戳 / 占位文字 */
--text-placeholder:#CBCDD1;  /* 输入框占位、未激活图标 */

/* === 通用 · 三级 alpha 黑（用于自由文本场景） === */
--text-primary:   rgba(0, 0, 0, 0.90);   /* ≈ #242424，标题/正文 */
--text-secondary: rgba(0, 0, 0, 0.56);   /* ≈ #81868F，次要信息 */
--text-tertiary:  rgba(0, 0, 0, 0.26);   /* 占位、禁用 */
--text-quaternary:rgba(0, 0, 0, 0.16);   /* 极弱 */
--text-link:      #206EF3;
--text-error:     #ED5050;
--text-vip:       #E59837;               /* 会员金 */
--text-white:     #FFFFFF;

/* 边框 · 透明黑 + 实测灰 */
--border-weak:        rgba(0, 0, 0, 0.06);   /* 卡片分隔 / 列表行底线（实测主导值） */
--border-default:     rgba(0, 0, 0, 0.08);   /* 输入框 / 次要按钮 */
--border-medium:      rgba(0, 0, 0, 0.12);   /* hover 后的次按钮 */
--border-strong:      rgba(0, 0, 0, 0.16);
--border-divider:     #D7D7D7;               /* 顶栏底分隔实色（可选） */

/* 填充 · 灰阶填色（hover/active 反馈层）—— 带蓝意的灰是腾讯文档签名细节 */
--fill-hover:   rgba(51, 77, 102, 0.06);
--fill-active:  rgba(51, 77, 102, 0.08);
--fill-strong:  rgba(51, 77, 102, 0.12);

/* 背景层级 */
--bg-page:   #F3F5F7;   /* body / 桌面工作台底（仅在三栏缝隙露出） */
--bg-card:   #FFFFFF;   /* 卡片、对话框、面板、左右栏 */
--bg-soft:   #F7F9FB;   /* 输入框未聚焦底、Tab 容器 */
--bg-search: #F3F5F7;   /* 顶栏全局搜索框底（与 bg-page 同色，无边框） */
--bg-mask:   rgba(0, 0, 0, 0.65);  /* 模态遮罩 */
```

### 1.4 状态色（Status）

| 用途 | 默认 | Hover | Pressed | Disabled | Bg-Default |
|---|---|---|---|---|---|
| 错误/危险 | `#EB4141` | `#ED5050` | `#EE5F5F` | `#FACACA` | `#FDECEC` |
| 成功 | `#008D4B` | `#1A9B5F` | `#33A973` | `#B8E2CE` | `#E8F6EF` |
| 警告/提示 | `#EB9D00` | `#ED9E0A` | `#EFA32E` | `#F8DDA1` | `#FFF6E5` |
| 会员 VIP | `#E6B76C` | `#DDB068` | `#D4A863` | `#F2D9A8` | `#FFF8EB` |

### 1.5 暗色模式（Dark Mode）

> 腾讯文档 dui 的暗色基底，保留以便切换。

```css
[data-theme="dark"] {
  --bg-page: #131414;
  --bg-card: #18191A;
  --bg-soft: #1C1D1F;
  --bg-elevated: #242529;

  --text-primary:   rgba(255, 255, 255, 0.90);
  --text-secondary: rgba(255, 255, 255, 0.56);
  --text-tertiary:  rgba(255, 255, 255, 0.26);

  --border-weak:    rgba(255, 255, 255, 0.08);
  --border-medium:  rgba(255, 255, 255, 0.12);
  --border-strong:  rgba(255, 255, 255, 0.16);

  --accent-default: #0D61F2;   /* 暗色下主蓝不变 */
  --accent-bg-default: rgba(13, 97, 242, 0.16);
}
```

### 1.6 文档类型识别色（Doc Type Colors）

腾讯文档列表里每种文档类型都有专属颜色，用作图标底色与左侧色条：

| 类型 | 主色 | 背景 |
|---|---|---|
| 📄 文档 (Word) | `#1E6FFF` | `#E8F0FE` |
| 📊 表格 (Excel) | `#08B96A` / `#1FAA62` | `#E6F7EF` |
| 📽 幻灯片 (PPT) | `#FF7A29` / `#FF8D3F` | `#FFF1E6` |
| 📕 PDF | `#E55C5C` / `#EB4141` | `#FDECEC` |
| 📋 收集表 | `#FFC53D` / `#F6B01E` | `#FFF8E1` |
| 🧠 思维导图 | `#9B59FF` | `#F1E8FF` |
| 🗂 智能表格 | `#00B8D9` | `#E0F7FA` |

---

## 2. 字体系统（Typography）

### 2.1 字体族（Font Family）

```css
/* 默认正文：苹方 SC（macOS）→ 微软雅黑（Win）→ 系统 UI */
--font-sans: "PingFang SC", "Microsoft YaHei", "Hiragino Sans GB",
             system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans",
             "Source Han Sans SC", "Noto Sans CJK SC", "WenQuanYi Micro Hei",
             "Helvetica Neue", Helvetica, Arial, sans-serif,
             "Apple Color Emoji", "Segoe UI Emoji";

/* 营销/运营标题专用：HarmonyOS Sans SC 700（Figma 实测用于"超级会员""限时半价优惠"等会员卡片大标题） */
--font-display: "HarmonyOS Sans SC", "PingFang SC", "Microsoft YaHei", sans-serif;

/* 数字字体：DIN Next LT Pro Medium（Figma 实测用于"258MB / 1GB"等容量数字、统计数据） */
--font-number: "DIN Next LT Pro", "DIN Alternate", "PingFang SC", system-ui, sans-serif;

/* 等宽 */
--font-mono: "SFMono-Regular", "Roboto Mono", "Source Code Pro",
             Menlo, Consolas, monospace;
```

> 字体使用规则：①正文/导航/列表/按钮 → `--font-sans`；②运营卡片/会员胶囊/营销 banner 大标题 → `--font-display`；③容量进度、表格中的数字、价格 → `--font-number`。混搭即得"严肃 + 商业活力"的腾讯文档质感。

### 2.2 字号层级（Type Scale）

腾讯文档遵循偶数像素 + 1.4~1.5 行高的工整节奏：

| Token | 字号 | 行高 | 字重 | 用途 |
|---|---|---|---|---|
| `--fs-display` | `48px` | `60px` | 700 | 官网 Hero 主标题 ("让创作即刻发生") |
| `--fs-h1` | `32px` | `44px` | 600 | 页面主标题 |
| `--fs-h2` | `24px` | `34px` | 600 | 区块标题 |
| `--fs-h3` | `20px` | `28px` | 600 | 卡片标题 |
| `--fs-h4` | `18px` | `26px` | 600 | 子区块标题 ("行业模板""职场办公") |
| `--fs-lg` | `16px` | `24px` | 500/400 | 强调正文 / Tab 选中 |
| `--fs-base` | `14px` | `22px` | 400 | 正文（默认） |
| `--fs-sm` | `12px` | `18px` | 400 | 辅助文字、标签、时间戳 |
| `--fs-xs` | `10px` | `14px` | 400 | 角标、密集型徽签 |

> 标题字重统一 600（不要 700/800），避免"过重"破坏办公克制感。

### 2.3 段落与排版规则

- 中英混排时英文/数字与中文之间留 1 个半角空格；
- 长正文行长 ≤ 720px，避免一眼扫不完；
- 数字优先用 `font-variant-numeric: tabular-nums;` 让财务/表格数字对齐；
- 链接默认无下划线，hover 才出现 `text-decoration: underline; text-underline-offset: 3px;`。

---

## 3. 间距与栅格（Spacing & Grid）

### 3.1 间距 Token（4 的倍数）

```css
--sp-1: 4px;    --sp-2: 8px;    --sp-3: 12px;   --sp-4: 16px;
--sp-5: 20px;   --sp-6: 24px;   --sp-8: 32px;   --sp-10: 40px;
--sp-12: 48px;  --sp-16: 64px;  --sp-20: 80px;  --sp-24: 96px;
```

- 卡片内 padding：`--sp-6 (24px)`；
- 区块间距：`--sp-8 ~ --sp-12`；
- Hero 区上下留白：`--sp-20 ~ --sp-24`；
- 列表项垂直 padding：`8px`，水平 padding：`16px`。

### 3.2 布局栅格

- 整页最大宽度：`1200px`（官网/详情）/ `1440px`（工作台）/ `100%`（编辑器）；
- 12 列栅格，gutter `24px`；
- 工作台经典三栏：
  - 左侧导航：`240px`（折叠 `56px`），背景 `#FFFFFF`，无右边框（用阴影分隔）；
  - 顶部栏：`56px` 高，背景 `#FFFFFF`，下边框 `1px solid var(--border-weak)`；
  - 主内容：`flex: 1`，内边距 `24px 32px`，背景 `var(--bg-page)`。

---

## 4. 圆角、阴影与边框

### 4.1 圆角（Radius）

```css
--radius-sm: 4px;    /* 按钮、Tag、Tab、输入框、小标签 —— 腾讯文档主圆角 */
--radius-md: 8px;    /* 卡片、Modal、Toast、文档缩略图 */
--radius-lg: 12px;   /* 大卡片、Banner */
--radius-xl: 16px;   /* AI 输入框、特色卡 */
--radius-pill: 999px; /* 胶囊、头像、Chip */
```

> **特征**：腾讯文档**强烈倾向 4px / 8px**，几乎不出现 16px 以上的"圆润感"。这是它和飞书（更圆，6/10/14）、Notion（10/14）的关键区别。

### 4.2 阴影（Elevation · 4 级）

```css
--shadow-lv1: 0px 1px 4px 0px rgba(0, 0, 0, 0.08);                                    /* 静态卡片 */
--shadow-lv2: 0px 3px 8px 1px rgba(0, 0, 0, 0.08);                                    /* hover、Tooltip */
--shadow-lv3: 0px 5px 12px 4px rgba(0, 0, 0, 0.08);                                   /* Popover、Dropdown */
--shadow-lv4: 0px 24px 48px 2px rgba(0, 0, 0, 0.08), 0px 5px 12px 4px rgba(0,0,0,.08); /* Modal、登录弹层 */
```

> **关键**：阴影**只用黑色 `rgba(0,0,0,.08)`**，不染色（不要 `rgba(13,97,242,.2)` 之类的"蓝阴影"）。这是腾讯文档"克制"的核心。

### 4.3 边框

- 默认 `1px solid var(--border-weak)`（次要按钮、输入框）；
- 强调描边 `1px solid var(--accent-default)`（聚焦/选中态）；
- 分隔线 `1px solid var(--border-weak)`，避免使用虚线。

---

## 5. 组件规范（Components）

### 5.1 按钮（Button）

```html
<!-- 主按钮 · Primary -->
<button class="btn btn-primary">立即使用</button>
<!-- 次按钮 · Default（白底 + 弱描边） -->
<button class="btn btn-default">购买咨询</button>
<!-- 文本按钮 · Text -->
<button class="btn btn-text">查看更多</button>
<!-- 危险按钮 · Danger -->
<button class="btn btn-danger">删除</button>
```

```css
.btn {
  font: 500 14px/1 var(--font-sans);
  border-radius: 4px;
  padding: 0 16px;
  height: 32px;            /* 三档：sm 24, md 32(default), lg 40 */
  border: 1px solid transparent;
  cursor: pointer;
  transition: background .15s ease, color .15s ease, border-color .15s ease;
  display: inline-flex; align-items: center; gap: 6px;
}
.btn-primary { background: #1E6FFF; color: #fff; }
.btn-primary:hover  { background: #347AF4; }
.btn-primary:active { background: #0D61F2; }
.btn-primary:disabled { background: #C2D8FF; color: rgba(255,255,255,.5); cursor: not-allowed; }

.btn-default { background: #FFF; color: rgba(0,0,0,.9); border-color: rgba(0,0,0,.12); }
.btn-default:hover  { background: rgba(51,77,102,.04); border-color: rgba(0,0,0,.16); }
.btn-default:active { background: rgba(51,77,102,.08); }

.btn-text { background: transparent; color: #206EF3; padding: 0 4px; }
.btn-text:hover { background: rgba(13,97,242,.06); }

.btn-danger { background: #EB4141; color: #fff; }
.btn-danger:hover { background: #ED5050; }

/* 尺寸 */
.btn-sm { height: 24px; font-size: 12px; padding: 0 12px; }
.btn-lg { height: 40px; font-size: 14px; padding: 0 20px; }
```

### 5.2 输入框（Input）

```css
.input {
  height: 36px;
  padding: 0 12px;
  background: #FFF;
  border: 1px solid rgba(0,0,0,.12);
  border-radius: 4px;
  font: 14px/1 var(--font-sans);
  color: rgba(0,0,0,.9);
  transition: border-color .15s, box-shadow .15s;
}
.input::placeholder { color: rgba(0,0,0,.26); }
.input:hover { border-color: rgba(0,0,0,.26); }
.input:focus {
  border-color: #1E6FFF;
  box-shadow: 0 0 0 2px rgba(30,111,255,.16);
  outline: none;
}
```

**搜索框（顶部栏特化）**：高度 `40px`，圆角 `8px`，左 16px、右 8px 内边距，左侧 16×16 灰色 search 图标，背景 `#F7F9FB`，无边框，hover 时边框 `1px solid rgba(0,0,0,.08)`，focus 时蓝边 + 白底。

### 5.3 卡片（Card）

```css
.card {
  background: #FFF;
  border-radius: 8px;
  box-shadow: var(--shadow-lv1);
  padding: 24px;
  transition: box-shadow .2s ease, transform .2s ease;
}
.card:hover { box-shadow: var(--shadow-lv2); }
/* 文档缩略卡 */
.doc-card {
  border-radius: 8px;
  background: #FFF;
  overflow: hidden;
  border: 1px solid var(--border-weak);
}
.doc-card .thumb { aspect-ratio: 4/3; background: #F3F5F7; }
.doc-card .meta  { padding: 12px 14px; }
.doc-card .title { font: 500 14px/20px var(--font-sans); color: rgba(0,0,0,.9); }
.doc-card .info  { font: 400 12px/18px var(--font-sans); color: rgba(0,0,0,.56); }
.doc-card:hover  { border-color: rgba(0,0,0,.16); box-shadow: var(--shadow-lv1); }
```

### 5.4 标签 / Tag / Chip

```css
.tag {
  display: inline-flex; align-items: center;
  height: 22px; padding: 0 8px;
  font: 400 12px/1 var(--font-sans);
  border-radius: 4px;
  background: var(--accent-bg-default);   /* #E8F0FE */
  color: var(--accent-default);
}
.tag-success { background: #E6F7EF; color: #008D4B; }
.tag-warning { background: #FFF6E5; color: #C77800; }
.tag-danger  { background: #FDECEC; color: #EB4141; }
.tag-vip     { background: linear-gradient(90deg, #FFE7B0 0%, #FFD27A 100%); color: #8C5300; }
.tag-new     { background: #E8F0FE; color: #1E6FFF; font-weight: 500; }
```

### 5.5 Tab（顶部 Tab · 下划线式）

```css
.tabs { display: flex; gap: 24px; border-bottom: 1px solid var(--border-weak); }
.tab {
  position: relative;
  padding: 12px 0;
  font: 400 16px/24px var(--font-sans);
  color: rgba(0,0,0,.56);
  cursor: pointer;
  transition: color .15s;
}
.tab:hover { color: rgba(0,0,0,.9); }
.tab.active { color: rgba(0,0,0,.9); font-weight: 600; }
.tab.active::after {
  content: '';
  position: absolute; left: 0; right: 0; bottom: -1px;
  height: 2px; border-radius: 2px;
  background: #1E6FFF;
}
```

### 5.6 侧边栏导航（Sidebar）

```css
.sidebar { width: 240px; background: #FFF; padding: 16px 8px; }
.nav-group-title {
  padding: 8px 16px;
  font: 600 14px/20px var(--font-sans);
  color: rgba(0,0,0,.88);
}
.nav-item {
  display: flex; align-items: center; gap: 8px;
  height: 36px; padding: 0 16px;
  font: 400 14px/20px var(--font-sans);
  color: rgba(0,0,0,.9);
  border-radius: 4px;            /* 注意：选中态用 4px 圆角的"轻微高亮"，不是大色块 */
  cursor: pointer;
  transition: background .15s;
}
.nav-item:hover { background: rgba(51,77,102,.06); }
.nav-item.active {
  background: rgba(51,77,102,.06);
  font-weight: 600;
}
.nav-item .icon { width: 16px; height: 16px; opacity: .7; }
```

### 5.7 弹窗 / Modal

```css
.modal-mask { position: fixed; inset: 0; background: rgba(0,0,0,.65); }
.modal {
  background: #FFF;
  border-radius: 8px;
  box-shadow: var(--shadow-lv4);
  min-width: 480px;
  padding: 24px;
}
.modal-title  { font: 600 18px/26px var(--font-sans); color: rgba(0,0,0,.9); margin-bottom: 8px; }
.modal-body   { font: 400 14px/22px var(--font-sans); color: rgba(0,0,0,.56); }
.modal-footer { display: flex; justify-content: flex-end; gap: 8px; margin-top: 24px; }
```

### 5.8 头像与协作者（Avatar / Collaborator）

```css
.avatar {
  width: 28px; height: 28px;
  border-radius: 50%;
  border: 2px solid #FFF;
  display: inline-flex; align-items: center; justify-content: center;
  color: #FFF; font: 600 12px/1 var(--font-sans);
}
/* 头像背景在 8 色中循环 */
.avatar:nth-child(1) { background: #1E6FFF; }
.avatar:nth-child(2) { background: #00B8D9; }
.avatar:nth-child(3) { background: #08B96A; }
.avatar:nth-child(4) { background: #FFC53D; color: rgba(0,0,0,.9); }
.avatar:nth-child(5) { background: #FF7A29; }
.avatar:nth-child(6) { background: #E55C5C; }
.avatar:nth-child(7) { background: #9B59FF; }
.avatar:nth-child(8) { background: #FF6B9D; }

.avatar-stack { display: inline-flex; }
.avatar-stack .avatar + .avatar { margin-left: -8px; }
```

### 5.9 通知 Banner（顶部公告条）

```css
.notice-banner {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  height: 36px;
  background: #E8F0FE;             /* 浅蓝 */
  font: 400 13px/1 var(--font-sans);
  color: rgba(0,0,0,.9);
}
.notice-banner .badge {
  padding: 2px 6px; border-radius: 4px;
  background: #1E6FFF; color: #FFF; font-size: 11px; font-weight: 600;
}
.notice-banner a { color: #1E6FFF; margin-left: 4px; }
```

### 5.10 AI 入口 / 智能徽标（开物 AI 专用）

```css
/* ✨ AI 流光头像（圆形、外圈 conic-gradient） */
.ai-avatar {
  width: 32px; height: 32px;
  border-radius: 50%;
  background: var(--ai-conic);
  display: grid; place-items: center;
  position: relative;
}
.ai-avatar::after {
  content: '';
  position: absolute; inset: 2px;
  border-radius: 50%;
  background: #FFF;
}
.ai-avatar .icon { position: relative; z-index: 1; color: #1E6FFF; }

/* ✨ AI 扫光标题（"让创作即刻发生"） */
.ai-headline {
  font: 600 48px/60px var(--font-sans);
  background: linear-gradient(135deg,
    rgba(0,0,0,.9) 42%, #0081FF 48%, #00BEFF 52%, rgba(0,0,0,.9) 58%);
  background-size: 200% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: ai-shine 4s linear infinite;
}
@keyframes ai-shine {
  0%   { background-position: 100% 0; }
  100% { background-position: -100% 0; }
}

/* ✨ AI 大输入框 */
.ai-input {
  width: 100%; max-width: 720px;
  background: #FFF;
  border: 1px solid rgba(0,0,0,.08);
  border-radius: 16px;
  padding: 20px 24px;
  box-shadow: var(--shadow-lv2);
  transition: border-color .15s, box-shadow .15s;
}
.ai-input:focus-within {
  border-color: #1E6FFF;
  box-shadow: 0 4px 16px rgba(30,111,255,.12);
}
.ai-input textarea {
  width: 100%; min-height: 24px; border: 0; outline: 0; resize: none;
  font: 400 16px/24px var(--font-sans); color: rgba(0,0,0,.9);
}
.ai-input textarea::placeholder { color: rgba(0,0,0,.4); }
.ai-input .send-btn {
  width: 36px; height: 36px; border-radius: 50%;
  background: #1E6FFF; color: #FFF;
  display: grid; place-items: center;
  border: 0; cursor: pointer;
}

/* ✨ AI 快捷动作 Pill */
.ai-action {
  display: inline-flex; align-items: center; gap: 6px;
  height: 36px; padding: 0 16px;
  background: #FFF;
  border: 1px solid rgba(0,0,0,.08);
  border-radius: 999px;
  font: 400 14px/1 var(--font-sans);
  color: rgba(0,0,0,.9);
  cursor: pointer;
  transition: all .15s;
}
.ai-action:hover { border-color: #1E6FFF; color: #1E6FFF; }
```

### 5.11 文档列表行（Doc List Row）

```css
.doc-row {
  display: grid;
  grid-template-columns: 24px 1fr 200px 120px 80px 32px;  /* 图标｜名称｜所有者｜修改时间｜大小｜操作 */
  gap: 16px;
  align-items: center;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}
.doc-row:hover { background: rgba(51,77,102,.06); }
.doc-row .name  { font: 400 14px/20px var(--font-sans); color: rgba(0,0,0,.9); }
.doc-row .meta  { font: 400 13px/18px var(--font-sans); color: rgba(0,0,0,.56); }
```

---

## 6. 图标系统（Iconography）

- **风格**：扁平描线（stroke 1.5px）+ 实色填充（仅文档类型图标），`16/20/24` 三档；
- **品牌 Logo**：T 字几何 + 蓝紫渐变水晶立方体（开物 AI），用于站点头部；
- **图标颜色**：默认 `rgba(0,0,0,.56)`，hover/active 变 `rgba(0,0,0,.9)` 或 `var(--accent-default)`；
- **建议库**：Tabler Icons / Lucide / Material Symbols (Outlined, weight 400, grade 0)；文档类型用「实色描边混合」自绘或选 Material Symbols Filled。

---

## 7. 动效（Motion）

| 类别 | 时长 | 缓动 |
|---|---|---|
| 微交互（hover/focus 颜色） | `150ms` | `ease` |
| 卡片抬升、Tab 切换 | `200ms` | `cubic-bezier(0.4, 0, 0.2, 1)` |
| 模态、抽屉进出 | `300ms` | `cubic-bezier(0.16, 1, 0.3, 1)` |
| 页面级滚动联动 | `400~600ms` | `cubic-bezier(0.22, 1, 0.36, 1)` |
| AI 流光 / 扫描动画 | `4s` | `linear infinite` |

> 原则：**短、稳、不弹跳**（不要 spring / overshoot）。腾讯文档没有"果冻感"。

---

## 8. 五个典型页面的版式蓝图（Page Blueprints）

### 8.1 工作台首页（`docs.qq.com/desktop/` · 登录态）

> ⚠️ **重要**：本节版式来自官方 Figma 设计稿「WEB端/首页」(节点 `2196-417771`, 1440×799)，是**真实登录后的工作台**——不是登录前的欢迎页。Agent 生成"腾讯文档首页"时必须按此结构。

#### 8.1.1 三栏总布局

```
┌────────────────────────────────────────────────────────────────────────────────┐
│ ▲ TopBar 高度≈64px · 通栏 #FFFFFF                                                │
│ [🦋腾讯文档]   [🔍 搜索文档、模板、文库、工具  · 圆角20 高40 #F3F5F7底]                │
│                              [💎 这里最多十二个字字字字]  [▢模板] [▦应用九宫格] [🔔] [☰] [👤]│
├──────────┬──────────────────────────────────────────────┬──────────────────────┤
│          │                                              │ ▲ 工具面板 360px      │
│ ◆ 左侧栏 │  ▲ 中央内容区 padding: 24px 32px              │ #FFFFFF              │
│ 240px    │                                              │                      │
│ #FFFFFF  │  ┌─ 卡片容器 #FFFFFF · 圆角8 ───────────────┐│  工具          [✕]   │
│          │  │ 快速访问                                 ││                      │
│ ┌──────┐ │  │ ┌────────┐┌────────┐┌────────┐┌────────┐ ││  最近使用            │
│ │+ 新建│ │  │ │📁社交体验││📄旅行计划││📊学院预算││📊卫生收集│ ││  ┌──────┐┌──────┐    │
│ └──────┘ │  │ └────────┘└────────┘└────────┘└────────┘ ││  │PDF编辑││PDF→W │    │
│ ┌──────┐ │  │ ┌────────┐┌────────┐┌────────┐┌────────┐ ││  └──────┘└──────┘    │
│ │↑ 上传│ │  │ │📊商业分析││📈春季理…││📄需求分析││ 展开全部▼│ ││                      │
│ └──────┘ │  │ └────────┘└────────┘└────────┘└────────┘ ││  热门工具            │
│          │  └──────────────────────────────────────────┘│  ┌──────┐┌──────┐    │
│ 🏠 首页  │                                              │  │腾讯文库││模板库 │    │
│ ☁  云盘ᐯ│  ▲ Tab 行：最近 ｜ 空间 ｜ 收藏     [筛选▼]   │  └──────┘└──────┘    │
│  └ 与我..│  ━━━━━                                       │  ┌──────┐┌──────┐    │
│  └ ISUX │                                              │  │智能翻译││论文查重│    │
│  └ 设计..│  名称           所有者  位置      最近查看▼ 大小│  └──────┘└──────┘    │
│  └ 工作 │  ─────────────────────────────────────────── │  ┌──────┐┌──────┐    │
│   └ 设…│  📘 在线文档    我      我的云文档  15:38  2.45MB│  │PDF压缩 ││图片转字│    │
│    └ 设│  📄 云Office.docx 我    我的云文档  15:38  3.58MB│  └──────┘└──────┘    │
│      └素│  📄 本地Office.docx[本地]⭐✓ 本机 /Use…/F 15:38│  ┌──────┐┌──────┐    │
│  └ 工作积│  📘 企业官方文档[企业] 德恩         15:38  2.45MB│  │智能扫描││制作证件│    │
│ ✦ 开物AI│  📊 在线表格    我      我的云文档  2023-03-11   │  └──────┘└──────┘    │
│ ▣ 空间  │  📈 在线幻灯片  美雅 SVIP+         2023-03-11    │                      │
│ ───────  │  📊 在线思维导图 天晓             2023-03-11   │  AI工具              │
│ 🗑 回收站│  📘 智能文档    浩哥 SVIP+         06-30 14:31  │  ┌──────┐┌──────┐    │
│          │                                              │  │一键PPT││AI总结 │    │
│ ─────── │                                              │  └──────┘└──────┘    │
│ 已用258MB/│                                             │  ┌──────┐┌──────┐    │
│ 1GB  查看>│                                             │  │AI思维 ││AI流程 │    │
│ ━━━━━░░░│                                              │  └──────┘└──────┘    │
│          │                                              │                      │
│          │                                              │      [更多工具]      │
└──────────┴──────────────────────────────────────────────┴──────────────────────┘
背景：#F3F5F7（仅在三栏之间的缝隙露出 8px）   主内容区无大段灰底，卡片直接贴近
```

#### 8.1.2 顶栏（TopBar · 高 64px · 通栏白）

| 区 | 内容 | 规格 |
|---|---|---|
| 左 | Logo「🦋腾讯文档」 | 蝴蝶图形 24×24 + 字 16/600，间距 8px，距左 24px |
| 中 | 全局搜索框 | **宽 ~720px · 高 40 · 圆角 20 · 底 `#F3F5F7` · 无边框**；左侧 16px 处放 🔍图标 16×16 (`#81868F`)；占位文字 14/regular `#81868F` "搜索文档、模板、文库、工具" |
| 右 | 会员胶囊 | **金色钻石💎 + "这里最多十二个字字字字" 14/600**；金色 `#E59837 → #FFAB00` 渐变描边或文字色；高 32 圆角 16；hover 显示更深金 |
| 右 | 「模板」入口 | 16×16 图标 + 「模板」14/regular，hover `--fill-hover` 圆角 8 |
| 右 | 应用九宫格 | 20×20 网格图标，按钮区 36×36 圆角 8 |
| 右 | 通知 🔔 | 同上；红点徽标 8×8，位于右上 -2/-2 |
| 右 | 菜单 ☰ | 同上 |
| 右 | 头像 | **40×40 圆形**，右下角 8×8 状态点（在线绿 `#00AA5B`）|
| 顶栏底分割线 | 1px solid `rgba(0,0,0,.06)` |

#### 8.1.3 左侧栏（Sidebar · 宽 240px · 白底 · 不浮起）

```
┌────────────────────────────────┐
│  [+ 新建]   主按钮 高44 圆角8   │ ← 大蓝按钮 #1E6FFF, 全宽 padding 12px 16px
│  [↑ 上传]   次按钮 高44 圆角8   │ ← 边框 1px solid rgba(0,0,0,.08), 文字 #454D5A
│  ───────────  分割 16px ────   │
│  🏠 首页              [选中态]  │ ← 高36, 圆角6, 选中: 底 #E8F0FE, 字 #1E6FFF
│  ☁ 云盘            ▶          │ ← 普通: 字 #454D5A 14/regular, hover #F3F5F7
│     📁 与我共享                │ ← 二级: 缩进 24, 字 #454D5A 13/regular
│     📁 ISUX                   │
│     📁 设计资源库              │
│     📁 工作文档      ▼        │
│        📁 设计稿     ▼        │ ← 三级缩进 40
│            📁 设计资源 ▼      │ ← 四级缩进 56
│                📁 素材包1     │
│        📁 工作积累             │
│  ✦ 开物 AI                    │ ← 流光图标（conic AI）
│  ▣ 空间                       │
│  ─────────────                │
│  🗑 回收站                    │
│                                │
│  ─────────────                │
│  已使用 258MB / 1GB  [查看 >]  │ ← 底部固定，DIN Next LT Pro 12/500
│  ━━━━━━░░░░░░░░░░░░░░░░░     │ ← 进度条高 4, 圆角 2, 蓝 #1E6FFF + 灰 #E5E7EB
└────────────────────────────────┘
背景: #FFFFFF; 右侧 1px solid rgba(0,0,0,.06) 与中央分隔
```

**关键细节**：
- "新建"主按钮 · 高 44 · 文字 14/600 白 · 底 `#1E6FFF` · 圆角 8 · `box-shadow: 0 1px 2px rgba(0,0,0,.04)`
- "上传"次按钮 · 高 44 · 文字 14/regular `#454D5A` · 底白 · 边框 1px `rgba(0,0,0,.08)` · 圆角 8
- 导航项行高 36，左 padding 16，图标 18×18，icon-text 间距 10
- **选中态**：背景 `#E8F0FE`（accent-bg-default），文字与图标 `#1E6FFF`，左侧 **3px 高 16 蓝色短指示条 round** 居中（可选）
- 折叠箭头 ▶/▼ 12×12 在最右侧
- 容量进度条：背景 `#E5E7EB`，已用部分蓝 `#1E6FFF`，圆角 2px

#### 8.1.4 中央内容区（无外卡片包裹 · 直接贴页面背景）

中央区不是单个大卡片，而是 **两个独立段落**：

**A) "快速访问" 网格段** —— `8 列网格 / gap 12px / 卡片高 56`

```css
.quick-access-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:12px; }
.quick-access-tile {
  display:flex; align-items:center; gap:8px;
  height:56px; padding:0 16px;
  background:#FFFFFF; border:1px solid rgba(0,0,0,.06); border-radius:8px;
  font:400 14px/20px "PingFang SC"; color:#242424;
  transition:all .15s ease;
}
.quick-access-tile:hover { background:rgba(51,77,102,.04); border-color:rgba(0,0,0,.12); }
.quick-access-tile .ft-icon { width:24px; height:24px; flex-shrink:0; }
.quick-access-tile.expand-all { color:#81868F; justify-content:center; }
```

每瓦片左侧 24×24 文档类型彩色图标（蓝/绿/橙/品红，对应文档/表格/PPT/PDF），右侧文档名 14/regular `#242424`，单行截断。

**B) "最近 / 空间 / 收藏" 列表段**

- Tab 行：高 32，三个 Tab "最近"（active）"空间" "收藏"，**未选 Tab 字 18/500 `#81868F`**，**选中 Tab 字 18/500 `#242424` + 下方 24×3px 圆角 2 蓝条 `#1E6FFF`**；右侧"筛选 ▼" 高 28 圆角 6 边框 1px `rgba(0,0,0,.08)`，文字 12/600 `#454D5A`

- 表格表头：高 40，灰文字 12/600 `#81868F`，列分配「名称(flex)｜所有者(120)｜位置(160)｜最近查看(120,排序)｜大小(80)」，无背景色

- 表格行：高 56，hover `rgba(51,77,102,.06)`；首列含 24×24 文档类型图标（icon-text 间距 12），文档名 14/regular `#242424`；同行内 chip：
  - 🟡「本地」chip · 高 18 圆角 4 边框 1px `#E0E0E0` · 字 10/500 `#81868F`
  - 🟦「企业」chip · 高 18 圆角 4 底 `#EBF2FF` · 字 10/500 `#1E6FFF`
  - ⭐ 收藏星 16×16 `#FFAB00` · ✓ 同步成功 16×16 `#00AA5B`
  - 👑 SVIP+ 徽章 · 高 14 渐变金 `#FFD980 → #FFAB00` · 字 8/700 白
- 行下分割线 1px `rgba(0,0,0,.06)`

#### 8.1.5 右侧"工具"面板（宽 360px · 白底 · 可关闭）

```
┌────────────────────────────────┐
│  工具                    [✕]   │ ← 标题 18/500 #242424, 关闭按钮 16×16
│                                │
│  最近使用                      │ ← 分组标题 14/500 #454D5A, 上方 24
│  ┌────────┐  ┌────────┐        │
│  │ A PDF  │  │ W PDF→W│        │ ← 工具卡片 高 64 圆角 8 边框 1px rgba(0,0,0,.06)
│  │ 编辑   │  │        │        │   左 32×32 应用图标 + 右 14/regular 文字
│  └────────┘  └────────┘        │   2 列 grid gap 12
│                                │
│  热门工具                      │
│  ┌────────┐  ┌────────┐        │
│  │ 腾讯文库│  │ 模板库 │        │
│  └────────┘  └────────┘        │
│  ┌────────┐  ┌────────┐        │
│  │ A 智能翻│  │ 论文查重│        │
│  └────────┘  └────────┘        │
│  ┌────────┐  ┌────────┐        │
│  │ A PDF压│  │ 图片转字│        │
│  └────────┘  └────────┘        │
│  ┌────────┐  ┌────────┐        │
│  │ 智能扫描│  │ 制作证件│        │
│  └────────┘  └────────┘        │
│                                │
│  AI 工具                       │ ← AI 段标题前可加 ✦ 流光小图标
│  ┌────────┐  ┌────────┐        │
│  │ 一键PPT│  │ AI总结 │        │
│  └────────┘  └────────┘        │
│  ┌────────┐  ┌────────┐        │
│  │ AI思维 │  │ AI流程 │        │
│  └────────┘  └────────┘        │
│                                │
│        [ 更多工具 ]             │ ← 文字按钮，14/regular #1E6FFF, 居中
└────────────────────────────────┘
左侧 1px solid rgba(0,0,0,.06)，与中央分隔；padding 24px
```

工具卡片图标使用真实应用色：PDF=红 `#E54C3F`、Word=蓝 `#2A5FE8`、Excel=绿 `#00AA5B`、PPT=橙 `#E49837`、AI类=蓝青渐变。

#### 8.1.6 数据网格说明

| 网格 | 列数 | 间距 | 行高 |
|---|---|---|---|
| 快速访问 (8.1.4 A) | 4 | gap 12 | 56 |
| 工具面板 (8.1.5) | 2 | gap 12 | 64 |
| 文档列表 (8.1.4 B) | 1 (table) | — | 56 |

#### 8.1.7 整体留白与间距

- 三栏间无缝隙（仅各栏间一条 1px `rgba(0,0,0,.06)` 分隔）
- 中央段：左右 padding **32px**，上下 padding **24px**；快速访问段与列表段之间 **40px**
- 左侧栏：水平 padding **16px**，新建/上传按钮上下 padding **16px**，导航段之间 **8px**
- 右侧栏：水平 padding **24px**，分组之间 **24px**，组内卡片 **gap 12**

#### 8.1.8 真实色值速查（来自 Figma 484 个 fill 统计）

| 用途 | 值 | 出现次数 |
|---|---|---|
| 主文字 / 文档名 | `#242424` | 14 |
| 次级深文字 / 导航项 | `#454D5A` | 120 |
| 次次级 / 时间戳 / 占位 | `#81868F` | 32 |
| 顶栏图标灰 | `#CBCDD1` | 82 |
| 列表分隔线 | `#D7D7D7` / `rgba(0,0,0,.06)` | 72 |
| 主品牌蓝 | `#1E6FFF` | 21 |
| 次蓝 / 链接 hover | `#3A7DFF` | 20 |
| 描边浅蓝 (hover) | `#5B97FF` | 13 |
| 选中底 | `#E8F0FE` | — |
| 成功 / Excel 绿 | `#00AA5B` | 22 |
| 警告 / PPT 橙 | `#E49837` | 17 |
| 收藏 / VIP 黄 | `#FFAB00` | 12 |
| 浅绿 chip 底 | `#E0FBF1` / `#A3F0D4` | 14 |

### 8.2 官网首页（`docs.qq.com/home`）

```
┌──────────────────────────────────────────────────────────────┐
│ Header 72px  [Logo]  产品 解决方案 会员 企业 帮助    [下载][登录]│  ← 白底，无阴影
├──────────────────────────────────────────────────────────────┤
│ 公告条 #E8F0FE 36px:  [NEW]腾讯文档企业版已正式推出 点击前往 → ✕│
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  HERO  左侧文案 + 右侧 3D 水晶 T 立方体（蓝/紫渐变）             │
│                                                              │
│  腾讯文档                                                     │
│  让协作更高效，创作更轻松。       ┌─────────────┐              │
│  支持多种文档格式，内容实时同步…   │   水晶立方   │   ← 3D插画   │
│                                  │   (蓝渐变)   │              │
│  [购买咨询(白)] [立即使用(蓝)]    └─────────────┘              │
│                                                              │
├──────────────────────────────────────────────────────────────┤
│                  丰富的 Office 应用                            │
│       了解更多 →                                               │
│                                                              │
│  📘文档  📗表格  📙幻灯片  📕PDF  📋收集表    ← 5列彩色图标卡   │
└──────────────────────────────────────────────────────────────┘
```

### 8.3 文档详情/编辑页（`docs.qq.com/doc/...`）

```
┌──────────────────────────────────────────────────────────────┐
│ TopBar 48px [☰][Logo] 文档名(可改) ⭐  [👥协作头像组] [分享] [⋯]│
├──────────────────────────────────────────────────────────────┤
│ Toolbar 40px  字体 大小 B I U   |  对齐 列表  |  插入  评论 …   │  ← 工具栏，hover 浅灰底
├──────────────┬───────────────────────────────────────────────┤
│ 大纲 (240px) │   编辑画布 #FFF 最大宽 816px (A4) 居中           │
│ - 标题1      │   光标蓝竖线 #1E6FFF                             │
│   - 二级     │   选区高亮 rgba(30,111,255,.16)                  │
│              │                                                │
│   折叠/可隐  │                                                │
└──────────────┴───────────────────────────────────────────────┘
画布外底色 #F3F5F7  右下角浮动："+评论" / 大纲折叠 / 缩放
```

### 8.4 开物 AI / Copilot（`docs.qq.com/copilot`）

```
┌──────────────────────────────────────────────────────────────┐
│ [✨开物AI] [侧栏切换]                          [积分0][菜单][登录]│
├──────┬───────────────────────────────────────────────────────┤
│ 240  │                                                       │
│ 侧栏 │             让创作即刻发生   ← 48px 600 + 蓝青扫光      │
│      │                                                       │
│ +新建│   ┌─────────────────────────────────────────────────┐ │
│ 最近 │   │  帮你处理文档、PPT、表格、数据分析、网页生成…    │ │ ← AI输入16px圆角
│      │   │  + 上传材料                          🎤 [➤蓝]   │ │   有阴影
│      │   └─────────────────────────────────────────────────┘ │
│      │                                                       │
│      │   [📊生成PPT] [✍撰写文档] [📈数据分析] [🌐生成网页] [📋生成表格] │ ← 圆角999 pill
│      │                                                       │
│      │   全部模板  活动策划  项目介绍  学习笔记  求职发展  …    │ ← 下划线 Tab
│      │   ━━━━━━                                              │
│      │   ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                │
│      │   │ 模板1 │ │ 模板2 │ │ 模板3 │ │ 模板4 │  ← 4列网格    │
│      │   │ 16:9  │ │       │ │       │ │       │             │
│      │   │ 缩略图│ │       │ │       │ │       │             │
│      │   ├──────┤ ├──────┤ ├──────┤ ├──────┤                │
│      │   │标题   │ │       │ │       │ │       │             │
│      │   │描述14 │ │       │ │       │ │       │             │
│      │   └──────┘ └──────┘ └──────┘ └──────┘                │
└──────┴───────────────────────────────────────────────────────┘
```

### 8.5 模板商城（`docs.qq.com/mall/index`）

```
┌──────────────────────────────────────────────────────────────┐
│ [📘腾讯文档·模板] [搜索框 AIPPT  长560×40 圆角20] [≡] [登录腾讯文档(蓝)]│
├──────────┬───────────────────────────────────────────────────┤
│ 模板 ᐯ   │ 行业模板    职场办公    校园模板   个人生活   风格类型  │  ← 6列分类映射
│  文档    │ 教育 IT 法律 项目 工作 总结 教学 高考 毕业 简历 婚礼 ……│
│  表格    │ 金融 零售 美食 电商 日报 周报 实习 离校 成绩 个人 理财 ……│
│  幻灯片  │                                                    │
│  收集表  │ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐   │
│  …      │ │ AI PPT       │ │ 求职季      │ │ 快捷记账     │   │ ← 3列 Banner
│         │ │ 一键生成     │ │ 海量简历模板 │ │ 常用便捷记账表│   │   柔和粉/紫/绿底
│ 腾讯文库 │ │ [插画]      │ │ [插画]      │ │ [插画]      │   │   12px 圆角
│  …      │ └─────────────┘ └─────────────┘ └─────────────┘   │
│         │                                                    │
│ 推荐 ᐯ  │ 热门精选  旅行相册 假期通知 旅行攻略 旅行记账         │  ← 二级 Tab
│  …      │ ━━━━━━━                                            │
│         │ ┌────┐┌────┐┌────┐┌────┐┌────┐┌────┐  ← 6列网格    │
│         │ │模板││    ││    ││    ││    ││    │   8px 圆角    │
│         │ │📘  ││    ││    ││    ││    ││    │   底部💎VIP徽标│
│         │ │标题││    ││    ││    ││    ││    │              │
│         │ └────┘└────┘└────┘└────┘└────┘└────┘              │
└──────────┴───────────────────────────────────────────────────┘
```

---

## 9. 必备 CSS 变量（Drop-in `:root`）

> 复制到任何项目的根样式即可。Agent 生成代码时**必须**先注入这一段。

```css
:root {
  /* —— 字体 —— */
  --font-sans: "PingFang SC","Microsoft YaHei","Hiragino Sans GB",system-ui,-apple-system,"Segoe UI",Roboto,"Noto Sans","Source Han Sans SC","Helvetica Neue",Helvetica,Arial,sans-serif,"Apple Color Emoji";
  --font-mono: "SFMono-Regular","Roboto Mono",Menlo,Consolas,monospace;

  /* —— 品牌主色 · 腾讯蓝 —— */
  --accent-default:   #0D61F2;
  --accent-primary:   #1E6FFF;   /* 实际按钮色 */
  --accent-hover:     #347AF4;
  --accent-pressed:   #0D61F2;
  --accent-disabled:  #C2D8FF;
  --accent-bg-default:#E8F0FE;
  --accent-bg-hover:  #D6E4FD;

  /* —— AI 强调色 —— */
  --ai-cyan: #00C8FF;
  --ai-blue: #1E6FFF;
  --ai-deep: #0081FF;
  --ai-conic: conic-gradient(from 90deg,#00C8FF 0deg,#00C8FF 62deg,#1E6FFF 190deg,#1E6FFF 245deg,#00C8FF 360deg);
  --ai-text-grad: linear-gradient(135deg,rgba(0,0,0,.9) 42%,#0081FF 48%,#00BEFF 52%,rgba(0,0,0,.9) 58%);

  /* —— 文字 —— */
  --text-primary:   rgba(0,0,0,.90);
  --text-secondary: rgba(0,0,0,.56);
  --text-tertiary:  rgba(0,0,0,.26);
  --text-link:      #206EF3;
  --text-error:     #ED5050;
  --text-vip:       #E59837;

  /* —— 边框 / 填充 —— */
  --border-weak:   rgba(0,0,0,.08);
  --border-medium: rgba(0,0,0,.12);
  --border-strong: rgba(0,0,0,.16);
  --fill-hover:    rgba(51,77,102,.06);
  --fill-active:   rgba(51,77,102,.08);

  /* —— 背景层级 —— */
  --bg-page: #F3F5F7;
  --bg-card: #FFFFFF;
  --bg-soft: #F7F9FB;
  --bg-mask: rgba(0,0,0,.65);

  /* —— 状态色 —— */
  --error-default:   #EB4141;  --error-bg:   #FDECEC;
  --success-default: #008D4B;  --success-bg: #E6F7EF;
  --warning-default: #EB9D00;  --warning-bg: #FFF6E5;
  --vip-default:     #E6B76C;  --vip-bg:     #FFF8EB;

  /* —— 文档类型色 —— */
  --doc-word: #1E6FFF;  --doc-word-bg: #E8F0FE;
  --doc-xls:  #08B96A;  --doc-xls-bg:  #E6F7EF;
  --doc-ppt:  #FF7A29;  --doc-ppt-bg:  #FFF1E6;
  --doc-pdf:  #E55C5C;  --doc-pdf-bg:  #FDECEC;
  --doc-form: #FFC53D;  --doc-form-bg: #FFF8E1;
  --doc-mind: #9B59FF;  --doc-mind-bg: #F1E8FF;

  /* —— 圆角 —— */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 16px;
  --radius-pill: 999px;

  /* —— 阴影（仅黑色不染色） —— */
  --shadow-lv1: 0 1px 4px 0 rgba(0,0,0,.08);
  --shadow-lv2: 0 3px 8px 1px rgba(0,0,0,.08);
  --shadow-lv3: 0 5px 12px 4px rgba(0,0,0,.08);
  --shadow-lv4: 0 24px 48px 2px rgba(0,0,0,.08), 0 5px 12px 4px rgba(0,0,0,.08);

  /* —— 间距（4 倍数） —— */
  --sp-1: 4px;  --sp-2: 8px;  --sp-3: 12px; --sp-4: 16px;
  --sp-5: 20px; --sp-6: 24px; --sp-8: 32px; --sp-10: 40px;
  --sp-12: 48px;--sp-16: 64px;--sp-20: 80px;

  /* —— 缓动 —— */
  --ease-base: cubic-bezier(.4,0,.2,1);
  --ease-soft: cubic-bezier(.16,1,.3,1);
}

* { box-sizing: border-box; }
html, body {
  margin: 0;
  font-family: var(--font-sans);
  font-size: 14px;
  line-height: 1.5;
  color: var(--text-primary);
  background: var(--bg-page);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
a { color: var(--text-link); text-decoration: none; }
a:hover { text-decoration: underline; text-underline-offset: 3px; }
```

---

## 10. Agent 生成准则（CRITICAL · 必须遵守）

> 以下是 AI 在依据本 tdocdesign.md 生成腾讯文档风格页面时的**强制规则**，违反任何一条就不算"100% 还原"。

### 10.1 必须做（DO）

1. ✅ **首先注入第 9 节的 `:root` 变量**，所有色值/间距/圆角必须用变量，不允许出现硬编码 `#xxx` 在组件层。
2. ✅ **主品牌按钮颜色固定为 `#1E6FFF`**，hover `#347AF4`，禁用 `#C2D8FF`，从不变体。
3. ✅ **圆角默认 4px（按钮/Tab/Tag/输入框）和 8px（卡片/Modal）**；只有 AI 输入框、营销 Banner 用 12/16px。
4. ✅ **阴影只用纯黑 `rgba(0,0,0,.08)`**，从 `--shadow-lv1` 起步，不染色。
5. ✅ **字体 stack 必须以 `"PingFang SC", "Microsoft YaHei"` 开头**，不要直接 `system-ui`；运营/会员标题用 `HarmonyOS Sans SC 700`，数字用 `DIN Next LT Pro`。
6. ✅ **页面背景 `#F3F5F7`，卡片白底 `#FFFFFF`**；Header/Sidebar 也是白底，依靠分隔线/阴影区分层次。
7. ✅ **文字用工作台真实色阶**：标题 `#242424`，正文/导航 `#454D5A`（带蓝意的深灰），次要 `#81868F`，占位 `#CBCDD1`；不要用纯黑 `#000` 或具名色 `#333`。
8. ✅ **Tab 选中态用蓝下划线 + 加粗 600**（实测 24×3px 圆角 2，色 `#1E6FFF`），未选用 `#81868F` + 500。
9. ✅ **列表 hover 用 `rgba(51,77,102,.06)` 带蓝意的灰**（不是 `rgba(0,0,0,.04)` 纯灰），这是腾讯文档的"细节签名"。
10. ✅ **AI 类页面**必须出现至少一处 `--ai-conic` 或 `--ai-text-grad`，且 AI 输入框圆角 16px、阴影 lv2、聚焦时蓝边 + `0 4px 16px rgba(30,111,255,.12)`。
11. ✅ **文档类型用对应彩色徽标**（蓝文档/绿表格/橙 PPT/红 PDF/黄收集表/紫思维导图），不要灰一片。
12. ✅ **顶部公告条**统一用浅蓝 `#E8F0FE` + `[NEW]` 蓝徽标 + 「点击前往 →」蓝链接。
13. ✅ **生成"工作台/首页"必须用三栏结构**：左 240 sidebar（白底） + 中央内容（无外卡片） + 右 360 工具面板（白底，可关闭）；TopBar 高 64，全局搜索为圆角 20、底色 `#F3F5F7` 的胶囊（无边框），右侧并排放金色会员胶囊 + 模板/九宫格/通知/菜单/头像。
14. ✅ **首页中央**先放「快速访问」4 列网格瓦片（高 56），再放 Tab「最近 / 空间 / 收藏」+ 文档表格（行高 56，hover 浅蓝灰），不要堆缩略图大卡片。
15. ✅ **左侧 sidebar** 顶部固定「+ 新建」（主蓝按钮 高 44）+「↑ 上传」（次按钮 高 44），底部固定容量进度条（DIN 字体 + 蓝进度）。

### 10.2 禁止做（DON'T）

1. ❌ **禁止毛玻璃 / glass morphism**（`backdrop-filter`）—— 腾讯文档不用。
2. ❌ **禁止彩色阴影**（`rgba(蓝, .2)` 之类）—— 仅黑阴影。
3. ❌ **禁止大圆角**（≥20px）—— 除胶囊 999px 外，最大 16px。
4. ❌ **禁止霓虹/赛博**配色 —— 紫粉荧光绝不出现在主流程。
5. ❌ **禁止"果冻 / spring overshoot"动画** —— 全部用 `ease` 或 `cubic-bezier(.4,0,.2,1)`。
6. ❌ **禁止超粗字重**（≥700）—— 标题最重 600（运营营销 banner 例外可用 HarmonyOS 700）。
7. ❌ **禁止把所有按钮做成 primary 蓝** —— 一个区域只准一个主按钮，其他用 default（白）或 text。
8. ❌ **禁止用 emoji 替代图标**（除非是文档类型示意）。
9. ❌ **禁止把工作台首页画成"登录前欢迎页"或"营销 hero"** —— 真实首页是三栏 + 数据列表，不要 3D 立方体/插画区域。
10. ❌ **禁止给左右栏加阴影或圆角** —— sidebar/工具面板都是齐边白底，仅 1px `rgba(0,0,0,.06)` 分隔。

### 10.3 适配目标需求的弹性（FLEXIBILITY）

虽然规则严格，但这套设计可以**无缝承载多种业务诉求**：

| 业务场景 | 调整方法 | 仍保持腾讯文档气质 |
|---|---|---|
| 数据看板 / Dashboard | 使用 `--bg-card` 卡片 + `tabular-nums` 数字 + 文档类型色作为指标色编码 | ✅ |
| AI 对话产品 | 启用第 5.10 节 AI 组件 + `--ai-conic` + 16px 圆角输入 | ✅ |
| B 端工作台 | 三栏布局 + 240px 侧栏 + 56px 顶栏 + 列表 hover 蓝灰 | ✅ |
| 营销官网 / Hero | 大字 48~60px + 立体水晶 3D 插画 + 公告条 + 双按钮（白+蓝） | ✅ |
| 移动端 H5 | 自动换为单列 + 卡片 12px 圆角 + 安全区 padding；色板不变 | ✅ |
| 暗色模式 | 启用第 1.5 节 `[data-theme="dark"]` 覆盖；主蓝 `#0D61F2` 不变 | ✅ |

---

## 11. 一段最小可运行示例（Reference Implementation）

将以下代码保存为 `.html` 即可看到典型的"腾讯文档式"页面：顶栏 + AI 输入区 + 文档卡片网格。Agent 应以此为骨架，在用户具体需求上扩展。

```html
<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8"/>
<title>腾讯文档风格 · Demo</title>
<style>
  /* 把第 9 节 :root 变量整段贴在这里 ... */
  :root{
    --font-sans:"PingFang SC","Microsoft YaHei",system-ui,sans-serif;
    --accent-primary:#1E6FFF; --accent-hover:#347AF4;
    --accent-bg-default:#E8F0FE; --text-primary:rgba(0,0,0,.9);
    --text-secondary:rgba(0,0,0,.56); --bg-page:#F3F5F7; --bg-card:#FFF;
    --border-weak:rgba(0,0,0,.08); --fill-hover:rgba(51,77,102,.06);
    --shadow-lv1:0 1px 4px rgba(0,0,0,.08); --shadow-lv2:0 3px 8px 1px rgba(0,0,0,.08);
    --radius-sm:4px; --radius-md:8px; --radius-xl:16px;
    --ai-text-grad:linear-gradient(135deg,rgba(0,0,0,.9) 42%,#0081FF 48%,#00BEFF 52%,rgba(0,0,0,.9) 58%);
  }
  *{box-sizing:border-box} body{margin:0;font:14px/1.5 var(--font-sans);background:var(--bg-page);color:var(--text-primary)}
  .topbar{display:flex;align-items:center;height:56px;padding:0 24px;background:#FFF;border-bottom:1px solid var(--border-weak);gap:24px}
  .logo{display:flex;align-items:center;gap:8px;font-weight:600;font-size:16px}
  .logo .t{width:28px;height:28px;border-radius:6px;background:linear-gradient(135deg,#1E6FFF,#0D61F2);color:#FFF;display:grid;place-items:center;font-weight:700}
  .spacer{flex:1}
  .btn{height:32px;padding:0 16px;border-radius:var(--radius-sm);border:1px solid transparent;font:500 14px/1 var(--font-sans);cursor:pointer;transition:.15s}
  .btn-primary{background:var(--accent-primary);color:#FFF}
  .btn-primary:hover{background:var(--accent-hover)}
  .btn-default{background:#FFF;color:var(--text-primary);border-color:rgba(0,0,0,.12)}
  .btn-default:hover{background:var(--fill-hover)}

  .main{max-width:1080px;margin:0 auto;padding:64px 24px}
  .ai-headline{font:600 48px/60px var(--font-sans);text-align:center;margin:0 0 32px;
    background:var(--ai-text-grad);background-size:200% 100%;-webkit-background-clip:text;background-clip:text;color:transparent;animation:shine 4s linear infinite}
  @keyframes shine{0%{background-position:100% 0}100%{background-position:-100% 0}}
  .ai-input{background:#FFF;border:1px solid var(--border-weak);border-radius:var(--radius-xl);padding:20px 24px;box-shadow:var(--shadow-lv2);max-width:720px;margin:0 auto 24px;display:flex;align-items:flex-end;gap:16px}
  .ai-input textarea{flex:1;border:0;outline:0;resize:none;font:16px/24px var(--font-sans);min-height:48px}
  .send{width:36px;height:36px;border:0;border-radius:50%;background:var(--accent-primary);color:#FFF;cursor:pointer}
  .pills{display:flex;flex-wrap:wrap;justify-content:center;gap:12px;max-width:720px;margin:0 auto 48px}
  .pill{height:36px;padding:0 16px;background:#FFF;border:1px solid var(--border-weak);border-radius:999px;display:inline-flex;align-items:center;gap:6px;cursor:pointer;transition:.15s}
  .pill:hover{border-color:var(--accent-primary);color:var(--accent-primary)}

  .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:16px}
  .doc-card{background:#FFF;border:1px solid var(--border-weak);border-radius:var(--radius-md);overflow:hidden;cursor:pointer;transition:.2s}
  .doc-card:hover{box-shadow:var(--shadow-lv1);border-color:rgba(0,0,0,.16);transform:translateY(-1px)}
  .doc-card .thumb{aspect-ratio:4/3;background:linear-gradient(135deg,#E8F0FE,#F3F5F7);display:grid;place-items:center;color:#1E6FFF;font-size:32px}
  .doc-card .meta{padding:12px 14px}
  .doc-card .name{font:500 14px/20px var(--font-sans);margin:0 0 4px;color:var(--text-primary);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .doc-card .info{font:12px/18px var(--font-sans);color:var(--text-secondary)}
  .tag{display:inline-block;height:18px;padding:0 6px;font:500 11px/18px var(--font-sans);border-radius:4px;background:var(--accent-bg-default);color:var(--accent-primary);margin-right:4px}
</style>
</head>
<body>

<div class="topbar">
  <div class="logo"><div class="t">T</div>腾讯文档</div>
  <div class="spacer"></div>
  <button class="btn btn-default">下载</button>
  <button class="btn btn-primary">登录</button>
</div>

<main class="main">
  <h1 class="ai-headline">让创作即刻发生</h1>

  <div class="ai-input">
    <textarea placeholder="帮你处理文档、PPT、表格、数据分析、网页生成等工作任务"></textarea>
    <button class="send">➤</button>
  </div>

  <div class="pills">
    <span class="pill">📊 生成PPT</span>
    <span class="pill">✍ 撰写文档</span>
    <span class="pill">📈 数据分析</span>
    <span class="pill">🌐 生成网页</span>
    <span class="pill">📋 生成表格</span>
  </div>

  <div class="grid">
    <div class="doc-card"><div class="thumb">📘</div><div class="meta">
      <p class="name">项目周报模板</p><p class="info"><span class="tag">NEW</span>我 · 3小时前</p>
    </div></div>
    <div class="doc-card"><div class="thumb" style="background:linear-gradient(135deg,#E6F7EF,#F3F5F7);color:#08B96A">📊</div><div class="meta">
      <p class="name">2026 财务汇总.xlsx</p><p class="info">李四 · 昨天</p>
    </div></div>
    <div class="doc-card"><div class="thumb" style="background:linear-gradient(135deg,#FFF1E6,#F3F5F7);color:#FF7A29">📽</div><div class="meta">
      <p class="name">产品发布会PPT</p><p class="info">王五 · 2天前</p>
    </div></div>
    <div class="doc-card"><div class="thumb" style="background:linear-gradient(135deg,#FDECEC,#F3F5F7);color:#E55C5C">📕</div><div class="meta">
      <p class="name">合同协议.pdf</p><p class="info">张三 · 1周前</p>
    </div></div>
  </div>
</main>

</body>
</html>
```

---

## 12. 设计 token JSON（Design Tokens · 给设计工具/代码生成器消费）

```json
{
  "color": {
    "accent":   { "default": "#0D61F2", "primary": "#1E6FFF", "hover": "#347AF4", "pressed": "#0D61F2", "disabled": "#C2D8FF", "bg": "#E8F0FE", "secondary": "#3A7DFF", "outline-hover": "#5B97FF" },
    "ai":       { "cyan": "#00C8FF", "blue": "#1E6FFF", "deep": "#0081FF" },
    "text":     { "title": "#242424", "body": "#454D5A", "muted": "#81868F", "placeholder": "#CBCDD1", "primary": "rgba(0,0,0,.9)", "secondary": "rgba(0,0,0,.56)", "tertiary": "rgba(0,0,0,.26)", "link": "#206EF3", "error": "#ED5050", "vip": "#E59837" },
    "bg":       { "page": "#F3F5F7", "card": "#FFFFFF", "soft": "#F7F9FB", "search": "#F3F5F7", "mask": "rgba(0,0,0,.65)" },
    "border":   { "weak": "rgba(0,0,0,.06)", "default": "rgba(0,0,0,.08)", "medium": "rgba(0,0,0,.12)", "strong": "rgba(0,0,0,.16)", "divider": "#D7D7D7" },
    "fill":     { "hover": "rgba(51,77,102,.06)", "active": "rgba(51,77,102,.08)" },
    "status":   { "error": "#EB4141", "success": "#00AA5B", "warning": "#E49837", "vip-yellow": "#FFAB00", "vip-gold": "#E6B76C" },
    "doc":      { "word":"#1E6FFF","xls":"#00AA5B","ppt":"#E49837","pdf":"#E55C5C","form":"#FFC53D","mind":"#9B59FF" },
    "chip":     { "green-bg":"#E0FBF1","green-fg":"#00AA5B","light-green":"#A3F0D4","blue-bg":"#EBF2FF","blue-fg":"#1E6FFF" }
  },
  "font": {
    "family-sans":    "\"PingFang SC\",\"Microsoft YaHei\",system-ui,sans-serif",
    "family-display": "\"HarmonyOS Sans SC\",\"PingFang SC\",sans-serif",
    "family-number":  "\"DIN Next LT Pro\",\"DIN Alternate\",sans-serif",
    "size":   { "display":48, "h1":32, "h2":24, "h3":20, "h4":18, "lg":16, "base":14, "sm":12, "xs":10 },
    "weight": { "regular":400, "medium":500, "semibold":600 }
  },
  "radius":  { "sm":4, "md":8, "lg":12, "xl":16, "pill":999 },
  "shadow":  {
    "lv1": "0 1px 4px 0 rgba(0,0,0,.08)",
    "lv2": "0 3px 8px 1px rgba(0,0,0,.08)",
    "lv3": "0 5px 12px 4px rgba(0,0,0,.08)",
    "lv4": "0 24px 48px 2px rgba(0,0,0,.08), 0 5px 12px 4px rgba(0,0,0,.08)"
  },
  "spacing": [4,8,12,16,20,24,32,40,48,64,80,96],
  "motion":  { "fast":"150ms ease", "base":"200ms cubic-bezier(.4,0,.2,1)", "slow":"300ms cubic-bezier(.16,1,.3,1)" }
}
```

---

## 附：典型截图清单

实地抓取的视觉参考已留存于工作区，可作为"像素对比"参考：

- `/desktop-figma.png` ⭐ —— **工作台首页（登录态 · 来自官方 Figma 设计稿，权威源）** · 2880×1598
- `/desktop.png` —— 工作台（登录前 / 欢迎页，仅供环境对照）
- `/official-home.png` —— 官网首页 Hero
- `/copilot.png` —— 开物 AI（Copilot）
- `/mall.png` —— 模板商城

> 当生成"工作台 / 首页 / 文件列表 / 工具中心"类页面时，**必须以 `desktop-figma.png` 为视觉真值**，不要参考 `desktop.png`（那是未登录欢迎页）。

---

**版本**：v1.1 · 2026-04-28
**适用**：WorkBuddy / CodeBuddy / 任何支持 tdocdesign.md 的 Agent 产品
**还原度承诺**：严格遵守第 10 节准则即可达到 100% 视觉一致性。
**变更日志**：
- v1.1：基于官方 Figma 「WEB端/首页」设计稿（节点 2196-417771）重写第 8.1 节工作台首页版式（三栏 + 真实组件），补充工作台真实文字色阶 `#242424/#454D5A/#81868F`，新增 HarmonyOS Sans SC / DIN Next LT Pro 字体栈，第 10 节加入 3 条工作台首页强制规则。
- v1.0：首版，基于 docs.qq.com 5 个典型页面 DOM 与 dui Design System 抓取。
