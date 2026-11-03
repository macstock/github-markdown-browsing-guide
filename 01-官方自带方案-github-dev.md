# 01 · 官方自带方案：github.dev、vscode.dev 与 GitHub 原生渲染

> 调研日期：2026-11-03 ｜ 信源：官方文档为主 + 社区交叉验证（16 个来源，见文末）
> 一句话结论：**GitHub 官方自带的浏览器内完整 VS Code，按 `.` 一键打开，免费、支持私有仓库，markdown 分栏预览体验最佳，但没有终端、不能运行代码。**

---

## 1. 三条官方主线速览

| 主线 | 一句话定位 | 打开方式 |
|---|---|---|
| **github.dev** | 浏览器内的完整 VS Code（轻量编辑器） | 仓库/PR 页按 `.` 或 `>`，或把 URL 中 `github.com` 换成 `github.dev` |
| **vscode.dev** | 同一引擎的独立版，可开本地文件夹 | 直接访问 https://vscode.dev |
| **GitHub 网页原生渲染** | README 与任意 `.md` 自动渲染 GFM | 打开仓库即用，无需任何操作 |

三者的关系：github.dev 本质是 vscode.dev 的「GitHub 定制入口」——预装 GitHub Repositories 扩展、预认证、自动加载当前仓库。两者共享同一套 VS Code Web 引擎，在 github.dev 里把地址改成 `insiders.vscode.dev` 还可以切 Insiders 版。

---

## 2. github.dev 深度解析

### 2.1 定位与原理

