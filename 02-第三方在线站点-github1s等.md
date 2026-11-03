# 02 · 第三方在线站点：github1s、gitprint、raw.githack、Sourcegraph、grep.app

> 调研日期：2026-11-03 ｜ 信源：20+ 个独立来源（见文末）
> 一句话结论：**「改 URL 即用」的第三方站点各有分工——读代码用 github1s，打印/归档用 gitprint.me，外链资源用 raw.githack/jsDelivr，跨仓库搜代码用 grep.app + GitHub 官方搜索；但都要警惕：第三方代理能看到你的浏览内容，免费服务有停运风险（RawGit 2019 停运、gitprint.com 疑似已死）。**

---

## 0. 速览对比表

| 工具 | URL 换法 | 核心用途 | 维护状态 | Markdown 渲染 | 国内可达性 |
|---|---|---|---|---|---|
| github1s.com | `github.com` → `github1s.com` | VS Code 界面浏览公开仓库 | 活跃（2025 年仍有 v0.35.2 更新） | 原文查看 | 总体可达，速度不稳 |
| github.dev（官方对照） | `github.com` → `github.dev` 或按 `.` | 官方浏览器版 VS Code | 官方持续维护 | 原文/预览均有 | 随 github.com |
| gitprint.com | `github.com` → `gitprint.com` | README/md 渲染成印刷体打印页 | **疑似已停运** | 渲染+打印/PDF | 不明 |
| gitprint.me | 需 OAuth 登录粘贴 URL | **整仓库转 PDF 书籍** | 活跃（开源 plutov/gitprint） | 渲染成 PDF | 不明 |
| raw.githack.com | `raw.githubusercontent.com` → `raw.githack.com` | 修正 Content-Type 的文件代理 | 存续（纯 nginx 项目） | **不渲染 md** | 一般可达 |
| Sourcegraph.com | 站内搜索入口（非换域） | 公开仓库全文搜索/浏览 | 覆盖收缩中（2024 索引缩减、转 Fair Source） | 搜索结果有 Markdown View | 不明 |
| grep.app | 站内搜索入口 | 跨 50万–100万 仓库正则搜代码 | 活跃（2024 被 Vercel 收购） | 不渲染 md，只搜代码 | 不明 |
| DeepWiki（详见 03） | `github.com` → `deepwiki.com` | AI 生成仓库 wiki | 活跃（Cognition 2025-05 上线） | AI 渲染 wiki | 不明 |

---

## 1. github1s.com

### 1.1 功能
把任意**公开** GitHub 仓库装进浏览器版 VS Code：完整文件树、代码高亮、`Ctrl+Shift+F` 全局搜索、Git 历史/文件时间线（v0.35.2 新增）、diff 查看，全程只读、免 clone。

### 1.2 原理（"1 秒"怎么来的）
- 纯前端静态项目（GitHub Pages 托管），内嵌 **VS Code Web 构建**（与 vscode.dev 同源架构）；
- 自研带 `FileSystemProvider` 的 VS Code 扩展，把仓库"挂载"成虚拟文件系统，数据经 **GitHub REST API** **懒加载**——首次只拉目录元数据，点开哪个文件才拉哪个文件；
- 前端解析 `github1s.com/{owner}/{repo}/tree/{branch}/...` 还原 owner/repo/branch/path 后调 API。

### 1.3 使用方法
1. 打开任意 GitHub 仓库页；
2. 地址栏 `github.com` 改为 `github1s.com`，回车；
3. 用侧栏文件树浏览、`Ctrl+Shift+F` 搜索；
4. 遇到 rate limit 提示时，点设置图标粘贴 GitHub PAT（配额升到 5000/h）；
5. 可装第三方扩展（Cutwell/github1s-chrome-extension 等）一键跳转。

