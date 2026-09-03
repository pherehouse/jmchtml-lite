---
name: "jmchtml-lite"
description: "创建简洁、全屏无外框的 JMC 书页式 HTML 演示。适用于多页汇报、方案展示或 HTML 幻灯片；页面始终铺满浏览器视口。"
---

# JMC HTML 演示：精简版

用于生成可直接打开的单文件 HTML 演示。目标是做出有封面感、翻页顺畅、视觉统一的 JMC 风格汇报页；画面必须直接铺满浏览器视口，不出现外层留白、灰色画布、圆角容器或投影边框。

## 输出边界

- 交付一个独立 HTML 文件；除明确需要的图片外，不引入构建流程或第三方运行时。
- “全屏”指演示画面铺满浏览器可视区域；浏览器本身的标签栏、地址栏不属于页面可控制范围。`F` 或 Logo 点击触发的 Fullscreen API 仅作为可选增强，不能替代默认全屏版式。

## 全屏画布（强制）

`.deck` 是整个浏览器画面的根容器，不是居中的白色卡片。每次生成都必须先写入以下骨架，再添加具体版式：

```css
html, body{
  width:100%; height:100%; margin:0; overflow:hidden;
}
body{
  background:var(--bg); /* 不显示 deck 外部画布 */
}
.deck{
  position:fixed; inset:0;
  width:100vw; height:100vh;
  overflow:hidden;
  border:0; border-radius:0; box-shadow:none;
  background:var(--bg);
}
.slide{
  position:absolute; inset:0;
  width:100%; height:100%;
}
```

- 禁止对 `html`、`body` 或 `.deck` 使用 `padding`、`max-width`、`max-height`、`margin:auto`、圆角、阴影、缩放或任何会露出外部背景的定位。
- 禁止使用 `.stage`、`.frame`、`.app` 等包住 `.deck` 后再设置居中、留边、圆角或投影的外层舞台。确有包装元素时，它只能是全视口且透明的交互层。
- `--radius` 和 `--shadow` 只可用于正文中的卡片、面板、数据块等内容组件；不得用于 `.deck`、`.slide` 或任何演示画面外壳。
- 不要用 `@media` 查询将桌面端 `.deck` 缩为带边框的预览卡片。窄屏时仍以可视区域为画布，可缩放内容或调整排版。
- 完成前静态检查上述四个选择器的 CSS：页面在 1920×1080 等常见桌面视口下应无画面外露区域、无外轮廓圆角、无容器投影。

## 页面结构

使用 `.deck` 容器和多个 `.slide` 页面。

- 首页为 `.cover`：标题、副标题、关键标签。
- 中间页面承载正文；一页只讲一个主题，优先用结论、数字、流程、卡片或对比表达。
- 末页为 `.cover` 风格的结束页：一句收尾文案、落款或行动号召，不放业务正文。

除非用户明确指定其他形式，使用“封面 → 内容页 → 结束页”的顺序。

## 视觉语言

在 `:root` 集中定义颜色、内容组件的圆角和阴影。默认采用白底、蓝青强调色和轻量玻璃质感：

```css
:root{
  --bg:#fff; --bg-soft:#f7f7f8; --surface:#fff;
  --text-1:#111216; --text-2:#55596a; --text-3:#8a8f9e;
  --accent:#0A79C3; --accent-2:#036AA2; --accent-3:#0FA3B1;
  --grad:linear-gradient(135deg,#0A79C3,#036AA2 55%,#0FA3B1);
  --radius:18px; --radius-sm:12px; /* 仅内容组件 */
  --shadow:0 10px 30px rgba(18,24,40,.08),0 2px 6px rgba(18,24,40,.04); /* 仅内容组件 */
  --ease:cubic-bezier(.4,0,.2,1);
}
```

- 标题紧凑、有层级；渐变只用于封面或关键结论。
- 内容优先留白与网格，不堆叠边框、装饰或大段文字。
- 卡片使用统一圆角和轻阴影；状态色仅用于需要强调的结论。

## JMC 标识

默认在 `.deck::after` 固定右上角 Logo。使用本技能 `assets/jmc-ford-logo.png` 的透明 PNG Base64 编码，直接内嵌在 HTML/CSS 中；不要使用相对路径、外链或非透明底图。Logo 只作冠名，不参与内容布局。

```css
.deck::after{
  content:''; position:absolute; top:34px; right:44px; z-index:15;
  width:150px; aspect-ratio:288/52;
  background:url('data:image/png;base64,<由 assets/jmc-ford-logo.png 编码得到的完整 Base64>') no-repeat center/contain;
  pointer-events:none;
}
```

如需提供浏览器 Fullscreen API 增强，可在 Logo 上方放一个透明按钮，调用同一个 `fullscreen()` 函数切换全屏；不要添加可见的额外按钮。无论是否提供该交互，页面默认版式都必须全屏无外框。

## 基础导航

为多页演示实现以下轻量交互：

- 顶部目录：仅索引中间内容页，当前页高亮。
- 底部圆点、页码和 3px 进度条。
- 键盘支持 `←`、`→`、空格、`Home`、`End`；点击左右区域可翻页，并排除按钮和导航区域。
- `F` 和 Logo 点击共用全屏切换函数。

封面和结束页隐藏顶部目录；底部导航保持克制，不遮挡内容。

## 实施原则

- 先按用户提供的内容和页数组织叙事，再决定图表、卡片和版式。
- 用原生 HTML、CSS、JavaScript 实现，不为未提出的兼容场景增加分支。
- 用户要求静态页、滚动页或特定交互时，以其要求为准，可省略不需要的导航。
