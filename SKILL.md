---
name: "jmchtml-lite"
description: "创建简洁的 JMC 书页式 HTML 演示。适用于多页汇报、方案展示或 HTML 幻灯片；不处理 Obsidian 兼容和浏览器验收。"
---

# JMC HTML 演示：精简版

用于生成可直接打开的单文件 HTML 演示。目标是做出有封面感、翻页顺畅、视觉统一的 JMC 风格汇报页；不加入与交付无关的兼容层或验收流程。

## 输出边界

- 交付一个独立 HTML 文件；除明确需要的图片外，不引入构建流程或第三方运行时。
- 不适配 Obsidian、`iframe.srcdoc` 或 URL hash。
- 不调用 Puppeteer、浏览器控制工具或截图工具做预览/验收。
- 不附加检查清单；完成内容和交互实现后即可交付。

## 页面结构

使用 `.deck` 容器和多个 `.slide` 页面。

- 首页为 `.cover`：标题、副标题、关键标签。
- 中间页面承载正文；一页只讲一个主题，优先用结论、数字、流程、卡片或对比表达。
- 末页为 `.cover` 风格的结束页：一句收尾文案、落款或行动号召，不放业务正文。

除非用户明确指定其他形式，使用“封面 → 内容页 → 结束页”的顺序。

## 视觉语言

在 `:root` 集中定义颜色、圆角和阴影。默认采用白底、蓝青强调色和轻量玻璃质感：

```css
:root{
  --bg:#fff; --bg-soft:#f7f7f8; --surface:#fff;
  --text-1:#111216; --text-2:#55596a; --text-3:#8a8f9e;
  --accent:#0A79C3; --accent-2:#036AA2; --accent-3:#0FA3B1;
  --grad:linear-gradient(135deg,#0A79C3,#036AA2 55%,#0FA3B1);
  --radius:18px; --radius-sm:12px;
  --shadow:0 10px 30px rgba(18,24,40,.08),0 2px 6px rgba(18,24,40,.04);
  --ease:cubic-bezier(.4,0,.2,1);
}
```

- 标题紧凑、有层级；渐变只用于封面或关键结论。
- 内容优先留白与网格，不堆叠边框、装饰或大段文字。
- 卡片使用统一圆角和轻阴影；状态色仅用于需要强调的结论。

## JMC 标识

默认在 `.deck::after` 固定右上角 Logo，使用本技能的离线资产：`assets/jmc-ford-logo.png`。Logo 只作冠名，不参与内容布局。

```css
.deck::after{
  content:''; position:absolute; top:34px; right:44px; z-index:15;
  width:150px; aspect-ratio:288/52;
  background:url('assets/jmc-ford-logo.png') no-repeat center/contain;
  pointer-events:none;
}
```

如页面需要全屏，在 Logo 上方放一个透明按钮，调用同一个 `fullscreen()` 函数切换全屏；不要添加可见的额外按钮。

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
