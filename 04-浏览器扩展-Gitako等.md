# 04 · 浏览器扩展：Gitako、Octotree、Refined GitHub、Markdown Viewer

> 调研日期：2026-11-03 ｜ 信源：12+ 个独立来源（仓库页/商店页/官方 changelog，见文末）
> 一句话结论：**GitHub 2022-2023 年已官方内置文件树侧边栏和 markdown 目录（Outline），"没树用"的时代结束了。扩展的剩余价值在速度手感、跨站支持、工作流增强：免费开源选 Gitako + Refined GitHub；渲染 raw md 用 Markdown Viewer（simov）但务必收紧权限；Octotree 核心功能已进付费墙（Pro $35/年）。**

---

## 0. 先回答一个前置问题：GitHub 官方已内置了什么？

| 能力 | 官方化时间 | 证据 |
|---|---|---|
| 仓库**文件树侧边栏** | 2022-08-17 GA | [github/roadmap#553](https://github.com/github/roadmap/issues/553) |
| **新代码视图 + 符号面板**（tree-sitter 驱动、符号跳转、粘性文件头、模糊文件查找） | 2023-05-08 GA | [官方 changelog](https://github.blog/changelog/2023-05-08-the-new-code-search-and-code-view-is-now-generally-available) |
| markdown 文件**目录 Outline** | 2021-04-13 | [官方 changelog](https://github.blog/changelog/2021-04-13-table-of-contents-support-in-markdown-files) |

**结论**：单纯"想要文件树/想要 md 目录"，2023 年后已不必装扩展。GitHub 还大量吸收了 Refined GitHub 的特性（该项目维护一份"被官方化而移除的特性"清单）。扩展的剩余价值：**速度手感**（Gitako 即时搜索跳转仍快于官方树逐级点开）、**跨站**（Gitea/Gitee/GHES）、**成体系工作流**（Octotree 书签与 PR 审查、Refined GitHub 数百项微交互）、**官方没有的定制**（图标主题、折叠、路径复制）。

---

## 1. Gitako（EnixCoda/Gitako）— 免费开源首选

### 1.1 功能
- 📂 文件树侧边栏：覆盖仓库页、commit 页、PR 页；
- 🔎 即时文件搜索与跳转（输入即过滤、免刷新导航）；
- 🔒 支持私有仓库（可选填 PAT）；
- 🧩 支持 GitHub Enterprise Server、**Gitea、Gitee** 等同类站点；
- ⌨️ 树内键盘导航（方向键 + 回车）；📋 复制文件路径/片段/内容；
- 🎨 丰富文件图标、跟随官方主题；🗂 git submodule；📏 源码折叠。

### 1.2 关键数据
- **约 2.6k stars**、Chrome 商店评分 4.9；**v3.15.4（2026-01-18 发布）**，累计 81 个 release、1500+ 提交——**非常活跃**；
- 权限面窄：仅 `*://*.github.com/*` 与自家官网域（最佳实践）；
- 隐私声明："NO BS POLICY"——不收集/分享数据；PAT 存扩展 storage（非 localStorage），仅用于 GitHub 认证。

### 1.3 安装
- Chrome：[Gitako - GitHub file tree](https://chromewebstore.google.com/detail/gitako-github-file-tree/giljefjcheohhamkjphiebfjnlphnokk)
- Firefox：[AMO](https://addons.mozilla.org/firefox/addon/gitako-github-file-tree/)
- Edge：官方建议直接装 Chrome 商店版（Edge 商店审核慢，更新滞后数周）
- Safari：仅仓库内实验性目录，**无正式发行版**

### 1.4 优缺点
优点：开源免费（MIT）、轻量（~1.86 MiB）、性能好（超大仓库也流畅）、权限克制、持续更新、支持 Gitee/Gitea。
缺点：偶发 UI bug、搜索排序一般、GitHub 改版时 UI 偶尔不稳、无 Octotree 式书签。

---

## 2. Octotree — 老牌但已商业分化

### 2.1 免费/付费现状
- 免费版明显缩水：仅剩代码树、树内搜索、有限书签、基础私仓支持；
- **Pro（约 $2.92/月，年付 $35）**：文件图标主题、PR 快速导航与代码审查、无限书签、侧栏停靠、多账号、GHES；
- [ovity/octotree](https://github.com/ovity/octotree) 开源仓库仅保留 v2 旧源码（README 自述 "old source code of a very limited version"，~1.7 万★ 实质冻结），**v3 起闭源商业化**；免费功能缩水曾引发用户不满（issue #700），直接催生 Gitako/Octofree。

### 2.2 数据与维护
- 商店版本 v8.3.0（2026-05-28 更新），商业持续维护；用户数口径不一（历史称 Chrome 商店 200 万+，2025 年评测约 30 万）——商业化后免费基盘收缩；
- 数据披露收集"个人身份信息 + 认证信息"，权限面较 Gitako 广。

### 2.3 与 Gitako 对比

| 维度 | Gitako | Octotree |
|---|---|---|
| 开源 | MIT 完全开源 | v2 旧码开源，v3+ 闭源 |
| 价格 | 免费 | 免费版缩水；Pro $35/年 |
| 私有仓库 | 免费（填 PAT） | Pro 卖点之一 |
| 图标主题 | 免费 | Pro |
| 书签 | 无 | 免费有限额 / Pro 无限 |
| PR 审查导航 | 仅树覆盖 PR 页 | Pro |
| GHES/Gitea/Gitee | 免费 | GHES 需 Pro |
| 权限面 | 窄 | 较广（含 PII 披露） |
| 适合 | 开源偏好、免费全能 | 依赖书签/PR 审查流且愿付费 |

---

## 3. Octofree — Octotree v2 的免费社区分支

- 定位：[tamer1an/octo-free](https://github.com/tamer1an/octo-free)，Octotree 商业化后延续 v2 经典体验；React + Vite 重写，兼容 GitHub 原生路由（点击文件免硬刷新）；
- 功能：折叠文件树、分支/标签切换自动刷新、文件名自动补全搜索、**树上一键下载原始文件**、设置中可填 PAT（绕限流/访问私仓，Chrome sync storage 本地加密、不经第三方服务器）；
- 平台：[Chrome](https://chromewebstore.google.com/detail/github-file-tree-octofree/japafjdhbmaapfhklkppbigmkioikboe) / [Firefox](https://addons.mozilla.org/en-US/firefox/addon/github-file-tree-octofree/)；无 Safari；
- 风险：个人维护、社区规模小、可持续性弱于 Gitako。同类还有 Span-tree、[GitOak](https://github.com/gitoaktools/gitoak)（自称吸收 Octotree/Gitako/Span-tree 设计）。

---

## 4. Refined GitHub — 细节增强之王

- [refined-github/refined-github](https://github.com/refined-github/refined-github)：约 **32k stars**、商店 4.8 分；fregante 主导，**几乎跟随 GitHub 每次改版高频发版**；
- 定位：不改变布局，"简化界面 + 补上 GitHub 该做没做的事"，数百项特性按场景分组。

### 与 markdown 阅读相关的增强（节选）
- 文件管理：文件列表直接编辑；`t` 查找全局生效；高亮最近修改；**整目录下载**（download-directory.github.io）；
- 代码阅读：issue/PR 引用与 URL 自动链接化；增强 `y` 复制永久链接；文件头粘性；**滚动时文件树自动跟随当前文件**；CSV/TSV 首列冻结；
- PR/commit：PR 文件树已查看文件变暗；`j/k` 聚焦上/下条评论或文件；提交列表标记 merge；
- 写作：md 符号包裹选中文本、`tab` 缩进、折叠块/表格按钮、评论框自适应。

### 关于 TOC 的澄清
Refined GitHub **没有**专门 md TOC 特性——因为 GitHub 2021-04 起已原生提供 Outline。官方还在持续吸收其特性（官方化后移除）。

### 平台覆盖（同类最广）
Chrome 及 Chromium 系、Firefox（**含 Firefox Android**）、**Safari（Mac/iOS/iPadOS 上架 App Store）**；支持 GHES（手动启用）；特性可逐项开关。

---

## 5. Markdown Viewer 类扩展 — 渲染 raw 链接的 md

### 主流选择
1. **Markdown Viewer（simov）**：[商店](https://chromewebstore.google.com/detail/markdown-viewer/ckkdlimhmcjmikdlpkmbgfkaikojcbjk) / 开源 [simov/markdown-viewer](https://github.com/simov/markdown-viewer)。多解析器、GFM、**TOC**、30+ 主题、MathJax、Mermaid、emoji、自动重载；本地文件 + 远程 URL（含 raw.githubusercontent.com）通吃。本地渲染需手动开启"允许访问文件 URL"（issue #288 有指引）。
2. **markdownReader（yaniswang）**：开源轻量，双击切换 raw/渲染。
3. 其他：Markdown Reader HTML Export 等。

### ⚠️ 权限风险（关键安全考量）
- 要渲染任意 URL 的 md，必须申请 **`<all_urls>`** host 权限——Chrome 显示"读取和更改您在所有网站上的所有数据"。技术上能读取你访问的任何页面（含登录态页面）。权限揭示的是"能力上限"而非"实际行为"，但恶意扩展正是借宽权限作恶；
- **缓解**：优先开源可审计版本（simov 版）；`chrome://extensions` → 详情 → **网站访问权限**改为"点击时/仅限特定站点"，只保留 raw.githubusercontent.com；或干脆不用——站内 md 已有原生渲染，raw 文件可把 URL 换回 `github.com` 路径在线查看；
- 对比：Gitako/Refined GitHub 只申请 `github.com` 单域权限，隐私面显著更小。**装任何扩展前先看权限清单。**

---

## 6. 移动端（没有扩展生态的补救）

- **Firefox Android**：常规渠道中唯一能原生装这些扩展的浏览器——Refined GitHub 官方支持，另有 GitHub Tree View 等树形扩展；
- **iOS**：Safari 扩展体系装 App Store 版 Refined GitHub；或依赖 GitHub App + 网页原生树；重度移动使用可配 Working Copy（iOS git 客户端）clone 仓库；
- **Kiwi Browser**（Android 装 Chrome 扩展的老路）：已于 2025 年前后停止开发并从 Play 商店下架，**不建议再依赖**。

---

## 7. 横向对比总表

| 扩展 | 核心价值 | 开源 | Star/评分 | 浏览器 | 权限面 | 维护 | 链接 |
|---|---|---|---|---|---|---|---|
| **Gitako** | 快速文件树+即时搜索，免费全能 | MIT | 2.6k★ / 4.9 | Chrome/FF/Edge（Safari 实验性） | 窄 | 活跃（2026-01） | [repo](https://github.com/EnixCoda/Gitako) |
| **Octotree** | 老牌树+书签+PR 审查（部分付费） | v2 旧码 | 历史 200 万+ 用户（现约 30 万） | Chrome/Edge/FF/官网称 Safari | 较广 | 商业持续（2026-05） | [官网](https://www.octotree.io) |
| **Octofree** | 免费 Octotree v2 体验+文件下载 | 开源 fork | 小体量 | Chrome/FF | 仅 github.com | 个人维护 | [repo](https://github.com/tamer1an/octo-free) |
| **Refined GitHub** | 数百项细节增强 | 开源 | ~32k★ / 4.8 | Chrome/FF(含 Android)/Safari 全家桶 | github.com 域 | 极活跃 | [repo](https://github.com/refined-github/refined-github) |
| **Markdown Viewer (simov)** | 渲染任意/本地 md（TOC/MathJax/Mermaid） | 开源 | 千★量级 | Chrome/FF | `<all_urls>`（需收紧） | 持续 | [repo](https://github.com/simov/markdown-viewer) |

## 8. 推荐组合

- **桌面日常（免费）**：Refined GitHub + Gitako；md 目录用官方 Outline 即可；
- **只要 Octotree 老手感不付费**：Octofree 或 Gitako；
- **重度 code review、要私仓树与书签且愿付费**：Octotree Pro（$35/年）；
- **常看 raw md**：Markdown Viewer（simov），站点权限收紧到 raw.githubusercontent.com；
- **移动端**：Android → Firefox Android + Refined GitHub；iOS → App Store 版 Refined GitHub 或官方 App。

---

## 9. 信源清单

1. EnixCoda/Gitako：https://github.com/EnixCoda/Gitako （功能/stars/release/隐私声明）
2. Gitako Chrome 商店页（v3.15.4 / 2026-01-18、数据披露）：https://chromewebstore.google.com/detail/gitako-github-file-tree/giljefjcheohhamkjphiebfjnlphnokk
3. chrome-stats Gitako（host 权限/评论摘要）：https://chrome-stats.com/d/giljefjcheohhamkjphiebfjnlphnokk
4. Gitako Edge / Firefox / Safari：https://microsoftedge.microsoft.com/addons/detail/gitako-github-file-tree/alpoloddcggjhakjemghahlkofjekbca ｜ https://addons.mozilla.org/firefox/addon/gitako-github-file-tree/ ｜ https://github.com/EnixCoda/Gitako/issues/43
5. Octotree Chrome 商店页（免费/Pro 划分、v8.3.0）：https://chromewebstore.google.com/detail/octotree-github-code-tree/bkhaagjahfmjljalopjnoealnfndnagc
6. Octotree 官网 pricing 与 ovity/octotree（v2 旧码冻结）：https://www.octotree.io ｜ https://github.com/ovity/octotree
7. tamer1an/octo-free 与 GitOak：https://github.com/tamer1an/octo-free ｜ https://github.com/gitoaktools/gitoak
8. refined-github 仓库（特性/平台/官方化移除）：https://github.com/refined-github/refined-github
9. GitHub changelog：md Outline（2021-04-13）https://github.blog/changelog/2021-04-13-table-of-contents-support-in-markdown-files ｜ 文件树 GA（2022-08-17）https://github.com/github/roadmap/issues/553 ｜ 新代码视图 GA（2023-05-08）https://github.blog/changelog/2023-05-08-the-new-code-search-and-code-view-is-now-generally-available
10. Markdown Viewer：https://chromewebstore.google.com/detail/markdown-viewer/ckkdlimhmcjmikdlpkmbgfkaikojcbjk ｜ https://github.com/simov/markdown-viewer ｜ https://github.com/simov/markdown-viewer/issues/288
11. 权限安全：https://www.nudgesecurity.com/post/browser-extension-permissions-tell-you-whats-possible-not-whats-happening ｜ https://security.stackexchange.com/questions/8550/
12. 移动端：Firefox Android 树扩展 https://addons.mozilla.org/en-GB/firefox/addon/github-tree-view ｜ Kiwi 停更 https://www.reddit.com/r/browsers/comments/1iaeziw/kiwi_browsers_new_update_is_out