- 跑在**浏览器沙箱里的 VS Code**，不克隆仓库：通过 **GitHub Repositories 扩展**以「虚拟文件系统」方式按需从 GitHub API 加载文件；
- 未提交的修改保存在**浏览器 localStorage**（官方提醒：不 commit 就清浏览器存储会丢改动）；
- **完全免费**，且**不消耗 Codespaces 额度**（无容器、无计算，不产生 core-hours 计费）；
- 注意：宣传仓库 [github/dev](https://github.com/github/dev) 已于 2025-03 归档只读，但这只是宣传仓库归档，**功能本身仍在维护**，官方文档现挂在 Codespaces 产品线下（[Using VS Code for the Web from GitHub](https://docs.github.com/en/codespaces/the-githubdev-web-based-editor)）。

### 2.2 如何打开（4 种方式）

| # | 方式 | 操作 | 结果 |
|---|---|---|---|
| 1 | 快捷键（当前标签页） | 仓库/PR 页按 `.`（句点键） | 同标签页打开 |
| 2 |快捷键（新标签页） | 按 `>` | 新标签页 |
| 3 | 改 URL | `github.com/github/dev` → `github.dev/github/dev` | 新标签页 |
| 4 | 文件页下拉菜单 | 编辑按钮旁下拉 → 选 github.dev | 打开并定位到该文件 |

> 前提：**必须登录 GitHub**。未登录时按 `.` 无反应——这是社区里「`.` 键突然失效」求助帖的最常见原因。

### 2.3 markdown 阅读能力矩阵

| 能力 | 支持 | 说明 |
|---|---|---|
| 文件树 | ✅ | 完整 VS Code Explorer，虚拟文件系统按需加载 |
| **markdown 分栏预览** | ✅ | `Ctrl+Shift+V` 打开预览，`Ctrl+K V` 侧边分栏预览，左源码右渲染实时滚动 |
| 全局全文搜索 | ✅ | `Ctrl+Shift+F` 跨文件搜索；`Ctrl+P` 文件名快速打开；`Ctrl+Shift+O` 在长文档内按标题跳转 |
| 语法高亮 | ✅ | Tree-sitter；TS/JS/Python 有浏览器内语言服务 |
| 编辑并 commit | ✅ | 改 → stage → Commit & Push，可直接建 PR |
| 安装扩展 | ⚠️ | 仅「web extensions」（浏览器沙箱版扩展） |
| 终端 / 运行 / 调试 | ❌ | 无计算能力 |

### 2.4 编辑并 commit 的完整流程

1. 仓库/PR 页按 `.` 打开 github.dev；
2. 编辑 `.md`（可多开、用 `Ctrl+K V` 分栏预览核对）；
3. 左侧 Source Control 视图 → 文件旁 `+` 暂存；
4. 写 commit message → **Commit & Push**；
5. 可点侧栏顶部 PR 图标**直接创建 Pull Request**。

加分项：
- **切分支不用 stash**，未提交改动会跟着到新分支；
- 在 PR 页按 `.` 打开后提交，改动**直入该 PR 分支**；
- 支持 Settings Sync 同步桌面版 VS Code 设置与兼容扩展。

### 2.5 私有仓库 / 登录 / 免费 / 移动端

| 问题 | 结论 |
|---|---|
| 私有仓库 | ✅ 支持，有读权限即可打开；push 需写权限 |
| 登录要求 | 必须登录 GitHub（账号需已验证邮箱才能 push） |
| 是否免费 | 完全免费，无套餐门槛，不耗 Codespaces 额度 |
| 移动端 | iPad 外接键盘尚可；**手机无实体键盘按不了 `.`**，需手动改 URL；GitHub 移动 App 内未集成 |

### 2.6 局限清单

1. ❌ 无终端、无法运行/构建/调试；
2. ❌ 扩展仅限 web extensions，重型扩展（Python/C++）不可用；
3. ⚠️ 改动存 localStorage，不 commit 有丢失风险；
4. ⚠️ 浏览器快捷键冲突：Firefox 的 `Ctrl+Shift+P` 被占用（改用 `F1`），`Ctrl+N`/`Ctrl+W` 需用替代键；
5. ⚠️ 大仓库性能：虚拟 FS 每个文件操作都是一次网络往返，100+ 文件的仓库就有可感知卡顿（HN 用户报告 + VS Code issue 佐证）；
6. ⚠️ Safari/Firefox 体验可能与 Chrome/Edge 有差异。

### 2.7 键盘流速查

| 键 | 作用 |
|---|---|
| `.` | 仓库/PR 页打开 github.dev（当前标签页） |
| `>` | 新标签页打开 |
| `,` | 打开 Codespaces（有算力的完整环境，开始计费） |
| `t` | GitHub 网页文件查找器（模糊匹配文件名） |
| `w` | 网页端切分支 |
| `/` | 聚焦搜索框 |
| `F1` | github.dev 命令面板 |
| `Ctrl+P` | 按文件名快速打开 |
| `Ctrl+Shift+F` | 全局搜索 |
| `Ctrl+K V` | markdown 侧边分栏预览 |
| `Ctrl+Shift+O` | 当前文件内按标题/符号跳转 |

---

## 3. vscode.dev 与 github.dev 的区别

| 维度 | github.dev | vscode.dev |
|---|---|---|
| 入口 | 仓库/PR 页按 `.`，或 `github.dev` 域名 | 直接访问 vscode.dev |
| 适用对象 | 只能打开 GitHub 仓库/PR | GitHub 仓库 / Azure Repos / **本地文件夹** / Remote Tunnels |
| 本地文件编辑 | ❌ | ✅（File System Access API） |
| 登录 | 打开即用 GitHub 会话 | 浏览本地文件可匿名 |
| 计算 | 无终端/运行/调试 | 同左（要算力用 Codespaces / Remote Tunnels） |
| 特色 | 改完即 commit/push、建 PR、`Continue Working On` 一键升级 Codespaces | 主题试用、Live Share 客人、Chrome/Edge 地址栏 `code`+仓库名直达 |

**结论**：浏览/编辑 GitHub 仓库的 markdown → github.dev 更顺手；编辑本地 md → vscode.dev 或桌面 VS Code。

---

## 4. GitHub 网页原生渲染能力与不足

### 4.1 能力

- **渲染范围**：README、任意 `.md` 文件视图、Wiki、Issue/PR/Discussion、Gist 均自动渲染；文件视图有 Code/Preview 切换；
- **GFM 完整支持**：表格、任务列表、删除线、自动链接、@引用、告警块（`> [!NOTE]` 等）；
- **Mermaid**：` ```mermaid ` 代码块直接渲染成图；
- **数学公式**：`$...$` / `$$...$$` 由 MathJax 渲染；
- **锚点**：标题自动生成锚点（转小写、空格→连字符、去标点，重复标题加 `-1`），hover 标题可复制链接；
- **Outline 目录**：md 文件视图头部有「大纲」下拉展示标题树。

### 4.2 仓库内搜索 markdown 全文

- 新一代 code search（Blackbird 引擎，2023-05 GA）对 markdown **支持全文检索**：`path:*.md 关键词`、`extension:md`、`content:关键词`、正则等；
- **硬限制**：只索引**默认分支**；只索引 **384KB 以下**文件；fork 不索引；私有仓库有索引数量上限。

### 4.3 不足清单

1. ❌ README 正文**不会自动生成目录**（只有 Outline 下拉）；
2. ❌ 不是文档站：无多页导航侧边栏、无文档级全文搜索体验；
3. ⚠️ HTML 白名单渲染，自定义 CSS/JS 被剥离；
4. ⚠️ `.rst`/`.adoc` 等非 md 格式网页不渲染为富文本；
5. ⚠️ 移动 App 渲染引擎与 Web 不同：LaTeX 公式 iOS 渲染异常、mermaid 常显示为源码、图片比例可能压扁；
6. ⚠️ 超 100MB 文件网页无法查看。

---

## 5. 盲区专项

### 5.1 无障碍
- 网页原生渲染是语义化 HTML + 锚点，对屏幕阅读器友好，官方有键盘快捷键体系（`?` 弹出面板）；
- github.dev 继承 VS Code 无障碍体系，但 Monaco 编辑器在 VoiceOver 下有未关闭的朗读缺陷，**视障用户建议用网页原生渲染视图而非 github.dev**。

### 5.2 大仓库性能
- 虚拟 FS 的代价：HN 报告 100+ 文件即卡顿；缓解方案：小仓库无感，大 monorepo 用 `Continue Working On` 升级 Codespaces，或克隆本地，只读浏览可考虑 github1s。

### 5.3 Codespaces 免费额度关系

| 项 | github.dev | Codespaces |
|---|---|---|
| 本质 | 浏览器编辑器，无容器 | 云端容器，有完整算力 |
| 计费 | 完全免费，不占额度 | 个人每月免费 120 core-hours + 15GB 存储（默认 2-core ≈ 60h/月）；超额需设 spending limit，否则可能扣费 |
| 互通 | `Continue Working On` 一键升级 | 可回到轻量编辑 |

---

## 6. 场景化操作步骤

**A. 分栏阅读/校对整仓 markdown**
1. 登录 GitHub → 进目标仓库/PR → 按 `.`；
2. `Ctrl+P` 按文件名打开 `.md`；
3. `Ctrl+K V` 分栏预览，左源码右渲染；
4. `Ctrl+Shift+O` 标题跳转，`Ctrl+Shift+F` 跨文件搜索。

**B. 在线改 README 并提交**
1. 文件页编辑按钮旁下拉 → github.dev；
2. 编辑 + 分栏预览核对；
3. Source Control → stage → commit message → Commit & Push（需要分支就在状态栏 Create new branch）；
4. 需要评审就点 PR 图标建 PR。

**C. 纯浏览不离开网页**
1. 首页看渲染好的 README；文件页 Outline 看目录；
2. `t` 模糊找文件；`path:*.md 关键词` 全文搜 md 内容；
3. 点 Code 按钮看源码。

**D. iPad/手机**
1. 手机浏览器改 URL 为 `github.dev`（App 内不可用）；
2. iPad + 外接键盘接近桌面体验；
3. App 内公式/Mermaid 乱码 → 「在浏览器中打开」。

---

## 7. 优缺点总结与场景矩阵

**优点**：零安装、免费、登录即用；markdown 实时分栏预览是原生编辑器没有的能力；支持私仓；不耗 Codespaces 额度；沙箱内运行不克隆仓库，打开不可信仓库攻击面小。

**缺点**：无终端/运行/调试；web-only 扩展受限；localStorage 丢失风险；大仓库卡顿；浏览器快捷键冲突；原生页面无正文 TOC、移动 App 渲染缺陷多。

| 场景 | 首选 |
|---|---|
| 通读/校对多篇 md、要目录与搜索 | github.dev（分栏预览 + 全局搜索） |
| 快速改 README/docs 并提交/发 PR | github.dev |
| 手机临时看文档 | 网页/App 原生渲染（简单 md） |
| 公式/Mermaid 重度文档 | 桌面浏览器渲染（移动 App 有缺陷） |
| 编辑本地 md | vscode.dev / 桌面 VS Code |
| 大 monorepo、要跑命令 | Codespaces（注意额度） |
| 只读快速浏览 | github.com 原生（配 `t` 文件查找器） |

---

## 8. 信源清单

**官方（一手）**
1. GitHub Docs — Using VS Code for the Web from GitHub：https://docs.github.com/en/codespaces/the-githubdev-web-based-editor
2. VS Code Docs — vscode.dev：https://code.visualstudio.com/docs/editor/vscode-web
3. github/dev 仓库 README（2025-03 归档）：https://github.com/github/dev
4. GitHub Docs — Basic writing and formatting syntax：https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax
5. GitHub Docs — Creating diagrams（Mermaid）：https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams
6. GitHub Docs — Writing mathematical expressions：https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions
7. GitHub Docs — Searching code：https://docs.github.com/en/search-github/searching-on-github/searching-code
8. GitHub Blog — The technology behind GitHub's new code search：https://github.blog/engineering/architecture-optimization/the-technology-behind-githubs-new-code-search
9. GitHub Docs — Keyboard shortcuts：https://docs.github.com/en/get-started/accessibility/keyboard账号-shortcuts
10. VS Code Blog — vscode.dev 发布文：https://code.visualstudio.com/blogs/2021/10/20/vscode-dev
11. VS Code Docs — Accessibility：https://code.visualstudio.com/docs/editor/accessibility

**社区/三方交叉验证**
12. GH-900 教材对照：https://notes.kodekloud.com/docs/GitHub-Foundations-Certification/GitHub-Codespaces/Differences-Between-githubdev-Editor-and-GitHub-Codespace/page
13. 移动 App 渲染缺陷（官方社区）：https://github.com/orgs/community/discussions/198652 、https://github.com/orgs/community/discussions/160188
14. Codespaces 额度讨论：https://github.com/orgs/topics/codespaces 、https://github.com/orgs/community/discussions/154496
15. 大仓库性能：https://news.ycombinator.com/item?id=36855517 、https://github.com/microsoft/vscode/issues/185413
16. Visual Studio Magazine（发布背景）：https://visualstudiomagazine.com/articles/2021/08/31/github-vs-code.aspx
