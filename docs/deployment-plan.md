# Daily Delta 博客配置与部署计划

> 目标：把基于 AstroPaper 模板的英文博客 `Daily Delta` 配置完成，并部署到 GitHub Pages。
>
> - 仓库：https://github.com/George-102/daily-delta
> - 线上地址：https://george-102.github.io/daily-delta/
> - 技术栈：Astro + TypeScript + Tailwind CSS + Pagefind
> - 包管理器：pnpm（Node ≥ 22.23.4）

## 进度总览

| 阶段 | 内容               | 状态                                                  |
| ---- | ------------------ | ----------------------------------------------------- |
| A    | 本地环境（一次性） | ✅ 已完成                                             |
| B    | 本地跑通验证       | ✅ 已完成（初次的 `pnpm build` 失败已随 D 阶段修复）  |
| C    | 站点身份配置       | ✅ 已完成（about.md 已写好，配置文件 8–13 项已核对）  |
| D    | 清理模板痕迹       | ✅ 已完成（15–24 全部处理，含 2 个替代方案）          |
| E    | 提交前检查         | ✅ 已完成（format / lint / build 全绿，4 个原子提交） |
| F    | GitHub 网页侧设置  | ✅ 已完成（Pages Source 早已是 Actions，线上已验证）  |
| G    | 日常写作循环       | ✅ 已沉淀为固定流程（见下方速查表）                   |

> 站点已上线：https://george-102.github.io/daily-delta/
> 首次完整部署记录：`d335a11..210f6d9`，Actions run `45179969116`（build + deploy 两个 job 全绿）

---

## A. 本地环境（一次性）

- [x] 1. 运行 `node -v`，确认 ≥ 22.23.4（`package.json` 的 engines 要求）
- [x] 2. 运行 `pnpm -v`，没有就 `corepack enable` 装 pnpm
- [x] 3. 运行 `pnpm install` 装依赖

## B. 先跑通本地（不改任何内容）

- [x] 4. 运行 `pnpm dev` 启动开发服务器
- [x] 5. 浏览器打开 `http://localhost:4321/daily-delta/`，确认首页能正常显示
- [x] 6. 运行 `pnpm build`，确认 `astro check` + Pagefind 索引都通过
- [x] 7. 运行 `pnpm preview`，确认打包产物正常

## C. 站点身份配置

- [x] 8. 核对 `astro-paper.config.ts` 的 `site.url` 与仓库地址完全一致
- [x] 9. 核对 `astro.config.ts` 的 `base` 与仓库名一致
- [x] 10. 改 `site.title`、`site.description`
- [x] 11. 改 `site.author`、`site.timezone`（现为 `Asia/Shanghai`）
- [x] 12. 核对 `socials`（github + mail 已配好，可补 x/linkedin）
- [x] 13. 核对 `editPost.url` 指向 `.../edit/main/`
- [x] 14. 完善 `src/content/pages/about.md` 的个人介绍

### C 检查结论

| 项  | 位置                                                    | 当前值                                                          | 结论                                                 |
| --- | ------------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------- |
| 8   | `site.url`                                              | `https://george-102.github.io/daily-delta/`                     | ✅ 与仓库 `George-102/daily-delta` 的 Pages 地址一致 |
| 9   | `base`                                                  | `/daily-delta`                                                  | ✅ 与仓库名一致                                      |
| 10  | `site.title` / `site.description`                       | `Daily Delta` / `A daily increment of code, books, and ideas.`  | ✅ 已改，无模板残留                                  |
| 11  | `site.author` / `timezone` / `lang` / `dir` / `profile` | `Christopher Chang` / `Asia/Shanghai` / `en` / `ltr` / about 页 | ✅ 全部正确                                          |
| 12  | `socials`                                               | github + mail                                                   | ✅ 图标文件存在，可访问性标题自动生成                |
| 13  | `editPost`                                              | `enabled: true`，`.../George-102/daily-delta/edit/main/`        | ✅ 地址格式正确                                      |
| 14  | `about.md`                                              | 已重写                                                          | ✅ frontmatter `title` / `description` 齐全          |

