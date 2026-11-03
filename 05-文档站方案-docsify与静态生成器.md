# 05 · 文档站方案：docsify、MkDocs Material、VitePress、mdBook、GitBook、Obsidian

> 调研日期：2026-11-03 ｜ 信源：17+ 个独立来源（官方文档/官方 workflow 原文，见文末）
> 一句话结论：**想"零改造把 md 仓库变网站"选 docsify（运行时渲染，免构建秒生效）或干脆开 GitHub Pages（README 即首页）；要搜索+SEO 全都要选 MkDocs Material + jieba（中文搜索开箱即用）或 VitePress；私有仓库发布 Pages 需要 Pro 套餐——想免费发布要么保持仓库公开，要么用 Cloudflare Pages/Netlify。**

---

## 1. docsify（运行时渲染）

**官网**：https://docsify.js.org ｜ **源码**：https://github.com/docsifyjs/docsify

### 1.1 原理：构建发生在每个访客的浏览器里
docsify 不生成任何静态 HTML：仓库只需一个 `index.html` 入口，浏览器加载后由 JS 通过 fetch 拉取对应路由的 `.md`，前端实时转 HTML 渲染。优点与代价同源：
- ✅ 零构建、部署极简（一个静态目录）、改 md 刷新即生效；
- ❌ SEO 差（爬虫拿到的初始 HTML 几乎为空）、首屏白屏、搜索要在浏览器端拉全部 md 才工作；静态导出至今是社区长年 feature request，官方救场方案是 `docsify-server-renderer`。

### 1.2 快速上手
```bash
npm i docsify-cli -g      # 推荐全局安装 CLI
docsify init ./docs       # 在 docs 子目录初始化
docsify serve docs        # 本地预览 http://localhost:3000
```
`init` 产生三个关键文件：
- `index.html` — 唯一入口；
- `README.md` — 首页内容；
- `.nojekyll` — **关键**：防止 GitHub Pages 的 Jekyll 忽略 `_sidebar.md`、`_coverpage.md` 等下划线文件（Pages 上侧边栏/搜索不工作，常见根因就是缺它或路径大小写错误）。

`index.html` 典型写法（含侧边栏 + 全文搜索，v5 为当前主版本）：
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>我的文档</title>
  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@5/dist/themes/vue.min.css">
</head>
<body>
  <div id="app"></div>
  <script>
    window.$docsify = {
      loadSidebar: true,          // 加载 _sidebar.md 侧边栏
      subMaxLevel: 3,             // 侧边栏自动带 h1-h3 目录
      search: {
        paths: 'auto',
        placeholder: '搜索…',
        noData: '没有结果！',
        depth: 3,
        maxAge: 86400000,         // 索引缓存 1 天（改文档搜不到时清缓存/调小）
        namespace: 'my-docs',     // 多站隔离防索引串站
      },
    };
  </script>
  <script src="//cdn.jsdelivr.net/npm/docsify@5/dist/docsify.min.js"></script>
  <!-- v5 插件路径为 dist/plugins/；v4 是 lib/plugins/ -->
  <script src="//cdn.jsdelivr.net/npm/docsify@5/dist/plugins/search.min.js"></script>