### 1.4 维护状态
2021-02 发布即爆红（HN 全网刷屏），此后更新放缓但**未停更**：2025 年年中仍有 v0.35.2 更新（新增 Git 历史与文件时间线）。GitHub 官方 github.dev 出现后独特性下降，属"低频维护但存活"。

### 1.5 限制
- 匿名配额 **60 次/小时/IP**（GitHub API 匿名限额），浏览大仓库或多级目录很快耗尽；填 PAT 升至 5000/h；
- 公开仓库免登录可用；**私有仓库必须填 token**（存 localStorage，有 XSS 窃取风险面）；
- 超大仓库目录树加载慢、易限流；纯只读。

### 1.6 自建部署
1. `git clone https://github.com/conwnet/github1s.git && cd github1s`
2. `yarn` 安装依赖 → `yarn build` 产出静态 bundle；
3. 任意静态服务器托管 `dist/`（或 Docker：node 构建 + nginx 托管）；
4. 注意：自建站同样是浏览器直连 GitHub API，私仓/高配额仍需自行解决 token 下发或服务端代理。

**优缺点**：IDE 级浏览、全局搜索强、开源可自建、URL 改动极小 ｜ 依赖 API 配额、大仓库体验差、PAT 交给第三方有隐私顾虑、被官方 github.dev 逐步替代。

**链接**：https://github1s.com ｜ https://github.com/conwnet/github1s

---

## 2. gitprint.com / gitprint.me

### 2.1 原 gitprint.com（2014）
把 GitHub 上任意 md 文件页渲染成排版精良的"印刷体"视图，配合浏览器 Ctrl+P 生成 PDF。**现状判断：疑似停运**——原项目 2014–2016 后无更新，近年社区讨论全部转向 gitprint.me，无任何 2023–2026 年仍在线的证据。**不建议依赖。**

### 2.2 gitprint.me（活跃替代）
开源项目 plutov/gitprint 的配套服务，功能升级为**整仓库转 PDF 书籍**：
1. 打开 https://gitprint.me → Sign in with GitHub；
2. 粘贴仓库 URL → 生成可下载的排版 PDF。

优点：整仓库、可离线归档；缺点：需 GitHub OAuth 授权（注意审查授权范围）。

同类：markdownprint.com、printify.base-station.io、开源 CLI 的 md-to-pdf。

**链接**：https://gitprint.me ｜ https://github.com/plutov/gitprint

---

## 3. raw.githack.com（RawGit 后继）

### 3.1 用途
`raw.githubusercontent.com` 对多数文件返回 `text/plain`，浏览器不渲染。raw.githack 以正确 Content-Type 重新发出同一文件，让 HTML/CSS/JS/图片"改个域名就能直接显示"，叠加 CDN 缓存。前身 RawGit 已于 2019-10 停运，githack 是主流继任者（另两个是 jsDelivr、Statically）。

### 3.2 用法
1. GitHub 文件页点 Raw 复制 `raw.githubusercontent.com/...` 链接；
2. 换成 `raw.githack.com`（开发，短缓存）或 `rawcdn.githack.com`（生产，长缓存）；
3. 直接打开即正确渲染；强制刷新缓存：`curl -X DELETE 'https://raw.githack.com/purge?url=<原URL>'`。

### 3.3 对 markdown 的实际效果（重点澄清）
**基本无效。** 浏览器原生不渲染 markdown——无论 Content-Type 是 text/plain 还是 text/markdown，看到的都是源码文本。它的价值域是 HTML/CSS/JS 演示页、图片、可被外链的静态资源。要渲染 md 请用 GitHub 自带 blob 页、github.dev 或 docsify-this（见 05）。

### 3.4 替代
- jsDelivr：`cdn.jsdelivr.net/gh/user/repo@version/path`（注意大陆主域被墙）
- Statically：`cdn.statically.io/gh/...`（带图片缩放）

**链接**：https://raw.githack.com

---

## 4. Sourcegraph 公共实例