### C 检查发现的问题（不属于 C 的修复范围，但会影响上线）

> 以下 5 条均已在 D / E 阶段处理完毕，保留原始记录以便回溯。

1. **阻塞级：`pnpm build` 失败**。`_releases/astro-paper-6.md` 引用 `assets/AstroPaper-v6.png`、`_color-schemes/predefined-color-schemes.mdx` 引用 `assets/*.png`，但这些图片已被删除，构建报 `[ImageNotFound]`。必须完成 D 的 15–17 步才能构建成功。→ **已解决**：删除全部模板示例内容后构建恢复。
2. **风险：搜索结果的链接缺 `base` 前缀**。Pagefind 索引里的 URL 形如 `/posts/xxx/`，而线上真实地址是 `/daily-delta/posts/xxx/`，默认 UI 直接使用该 URL，点击会 404。需在 `src/pages/search.astro` 的 `PagefindUI` 中加 `processResult` 补 `base`，或改用一个带 base 的页面结构。→ **已解决**：见「D 顺带修掉的两个隐患」第 2 条。
3. **冗余：`public/robots.txt` 与 `src/pages/robots.txt.ts` 同时生成 `/robots.txt`**。两者内容目前一致，建议删掉 `public/robots.txt`，只保留动态生成的版本（D 第 23 步）。→ **已解决**。
4. **模板残留：首页 hero 文案**。`src/pages/index.astro` 里仍是 `Mingalaba` + AstroPaper 的自我介绍 + 指向 `satnaing/astro-paper#readme` 的 README 链接，需要换成你自己的站点介绍。→ **已解决**：见「D 顺带修掉的两个隐患」第 1 条。
5. **模板残留：`public/google5805ff1a5c362673.html`** 是模板作者的 Search Console 验证文件（D 第 22 步）；`README.md`、`.github/FUNDING.yml` 也仍是模板作者的内容。→ **已解决**：验证文件确认是你自己的并保留；`README.md`、`.github/FUNDING.yml` 未改动（不影响站点运行，可日后自行处理）。

## D. 清理模板痕迹

- [x] 15. 删除示例文章目录 `src/content/posts/_releases/`（已删除）
- [x] 16. 删除示例文章目录 `src/content/posts/_color-schemes/`（已删除）
- [x] 17. 删除 `src/content/posts/examples/` 里的 4 篇示例（已删除）
- [x] 18. 删除根目录下的模板教程文章（`adding-new-post.mdx` 等 8 篇，已删除）
- [x] 19. 删除模板图片 `src/assets/images/` 里的 AstroPaper-v3~v5 和 `forrest-gump-quote.png`（已删除，模板图片目录清空）
- [x] 20. ~~替换 `public/default-og.jpg`~~ → 改为启用 `features.dynamicOgImage: true`，由 `src/pages/og.png.ts` 在构建时用站点标题动态生成 1200×630 分享图，不再需要静态图
- [x] 21. ~~替换 `public/favicon.svg`~~ → 改用你自己的 `public/favicon.png`：`Layout.astro` 已改为 `<link rel="icon" type="image/png">` + `apple-touch-icon`，并已把原图从 1024×1024 / 1.4MB 压到 512×512 / 12KB
- [x] 22. `public/google5805ff1a5c362673.html` 是你自己的 Search Console 验证文件，已保留（线上地址 `https://george-102.github.io/daily-delta/google5805ff1a5c362673.html`）
- [x] 23. 已删除静态 `public/robots.txt`，只保留 `src/pages/robots.txt.ts` 动态生成（Sitemap 地址自动带 `site.url`）
- [x] 24. 已写第一篇测试文章 `src/content/posts/hello-daily-delta.md`，frontmatter 含 `title` / `description` / `pubDatetime` / `tags`（另加 `featured` 进首页推荐位），正文覆盖代码块文件名标签、提示块、表格等样式

