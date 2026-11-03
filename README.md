# GitHub Markdown 浏览指南 · 系列总览与导航

> 调研日期：2026-11-03 ｜ 本篇为系列总览与导航：7 篇文档的地图、快速决策表与全景速览
> 一句话结论：**从「按 `.` 打开 github.dev」到「自建文档站」，7 篇覆盖官方方案、第三方站点、AI 工具、扩展、文档站、横向决策与盲区补全——先看本页选路径，再进对应分篇查细节。**

---

## 1. 系列文档列表

| 篇 | 文档 | 一句话内容 |
|---|---|---|
| 01 | 01-官方自带方案-github-dev.md | github.dev / vscode.dev / GitHub 原生渲染：免费、支持私仓、分栏预览最佳 |
| 02 | 02-第三方在线站点-github1s等.md | github1s / gitprint.me / raw.githack / Sourcegraph / grep.app：改 URL 即用 |
| 03 | 03-AI仓库阅读工具-DeepWiki等.md | DeepWiki / Zread / DeepWiki-Open / GitIngest / Repomix：AI 帮你读仓库 |
| 04 | 04-浏览器扩展-Gitako等.md | Gitako / Octotree / Refined GitHub / Markdown Viewer：补手感与工作流 |
| 05 | 05-文档站方案-docsify与静态生成器.md | docsify / MkDocs Material / VitePress / mdBook / GitBook / Obsidian：仓库变文档站 |
| 06 | 06-横向对比与场景决策.md | 全景对比总表 + 决策树 + 组合方案 A–D + 十条经验法则（汇总篇） |
| 07 | 07-盲区补全.md | 网络、安全、私仓、移动端、离线、搜索、成本、失效风险八大盲区 |

## 2. 快速决策表

| 你的需求 | 直接用 |
|---|---|
| 只看一个 README | GitHub 网页原生渲染（免登录） |
| 通读整仓 md、校对 | github.dev（按 `.`，`Ctrl+K V` 分栏预览） |
| 读懂陌生大代码库 | DeepWiki（可翻墙）/ Zread（国内直连），结论回源核对 |
| 更快翻文件 / 找文件 | 官方文件树已内置；要手感装 Gitako |
| md 仓库变长期文档站 | docsify（快）/ MkDocs Material + jieba（中文搜索最强） |
| 读私有仓库 | github.dev（免费登录即用）；AI 理解上 Devin 订阅或自部署 DeepWiki-Open |
| 把仓库喂给 LLM | Repomix（本地）/ GitIngest（在线） |
| 打印 / 归档 | gitprint.me 整仓 PDF / 本地 md-to-pdf |
| 国内网络不佳 | github1s 改 URL + Zread + 自建站托管 Cloudflare Pages |
| 手机 / 平板阅读 | iPad+键盘用 github.dev；Android 用 Firefox+扩展；公式图表回浏览器打开 |

## 3. 全景速览

| 类别 | 代表工具 | 一句话定位 |
|---|---|---|
| 官方自带 | github.dev / vscode.dev / 原生渲染 | 免安装的默认起点，登录即用 |
| 第三方在线站点 | github1s / gitprint.me / raw.githack / grep.app | 改 URL 即用，警惕代理记录与停运 |
| AI 仓库阅读 | DeepWiki / Zread / DeepWiki-Open | 生成 wiki + 问答，结论需回源核对 |
| 浏览器扩展 | Gitako / Refined GitHub / Octotree | 补速度手感与工作流，权限越窄越好 |
| 文档站方案 | docsify / MkDocs Material / VitePress | 仓库变网站，中文搜索 MkDocs 最强 |
| 本地 / 自托管 | Obsidian + obsidian-git / Repomix / DeepWiki-Open | 隐私最好，离线可用，数据不出本机 |

## 4. 系列特点

- **多信源交叉验证**：每篇 12–25 个信源，全系列累计 90+，官方文档与社区实测互相印证；
- **面向国内用户**：每篇标注国内可达性，并给出 Zread、github1s、Cloudflare Pages 托管等替代路线；
- **补全盲区**：私有仓库、安全与隐私、移动端、离线、全文搜索、成本、工具失效风险——单篇讲不到的横向问题集中在 06（决策）与 07（盲区）。

## 5. 免责与时点声明

- 全部信息时点为 **2026-11-03**：星数、价格、维护状态、国内可达性随时会变，使用前请自测；
- AI 工具输出可能存在幻觉，重要结论请回源核对；
- 本系列为独立调研，与文中提及的任何产品无利益关联。

---

**系列完 · 2026-11-03**