### 4.1 能力
云端代码搜索与浏览：全文/正则/符号搜索（`repo:`、`file:`、`lang:`、`contains:file:` 等谓词）、完整文件树、blob 视图、commit/diff 搜索；对 md 有 "Markdown View" 展示能力。无需 clone 跨仓库检索。

### 4.2 用法
1. 打开 https://sourcegraph.com/search；
2. 如 `repo:torvalds/linux file:README lang:markdown`；
3. 点结果进 blob 页（高亮、符号跳转）；
4. 直接浏览仓库：`https://sourcegraph.com/github.com/{owner}/{repo}`。

### 4.3 重要变化（盲区）
- 2023 弃 Apache 2.0 转 source-available；2024 转 **Fair Source**（FCL/FSL 延迟开源）；
- **2024 年中公开索引被悄然缩减**为"策展的开源仓库子集"，社区强烈不满——小项目/学术仓库可能搜不到；
- 自建企业版可全量。

**优缺点**：跨仓库搜索能力顶级、查询语法丰富 ｜ 索引不再全覆盖、闭源化不确定性、非换域入口。

**链接**：https://sourcegraph.com/search ｜ https://sourcegraph.com/docs/code-search

---

## 5. grep.app 与 GitHub 官方代码搜索

### 5.1 grep.app
Solr-based 跨仓库正则代码搜索，索引近百万公开仓库，支持正则、语言/license/repo 过滤；2024-11 被 Vercel 收购，2025-05 用 Next.js 重构提速；提供公开 API 与 MCP Server，被大量 AI 工具用作数据源。**不渲染 md，定位是"跨项目找用法"。**
用法：打开 https://grep.app → 正则/关键字 → 过滤 → 点结果看原文（可跳回 GitHub）。

### 5.2 GitHub 官方新代码搜索（Blackbird）
2023-05 GA，比旧引擎快约 2 倍：斜杠正则 `/log.*\.java/`、子串、符号搜索、新代码视图。仓库内按 `/` 或 github.com/search。能直接命中 md 内容（`path:*.md`），对"浏览 md"的帮助是**定位**——找到后回 blob 页阅读。

**优缺点**：官方全覆盖、与仓库页无缝 ｜ 正则语法受限、fork 不索引、移动端不可用。

**链接**：https://grep.app ｜ https://github.com/search

---

## 6. 盲区与风险分析

### 6.1 隐私
- **中间人属性**：github1s、githack、gitprint 类站点经第三方代理转发 GitHub 数据，能记录你浏览的所有仓库/文件/查询词；
- **Token 暴露面**：github1s 的 PAT 存 localStorage（XSS 窃取风险）；gitprint.me 需 GitHub OAuth。建议为这类工具发**最小权限、可随时吊销**的 fine-grained token（只读、限定仓库），绝不用主账号高权限 token；
- **代理缓存**：githack CDN 缓存可能延长已删敏感文件的可获取时间（有 purge 接口）；切勿把密钥写进会推到公开仓库的文件。

### 6.2 失效风险（历史反复验证）
- RawGit 2019-10 停运；gitprint.com 疑似停运；
- jsDelivr 2021-12 丢失中国 ICP、2022-04 被 DNS 污染，大陆主域至今不可用——免费第三方服务随时可能消失；
- 对策：多备同类工具；关键场景自建（github1s 可自建、githack 本质是 nginx 反代、jsDelivr 可自建镜像）。

### 6.3 国内可达性
- github1s.com：国内文章普遍列为 GitHub 打不开时的替代手段，总体可达但速度随时段波动；
- jsDelivr：大陆主域被墙，需自建反代或国内镜像；
- raw.githubusercontent.com 长期不可达 → githack 体验受上游影响；
- sourcegraph.com / grep.app / gitprint.me：无权威可达性数据，使用前自测。