### D 顺带修掉的两个隐患

1. **首页 hero 文案**（`src/pages/index.astro`）：`Mingalaba` → `Daily Delta`，AstroPaper 自我介绍 → 你自己的博客简介，README 外链 → 站内 `about` 页。
2. **搜索结果链接缺 `base` 前缀**（`src/pages/search.astro`）：给 `PagefindUI` 加了 `processResult`，把索引里的 `/posts/xxx/` 补成 `/daily-delta/posts/xxx/`，否则线上点搜索结果会 404。

## E. 提交前检查

- [x] 25. 运行 `pnpm format`（已跑通，`format:check` 全部通过；另把 Search Console 验证文件加进 `.prettierignore`，避免被格式化改动）
- [x] 26. 运行 `pnpm lint`（已跑通，无报错）
- [x] 27. 确认 `.gitignore` 已忽略 `dist/`、`.astro/`、`public/pagefind`（已重写为分组版本，并新增 `.gitattributes`，见下方说明）
- [x] 28. `git add -A` 后按 Conventional Commits 提交（已拆成 4 个原子提交，见下表）
- [x] 29. `git push origin main`（已推送，`d335a11..210f6d9`）

### E 阶段构建验证结果

`pnpm build` 全绿：`astro check` 0 errors / 0 warnings / 0 hints，动态 OG 图生成正常（`dist/og.png` 1200×630），Pagefind 索引 + 复制脚本正常（索引 1 页，即测试文章；`about` 页按模板设计不进索引）。

### E 阶段提交记录

| commit    | 类型          | 内容                                                                                 |
| --------- | ------------- | ------------------------------------------------------------------------------------ |
| `3eacf56` | `chore(repo)` | 新增 `.gitattributes`，重写 `.gitignore`，`.prettierignore` 排除 Search Console 文件 |
| `867f207` | `chore`       | 删除模板示例文章 / 模板图片 / 冗余静态文件，首页 hero 改写，Pagefind 结果补 `base`   |
| `557efcf` | `feat(post)`  | 新增第一篇测试文章 `hello-daily-delta.md`                                            |
| `210f6d9` | `docs`        | 新增本部署计划文档                                                                   |

### E 阶段关于 `.gitignore` / `.gitattributes` 的说明

`.gitignore` 按类别分组重写，覆盖：构建产物（`dist/`、`.astro/`）、搜索索引（`public/pagefind/`）、依赖（`node_modules/`、`.pnpm-store/`）、缓存（`.eslintcache`、`*.tsbuildinfo`）、日志（`*.log`）、环境变量（`.env*`，但保留 `!.env.example`）、IDE（`.idea/`）、操作系统（`.DS_Store`、`Thumbs.db`、`Desktop.ini`）、覆盖率（`coverage/`）。`.vscode/` **故意不忽略**——模板自带的 `extensions.json`、`launch.json` 和代码片段对团队协作有用。

新增 `.gitattributes` 是必须的，不是可选优化。本机 `core.autocrlf=true`，提交时 Git 一直警告 `LF will be replaced by CRLF`。若不声明，全新克隆后工作区所有文件变成 CRLF，而 `.prettierrc` 里 `endOfLine` 是 `"lf"`，`pnpm format:check` 会整片报错，pre-commit 钩子也可能失效。文件内容：

- `* text=auto eol=lf` —— 仓库内强制统一 LF
- 图片 / 字体标记 `binary` —— 禁止换行符转换破坏文件
- `/public/google*.html -text` —— Search Console 验证文件保持字节原样

## F. GitHub 网页侧设置（手动操作）

