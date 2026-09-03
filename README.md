# jmchtml-lite

一个用于生成 JMC 风格 HTML 演示文稿的 Agent Skill。

它把内容组织成可直接打开的单文件 HTML 幻灯片，提供封面、内容页、结束页、目录、页码、翻页和全屏交互。默认画布铺满浏览器视口，不生成居中卡片式外框、周边留白、圆角容器或投影边框。

适合用于：

- 多页汇报、方案展示、项目复盘
- 产品介绍、研究结论、流程和对比页
- 不依赖构建工具、打开即用的 HTML 幻灯片

## 快速安装

### 方式一：使用 `npx skills`（推荐）

无需全局安装 CLI，直接从 GitHub 安装：

```bash
npx skills add pherehouse/jmchtml-lite
```

安装到当前用户的 Agent 环境：

```bash
npx skills add pherehouse/jmchtml-lite --skill jmchtml-lite -g -y
```

也可以使用完整仓库地址：

```bash
npx skills add https://github.com/pherehouse/jmchtml-lite --skill jmchtml-lite -g -y
```

`-g` 表示安装到用户级目录；不加 `-g` 时通常安装到当前项目。该 CLI 会把 Skill 配置到它检测到的 Agent（如 Codex、Claude Code、Cursor、GitHub Copilot、Cline 等）。

### 方式二：让 Agent 自动安装

把下面这段提示词发给你的 Agent：

```text
请从 GitHub 安装并启用这个 Agent Skill：
https://github.com/pherehouse/jmchtml-lite

Skill 名称：jmchtml-lite
安装后请读取其中的 SKILL.md，并在生成 JMC HTML 演示时遵守它的全部要求，尤其是：
1. 页面默认铺满浏览器视口；
2. 不要生成外层留白、灰色画布、圆角容器、投影边框或居中卡片式舞台；
3. 圆角和阴影只用于内容卡片，不用于演示画布外壳；
4. 输出可直接打开的单文件 HTML。
安装完成后告诉我 Skill 的安装位置。
```

### 方式三：手动安装

如果你的 Agent 没有统一的 Skills CLI，可以把仓库中的 `SKILL.md` 放进它的用户级 Skill 目录，并保留目录名 `jmchtml-lite`：

```text
<agent-skills-directory>/jmchtml-lite/SKILL.md
```

以 Codex 为例：

```text
~/.codex/skills/jmchtml-lite/SKILL.md
```

放置完成后，重启或刷新 Agent，让它重新扫描 Skills 目录。

## 使用

安装后，直接告诉 Agent 你的内容和页数即可，例如：

```text
使用 jmchtml-lite，把下面的内容做成 8 页 JMC 风格 HTML 汇报。
要求：画面全屏、单文件输出、支持键盘翻页，并保持内容页信息清晰。
```

如果 Agent 没有自动触发，可以明确指定：

```text
请调用 jmchtml-lite Skill 生成这份 HTML 演示。
```

## 更新

通过 `npx skills` 安装的 Skill，可以使用以下命令检查并更新：

```bash
npx skills check
npx skills update jmchtml-lite
```

手动安装则重新下载或复制最新的 `SKILL.md` 即可。

## 文件结构

```text
jmchtml-lite/
├── README.md   # 安装和使用说明
└── SKILL.md    # Agent 执行规则
```

## 相关链接

- [GitHub 仓库](https://github.com/pherehouse/jmchtml-lite)
- [Skills CLI 文档](https://www.skills.sh/docs/cli)
- [Skills 文档](https://www.skills.sh/docs)