</body>
</html>
```

`_sidebar.md` 手写导航：
```markdown
- [首页](/)
- [指南](guide.md)
- [API](api.md)
```

**giscus 评论接入**（让文档站可评论）：前置条件为仓库 Public + 开启 Discussions + 安装 [giscus App](https://github.com/apps/giscus) + 到 [giscus.app/zh-CN](https://giscus.app/zh-CN) 生成配置。docsify 是 SPA，需写成自定义插件，在 `mounted` 和 `doneEach`（每次路由切换）重建 giscus iframe。

### 1.3 部署
`docs/` 目录整个推到 GitHub Pages（Settings → Pages → 选分支/目录），或 Vercel/Netlify/任意静态托管，无需后端。

### 1.4 优缺点小结

| ✅ | ❌ |
|---|---|
| 零构建、改 md 秒生效 | SEO 差、首屏白屏 |
| 配置就是一个 HTML | 搜索运行时全量拉 md，大仓库慢、**中文分词弱**（默认按空格切词，需第三方分词库或接 Algolia/MeiliSearch） |
| 插件生态丰富（搜索/高亮/emoji/PWA） | 无静态导出；复杂排版能力弱 |

成本：完全免费（MIT）。适用：个人/团队内部文档、笔记发布、SEO 无要求的 wiki。

---

## 2. docsify-this.net（在线零部署版）

**官网**：https://docsify-this.net ｜ 作者 Paul Hibbitts（免费开源，部署在 Netlify，可自托管）

### 2.1 原理与用法
"别人替你提供了一个 docsify 壳，你只提供 md URL"：粘贴 GitHub raw URL（或 blob URL，服务会转换）→ 点 "Open as Web Page" 即渲染成响应式网页。

URL 参数自定义外观：
```
https://docsify-this.net/?basePath=https://raw.githubusercontent.com/user/repo/main&homepage=README.md&font-family=Open+Sans&font-size=1.2em&link-color=#0969DA
```
常用参数：`basePath`+`homepage`（跨页导航）、`font-family`/`font-size`/`link-color`、`coverpage=true`（封面页）、`theme=docsify-darklight-theme`（暗色）、`edit-link`（右上角 Edit this Page 跳回 GitHub）、`toc`、`pagination`。还支持 iframe 无缝嵌入其他网站/LMS。

### 2.2 优缺点
✅ 真零成本零部署：改 GitHub 里的 md，页面即时更新 ｜ ❌ 依赖第三方可用性；只支持**公开** md；单文件/页面级（非整站导航）；SEO 差；受 GitHub raw 速率限制影响。
适用：给单个 README/讲义/说明书快速一个体面 URL、课堂发布、页面嵌入。

---

## 3. MkDocs Material（构建型 · Python）

**官网**：https://squidfunk.github.io/mkdocs-material/

### 3.1 特点
构建时把 md 预渲染成静态 HTML，**SEO 天然良好**；内置站内搜索（构建期生成 lunr.js 索引）、60+ 语言界面、Material Design（暗色模式/标签/社交卡片）、导航/目录/锚点自动生成、插件体系（awesome-pages、git-revision-date 等）。

### 3.2 安装与配置
```bash
pip install mkdocs-material jieba   # jieba 用于中文搜索
mkdocs new .
mkdocs serve                         # 本地实时预览
```
`mkdocs.yml` 关键片段（中文站点）：
```yaml
site_name: 我的文档
theme:
  name: material
  language: zh          # 界面中文
plugins:
  - search:
      lang: zh          # 中文搜索
markdown_extensions:
  - admonition
  - pymdownx.superfences
```

### 3.3 GitHub Actions 自动部署（官方 workflow）
存为 `.github/workflows/ci.yml`（来自[官方发布指南](https://squidfunk.github.io/mkdocs-material/publishing-your-site/)）：
```yaml
name: ci
on:
  push:
    branches: [master, main]
permissions:
  contents: write          # 关键：允许推 gh-pages 分支
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with: { python-version: "3.x" }
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV   # 每周更新构建缓存
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: ~/.cache
          restore-keys: mkdocs-material-
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```
部署后到仓库 Settings → Pages 确认发布源为 `gh-pages` 分支；站点地址 `<username>.github.io/<repo>`。

### 3.4 中文搜索（重点）
MkDocs 默认搜索基于 lunr.js，对中文弱。**Material for MkDocs 8.5.6 起原生支持中文**：装了 jieba 后，`lang: zh` 时构建期自动用 jieba 分词建索引（官方 discussion #5992 确认）。这是各方案里**中文搜索开箱体验最好的路径之一**。

### 3.5 优缺点 / 成本
✅ SEO 好；搜索强（含中文 jieba）；文档功能最全（版本提示/评论/标签） ｜ ❌ 需 Python 环境；构建 30s–2min；深度定制要懂 overrides；有自己的 md 扩展方言。
成本：开源免费；Actions 公共仓库免费，私仓消耗分钟配额。
适用：认真维护的项目文档站、要搜索与 SEO 的团队知识库。

---

## 4. VitePress（构建型 · Node/Vue）

**官网**：https://vitepress.dev

### 4.1 特点
Vue 团队出品、基于 Vite：构建时并行 SSG 预渲染完整静态 HTML（快、省内存），再注水成 Vue SPA——**兼得构建型 SEO 与应用级交互**。默认主题开箱即用：首页 hero+features、导航/侧边栏、内置本地模糊搜索（minisearch，`themeConfig.search` 一行开启）或 Algolia DocSearch、i18n、md 里直接写 Vue 组件、死链检查。
注意：项目 Pages 子路径需在 `.vitepress/config.ts` 设 `base: '/<repo>/'`。

### 4.2 部署（官方 Deploy 指南 workflow 模式）
```yaml
name: Deploy VitePress site to Pages
on:
  push: { branches: [main] }
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: npm }
      - run: npm ci
      - run: npm run docs:build        # 产出 docs/.vitepress/dist
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with: { path: docs/.vitepress/dist }
  deploy:
    needs: build
    environment: { name: github-pages }
    runs-on: ubuntu-latest
    steps:
      - uses: actions/deploy-pages@v4