- [x] 30. 仓库 → Settings → Pages → Build and deployment → Source 选 **GitHub Actions**（部署已成功，说明此项早已配置正确）
- [x] 31. 到 Actions 页面看 Deploy workflow 是否变绿（run `45179969116`：`build` + `deploy` 两个 job 均 success）
- [x] 32. 访问 `https://george-102.github.io/daily-delta/` 验证上线（200）
- [x] 33. 逐个点开各页面验证（结果见下表）

### F 阶段线上验证结果

| 地址                                    | 状态          |
| --------------------------------------- | ------------- |
| `/`（首页）                             | 200           |
| `/posts/hello-daily-delta/`（测试文章） | 200           |
| `/tags/`、`/tags/meta/`                 | 200           |
| `/archives/`                            | 200           |
| `/search/`                              | 200           |
| `/about/`                               | 200           |
| `/rss.xml`、`/sitemap-index.xml`        | 200           |
| `/og.png`（动态分享图）                 | 200           |
| `/favicon.png`                          | 200           |
| `/google5805ff1a5c362673.html`          | 200，内容原样 |
| `/pagefind/pagefind.js`（搜索索引）     | 200           |

## G. 日常写作循环（以后每次都这样）

核心四步：**写 → 看 → 提交 → 自动部署**。

### 1. 新建文章

放在 `src/content/posts/` 下，**文件名直接决定 URL**：

```
src/content/posts/my-first-note.md    →  /daily-delta/posts/my-first-note/
src/content/posts/notes/day-12.md     →  /daily-delta/posts/notes/day-12/
```

子目录会拼进 URL，可用作分类（如 `notes/`、`essays/`）。文件名建议全小写英文 + 连字符——`getPostPaths.ts` 里文件名是直接当 slug 用的，不做转写，中文文件名会变成 `%E4%B8%AD` 这类编码。

### 2. 写 frontmatter

必填三项（`description` 在 schema 里没有 default，也必须写）：

```yaml
---
title: "文章标题"
description: "一句话摘要，会显示在首页卡片和 SEO meta 里"
pubDatetime: 2026-11-24T21:00:00+08:00
tags: ["code", "reading"]
---
```

可选字段（定义见 `src/content.config.ts`）：

| 字段                                   | 作用                                       |
| -------------------------------------- | ------------------------------------------ |
| `featured: true`                       | 进首页 Featured 区，排在普通文章前面       |
| `draft: true`                          | **永不发布**，连 `pnpm dev` 都不显示       |
| `modDatetime`                          | 显示「更新于」，不影响排序                 |
| `ogImage: ./cover.png`                 | 自定义分享图，不写则走动态生成的 `/og.png` |
| `hideToc: true` / `hideEditPost: true` | 关掉目录 / 关掉「编辑此页」按钮            |
| `canonicalURL`                         | 原文链接（转载场景）                       |
| `timezone`                             | 单篇覆盖站点默认的 `Asia/Shanghai`         |

### 3. 本地预览

```powershell
pnpm dev
```

打开 `http://localhost:4321/daily-delta/`（必须带 `/daily-delta` 这段 base）。

两个模板特有机制：

- **草稿**：`draft: true` 在 dev 里也不显示（`postFilter.ts` 第一行就是 `if (data.draft) return false`）。想本地预览草稿，让文件名以 `_` 开头（如 `_my-draft.md`）——loader 的 glob 是 `**/[^_]*`，下划线开头会被整个忽略，发布时去掉 `_` 即可。
- **定时发布**：`pubDatetime` 写未来日期时，dev 里能看见，但线上构建会藏起来，到那天（按 `Asia/Shanghai` 的日历日，不精确到时刻）才出现。可以提前囤稿。

### 4. 提交 + 推送

```powershell
git add -A
git commit -m "post: add my first note"
git push origin main
```

推上去 GitHub Actions 自动构建部署，不需要手动跑 `pnpm build`。提交信息按 Conventional Commits（仓库有 `cz.yaml`，也支持 `pnpm cz` 交互式写），常用前缀：`post:`（新增文章）、`fix:`（修错别字/断链）、`feat:`（加功能）、`chore:`（改配置/升依赖）、`docs:`（改文档）。