### 6.4 替代品全景（按改 URL 难度）
- 换域名：github1s、github.dev、deepwiki.com、raw.githack、jsDelivr/Statically；
- 低门槛入口：Sourcegraph（站内）、grep.app、gitprint.me（OAuth）、Gitpod/StackBlitz（URL 前缀打开仓库）、surfcodes/surf、freestuff.dev/alternative/github1s 收录的更多替代。

---

## 7. 结论建议

- **读代码**：能上 GitHub 用官方 github.dev（按 `.`）；网络不佳或要更强搜索用 github1s（配最小权限 PAT）；
- **读文档/打印**：gitprint.com 不可依赖 → gitprint.me（整仓 PDF）或本地 md-to-pdf；
- **外链静态资源**：raw.githack/Statically（开发），jsDelivr（海外生产）/自建反代（国内）；
- **跨仓库搜代码**：GitHub 官方正则搜索（单仓深挖）+ grep.app（跨仓找用法）；Sourcegraph 收缩后更适合自建企业方案；
- **读不懂大仓库**：DeepWiki 一键换域生成 wiki，内容需回源核对（详见 03）。

---

## 8. 信源清单

1. conwnet/github1s：https://github.com/conwnet/github1s
2. HN github1s 讨论：https://news.ycombinator.com/item?id=26083919
3. dev.to github1s：https://dev.to/spiderpig86/github1s-instantly-browse-projects-on-vscode-in-your-browser-34i
4. Medium github1s 限流细节：https://medium.com/better-programming/how-to-browse-github-repos-instantly-with-vscode-de7055480de8
5. 知乎 github1s 原理：https://zhuanlan.zhihu.com/p/350330364
6. github1s v0.35.2 更新（2025）：https://www.linkedin.com/posts/undercode-news_github1s-0352-just-got-updated-with-a-major-activity-7513510067124498433-JPxS
7. HN GitPrint 发布（2014）：https://news.ycombinator.com/item?id=7405376
8. GitPrint 2015 用法：https://www.pipiscrew.com/threads/1159
9. plutov/gitprint：https://github.com/plutov/gitprint ｜ 原版存档：https://github.com/adamburmister/gitprint.com
10. Stack Overflow 打印整仓：https://stackoverflow.com/questions/59938160/print-entire-github-repository
11. raw.githack 官网：https://raw.githack.com
12. SO 能否直接运行 GitHub HTML：https://stackoverflow.com/questions/6551446/
13. Twistblogg GitHub 托管文件：https://www.twistblogg.com/2020/06/use-github-for-hosting-files.html
14. Sourcegraph 文档：https://sourcegraph.com/docs/code-search
15. Sourcegraph 综述：https://www.seporaitis.net/posts/2023/06/13/sourcegraph
16. HN 公共索引缩减：https://news.ycombinator.com/item?id=41298043
17. Reddit Sourcegraph 弃开源：https://www.reddit.com/r/hackernews/comments/14qel2j/
18. TechTarget Fair Source：https://www.techtarget.com/news/IT-vendors-still-search-for-open-source-license-sweet-spot/858605 ｜ https://fairsource.org
19. GitHub 社区新代码搜索：https://github.com/orgs/community/discussions/38692
20. Visual Studio Magazine code search GA：https://visualstudiomagazine.com/articles/2023/05/09/github-code-search-ga.aspx
21. HN Vercel 收购 Grep：https://news.ycombinator.com/item?id=42203245
22. Cognition DeepWiki 发布：https://cognition.com/blog/deepwiki
23. jsDelivr 国内失效：https://www.v2ex.com/t/849894 ｜ https://www.cnblogs.com/apolloren/p/18115995
24. Statically：https://statically.io
25. 掘金 github1s 国内使用：https://juejin.cn/post/7096162080708886536
26. freestuff.dev github1s 替代：https://freestuff.dev/alternative/github1s
27. vscode #135207 github.dev 限制：https://github.com/microsoft/vscode/issues/135207