```

### 4.3 与 docsify 对比（构建型 vs 运行时）

| 维度 | docsify（运行时） | VitePress（构建型） |
|---|---|---|
| 构建步骤 | 无 | `npm run docs:build` |
| SEO / 首屏 | 差（空 HTML + 白屏） | 好（完整 HTML） |
| 上手成本 | 极低（一个 html） | 中（Node 工程、config、base） |
| 改动即时性 | 推上去刷新即见 | 等 Actions 重建 |
| 扩展能力 | 插件 JS | Vue 组件/主题定制/死链检查/i18n |
| 搜索 | 运行时拉全量，中文弱 | minisearch（中文一般，可换 Algolia/Pagefind） |

成本：开源免费，构建耗 Actions 分钟。适用：前端项目文档、要性能与 SEO 的团队。

---

## 5. mdBook（Rust 生态）

**官方**：https://rust-lang.github.io/mdBook ｜ 源码：https://github.com/rust-lang/mdbook

- Rust 官方"书本"生成器：把 md 按 **`src/SUMMARY.md`** 声明的章节树编译成带左侧章节导航的静态书——与 MkDocs/VitePress 按目录自动生成导航的最大差异是**目录显式由 SUMMARY.md 控制**；
- 上手：`cargo install mdbook`（或下载预编译二进制）→ `mdbook init my-book` → `mdbook serve` 预览 → `mdbook build` 输出 `book/`；引用不存在文件会自动创建占位；不想手维护 SUMMARY.md 可用 mdbook-fs-summary 预处理器从文件树生成；
- 部署：官方 CI 文档 + GitHub starter workflow（peaceiris/actions-mdbook 安装 → `mdbook build` → actions/deploy-pages 发布）；
- ✅ Rust 单二进制、构建极快、"书"的心智模型适合教程/手册/规范 ｜ ❌ 线性章节模型不适合松散 wiki；插件生态小；中文搜索一般；
- 成本：免费开源。适用：Rust 项目、教程书、课程讲义。

---

## 6. GitBook 现状（澄清）

- 旧 GitBook 开源工具链（gitbook-cli / legacy v3）**早已停维**，"本地 gitbook build" 时代结束；
- **但 GitHub 同步没有消失**：现行 **Git Sync**（GitHub App 驱动）是**双向同步**——GitBook 编辑器的改动自动 commit 到 GitHub，GitHub 的 push 也自动同步回 GitBook；
- 用法：注册 GitBook.com → 安装 GitHub App 授权仓库 → 选同步分支 → 初始导入解析 md 生成空间结构 → 任意一端改动双向同步；托管/搜索/AI 问答全在 GitBook 云端；
- 成本：商业 SaaS。**免费档仅一个基础公开站点**；Essential 约 $65/站点/月（年付）+ $12/用户/月，自定义域名从付费档起（2024–2025 涨价明显）；开源/非营利可申请 **Community 计划**（免费无限私有内容）；
- 适用：预算充足的团队产品文档；个人小项目免费档够用但限制多。

---

## 7. 通用零成本做法：GitHub Pages + Jekyll

"纯 md 仓库零改造上线"的默认答案。

- **原理**：Pages 内置 Jekyll，开启后自动对仓库 `.md` 跑构建，每个 md 变一个静态页——**零配置也能出站**；没有 `index.md/index.html` 时，**`README.md` 被当作站点首页渲染**（benbalter/jekyll-readme-index 插件逻辑已内置）；
- **零配置档**：Settings → Pages → 选分支（`main` 根或 `/docs`）→ 完成；
- **微配置档**（推荐）：根放一个 `_config.yml`：
  ```yaml
  theme: jekyll-theme-cayman    # Pages 内置主题，无需安装
  ```
- 坑：Pages 的 Jekyll 渲染（jekyll-commonmark）与 github.com 网页渲染略有差异；插件白名单，不能随便装；纯 docsify 站要加空文件 `.nojekyll` 跳过 Jekyll；
- ✅ 真零成本零配置、README 即首页 ｜ ❌ 默认无侧边栏/站内搜索，导航靠 md 相对链接；大仓库构建慢；排版上限低；
- 成本：公开仓库完全免费。适用：小体量知识库；或作为 docsify 的宿主。

---

## 8. Obsidian + obsidian-git（反向消费：仓库当本地知识库）

**插件**：https://github.com/Vinzent03/obsidian-git

- 方向与前七种相反：把 GitHub md 仓库**克隆为 Obsidian Vault 本地浏览/编辑**，obsidian-git 自动定时 commit/push/pull，仓库即远端备份与多设备通道；
- 体验：文件树、全文搜索、双链图谱、反链、标签全部可用；与文档站方案不冲突（同一仓库可同时是 Vault + docsify/MkDocs 源）；
- **兼容性关键**：GitHub 不渲染 `[[wikilink]]` 双链语法——要在 GitHub 网页和文档站里都正常，必须在 Obsidian 设置里关闭 "Use [[Wikilinks]]"，改用标准 `[text](path.md)`；附件固定 `attachments/` 相对路径；
- **移动端短板**：iOS 走纯 JS 的 isomorphic-git（大仓库慢）；**Android 原生不支持**（插件无法调用系统 git，官方 issue #497）——替代：Termux + git 手工流、Auto Git Sync 插件，或 iOS 上用 Working Copy 做 git 层；
- 成本：Obsidian 个人免费；GitHub 免费私库即可（不发布 Pages 无付费要求）；
- 适用：以 GitHub 仓库为真相源的个人知识管理，桌面为主。

---

## 9. 盲区与横向对比

### 9.1 构建时间与成本
- 公共仓库 Actions 完全免费，MkDocs/VitePress/mdBook 中小站点构建 0.5–3 分钟/次，配 `paths: ['docs/**']` 过滤省配额；
- 私有仓库 Actions 有每月免费分钟数（个人约 2000 分钟/月），超出计费——**docsify 零构建在这项占优**；
- **Pages 硬限额**：源仓库建议 ≤1GB；**月带宽软限 100GB**；**每小时每仓库 10 次构建**——高频 push 全量部署会撞限，用路径过滤或合并提交。

### 9.2 私有仓库发布 Pages 要付费吗？——要
Pages 对公共仓库免费；**私有仓库发布需付费计划**（Pro / Team ~$4 / Enterprise ~$21 每用户每月）。更要注意：即使仓库私有、计划付费，**默认发布的站点仍是公网可访问的**——站点级访问控制（private Pages）是另一项付费能力。想"私库免费发布"只能走 Cloudflare Pages/Netlify 免费档。

### 9.3 中文搜索分词横评

| 方案 | 默认中文搜索 | 解法 |
|---|---|---|
| docsify | 差（按空格分词） | 第三方分词库或接 Algolia/MeiliSearch |
| MkDocs Material | **好**（8.5.6+ 构建期 jieba） | 基本开箱 |
| VitePress | 一般（minisearch CJK 有限） | Algolia DocSearch（公开文档免费）/ Pagefind |
| mdBook | 一般 | 主题替换/外部搜索 |
| Pages Jekyll | 无站内搜索 | 前端接 lunr/Algolia |
| GitBook | 好（云端） | 免费档可用 |

### 9.4 图片相对路径：GitHub vs 站点渲染
GitHub 把 md 里相对路径图片**相对该 md 所在目录**解析并重写到 raw 地址；而很多 SSG 按站点 URL 结构解析——"GitHub 上能看、部署后断链"非常普遍。兼容性从高到低：
- **图片与 md 同目录**：docsify、GitHub 网页、Jekyll 基本都兼容——最适合"仓库兼网站"的写法；
- MkDocs：同目录图片可用（docs/ 下文件按原路径拷贝）；
- VitePress：注意 `base` 子路径，静态资源放 `public/` 或相对路径编译期处理；
- Hugo 类 page-bundle 模型差异最大，需构建期重写。

### 9.5 文档更新自动同步
标准答案：**push 触发 Actions + paths 过滤**：
```yaml
on:
  push:
    branches: [main]
    paths: ['docs/**']     # 只在文档目录变化时构建
  workflow_dispatch:
```
GitBook 同步由其 GitHub App 监听 push 自动完成，无需自写 workflow。

---

## 10. 选型结论

| 需求 | 推荐 |
|---|---|
| 单文件/README 快速给个体面 URL | **docsify-this.net** |
| 纯 md 仓库零改造、改完即生效 | **docsify**（index.html + .nojekyll + _sidebar.md） |
| 认真做中文文档站：搜索+SEO 全都要 | **MkDocs Material + jieba**（官方 workflow 五分钟上线） |
| 前端栈、Vue 生态与现代 DX | **VitePress** |
| 教程/手册/"一本书"结构 | **mdBook** |
| 团队有预算、云端编辑与托管 | **GitBook（Git Sync 双向同步仍在）** |
| 纯本地/个人知识库读写 | **Obsidian + obsidian-git**（关 wikilinks；Android 无原生支持） |
| 什么都不想装 | **GitHub Pages 默认 Jekyll**：开 Pages，README 即首页 |

**总原则**：运行时渲染（docsify 系）用"免构建、即时性"换 SEO 与搜索；构建型用"等 1–3 分钟 CI"换 SEO、搜索与可维护性。中文内容优先 MkDocs Material；个人轻量优先 docsify 或 Pages+Jekyll。

---

## 11. 信源清单

1. docsify 官网与 Quickstart：https://docsify.js.org ｜ https://github.com/docsifyjs/docsify
2. docsify v5 插件路径变更：https://github.com/docsifyjs/docsify/issues/1061 ｜ 中文插件文档：https://github.com/docsifyjs/docs-zh/blob/main/plugins.md
3. opensource.com：docsify+Pages https://opensource.com/article/20/7/docsify-github-pages ｜ Docsify-This https://opensource.com/article/23/5/docsify-markdown-html
4. Docsify-This 官网/参数：https://docsify-this.net ｜ https://docsify-this.net/README.md ｜ https://hibbittsdesign.org/docsify-this.html
5. MkDocs Material 官方发布指南（workflow 原文）：https://squidfunk.github.io/mkdocs-material/publishing-your-site/
6. 中文搜索 jieba：https://github.com/squidfunk/mkdocs-material/discussions/5992 ｜ https://github.com/squidfunk/mkdocs-material/issues/3915
7. VitePress 官方：https://vitepress.dev/guide/getting-started ｜ https://vitepress.dev/guide/deploy ｜ https://vitepress.dev/reference/default-theme-search
8. mdBook 官方：https://rust-lang.github.io/mdBook/guide/creating.html ｜ https://rust-lang.github.io/mdBook/continuous-integration.html ｜ starter workflow：https://github.com/actions/starter-workflows/blob/main/pages/mdbook.yml
9. GitBook Git Sync 与定价：https://gitbook.com/docs/docs-as-code/git-sync ｜ https://www.gitbook.com/pricing ｜ Community 计划：https://gitbook.com/docs/account-and-billing/plans/community
10. Pages+Jekyll/README 首页：https://github.com/orgs/community/discussions/22400 ｜ https://github.com/benbalter/jekyll-readme-index ｜ https://www.markdownguide.org/tools/github-pages
11. Pages 限额与私仓付费：https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits ｜ https://github.com/orgs/community/discussions/65262
12. obsidian-git 移动端：https://github.com/Vinzent03/obsidian-git/issues/497 ｜ Termux 方案：https://werzum.github.io/tech/2022/02/13/Obsidian-Mobile-Sync.html
13. wikilinks 兼容性：https://forum.obsidian.md/t/which-type-of-links-should-i-use/8767
14. 图片路径问题：https://discourse.gohugo.io/t/documents-posted-on-github-looks-different-from-rendered-by-hugo/41339
15. Actions paths 过滤：https://docs.github.com/actions/using-workflows/workflow-syntax-for-github-actions ｜ https://github.com/dorny/paths-filter
16. giscus：https://giscus.app/zh-CN ｜ 接入教程：https://juejin.cn/post/7325800661232074791
17. docsify 中文搜索讨论：https://github.com/docsifyjs/docsify/issues/693