### 5. 什么时候才需要本地跑 build

日常写作**不需要**。只有以下情况建议跑一次（**记得先清代理**，见下方「坑 3」）：

```powershell
$env:HTTP_PROXY=''; $env:HTTPS_PROXY=''; $env:http_proxy=''; $env:https_proxy=''
pnpm build
```

- 改动了配置、布局、组件之后（`astro check` 会做类型检查）
- 想确认搜索功能（Pagefind 索引是构建时生成的；dev 下搜索页会提示你先 build 一次，`copy-pagefind.mjs` 会把索引复制到 `public/pagefind` 供 dev 使用）
- 推送前想彻底确认没问题

### 6. 部署后确认

推送后去 Actions 页面看 Deploy workflow 变绿，然后访问 `https://george-102.github.io/daily-delta/`。Actions 失败时日志会直接写明是哪个页面构建报错。

### 7. 高频日常小事

| 我想…            | 做法                                                                                                  |
| ---------------- | ----------------------------------------------------------------------------------------------------- |
| 发一篇新文章     | 建 `xxx.md` → 写 frontmatter → `pnpm dev` 看 → push                                                   |
| 存草稿           | 文件名加 `_` 前缀                                                                                     |
| 定时发布         | `pubDatetime` 写未来日期                                                                              |
| 加文章配图       | 图片放文章同目录，正文 `![说明](./cover.png)`，`ogImage: ./cover.png` 也支持本地路径                  |
| 加标签           | `tags` 数组里写即可，标签页自动生成；大小写/空格会被 slugify（`Meta` 与 `meta` 归到同一标签页）       |
| 改站点信息       | 只改 `astro-paper.config.ts`（标题、简介、社交链接、每页篇数、搜索/目录开关）                         |
| 手机上临时补一句 | 文章页有「编辑此页」按钮，指向 `.../edit/main/src/content/posts/xxx.md`，网页改完 commit 也会触发部署 |
| 本地验证全流程   | 清代理 + `pnpm build`                                                                                 |

---

## 四个必须注意的坑

1. **本地 Node 版本低于 22.23.4 会直接失败**。`deploy.yml` 使用 Node 24 + pnpm 11.3.0，本地最好对齐。
2. **仓库名、`base`、`site.url` 三者必须一致**。一旦不一致，站点能构建成功，但线上会出现大量断链和样式丢失——这是新手用 AstroPaper 上 GitHub Pages 最常见的翻车点。
3. **本机开着系统代理时 `pnpm build` 会失败**（报 `RangeError: Offset is outside the bounds of the DataView`）。原因不是代码问题：Astro 在预渲染阶段起了一个本地 HTTP 服务（`http://[::]:端口`）供 `og.png` 拉取字体，而你的 `HTTP_PROXY` 把访问 `localhost` 的请求也转给了代理，代理返回 502 + 空响应体，字体解析随即崩溃。临时规避：

   ```powershell
   $env:HTTP_PROXY=''; $env:HTTPS_PROXY=''; $env:http_proxy=''; $env:https_proxy=''
   pnpm build
   ```

   或者把 `localhost` / `127.0.0.1` 加进代理软件的绕过列表。GitHub Actions 里没有代理，所以线上不受影响。

4. **换行符与 Prettier 的冲突**。本机 `core.autocrlf=true`，Git 会把 LF 转成 CRLF，而 `.prettierrc` 要求 `endOfLine: "lf"`。靠 `.gitattributes` 里的 `* text=auto eol=lf` 兜住。如果哪天 `pnpm format:check` 突然整片报 `Delete CR` 或 `Replace CRLF with LF`，先查两件事：`.gitattributes` 还在不在、`git config core.autocrlf` 是不是又变回 `true`。别急着跑 `pnpm format` 全量重写——那会把 CRLF 真实写进文件，反而更乱。
