# Daily Delta 博客配置与部署计划

> 目标：把基于 AstroPaper 模板的英文博客 `Daily Delta` 配置完成，并部署到 GitHub Pages。
>
> - 仓库：https://github.com/George-102/daily-delta
> - 线上地址：https://george-102.github.io/daily-delta/
> - 技术栈：Astro + TypeScript + Tailwind CSS + Pagefind
> - 包管理器：pnpm（Node ≥ 22.23.4）

## 进度总览

| 阶段 | 内容 | 状态 |
| --- | --- | --- |
| A | 本地环境（一次性） | ✅ 已完成 |
| B | 本地跑通验证 | ⚠️ 已完成，但 `pnpm build` 当前失败（缺图，见下方「C 检查发现的问题」） |
| C | 站点身份配置 | ✅ 已完成（about.md 已写好，配置文件 8–13 项已核对） |
| D | 清理模板痕迹 | ⬜ 待办 |
| E | 提交前检查 | ⬜ 待办 |
| F | GitHub 网页侧设置 | ⬜ 待办 |
| G | 日常写作循环 | ⬜ 待办 |

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

| 项 | 位置 | 当前值 | 结论 |
| --- | --- | --- | --- |
| 8 | `site.url` | `https://george-102.github.io/daily-delta/` | ✅ 与仓库 `George-102/daily-delta` 的 Pages 地址一致 |
| 9 | `base` | `/daily-delta` | ✅ 与仓库名一致 |
| 10 | `site.title` / `site.description` | `Daily Delta` / `A daily increment of code, books, and ideas.` | ✅ 已改，无模板残留 |
| 11 | `site.author` / `timezone` / `lang` / `dir` / `profile` | `Christopher Chang` / `Asia/Shanghai` / `en` / `ltr` / about 页 | ✅ 全部正确 |
| 12 | `socials` | github + mail | ✅ 图标文件存在，可访问性标题自动生成 |
| 13 | `editPost` | `enabled: true`，`.../George-102/daily-delta/edit/main/` | ✅ 地址格式正确 |
| 14 | `about.md` | 已重写 | ✅ frontmatter `title` / `description` 齐全 |

### C 检查发现的问题（不属于 C 的修复范围，但会影响上线）

1. **阻塞级：`pnpm build` 失败**。`_releases/astro-paper-6.md` 引用 `assets/AstroPaper-v6.png`、`_color-schemes/predefined-color-schemes.mdx` 引用 `assets/*.png`，但这些图片已被删除，构建报 `[ImageNotFound]`。必须完成 D 的 15–17 步才能构建成功。
2. **风险：搜索结果的链接缺 `base` 前缀**。Pagefind 索引里的 URL 形如 `/posts/xxx/`，而线上真实地址是 `/daily-delta/posts/xxx/`，默认 UI 直接使用该 URL，点击会 404。需在 `src/pages/search.astro` 的 `PagefindUI` 中加 `processResult` 补 `base`，或改用一个带 base 的页面结构。
3. **冗余：`public/robots.txt` 与 `src/pages/robots.txt.ts` 同时生成 `/robots.txt`**。两者内容目前一致，建议删掉 `public/robots.txt`，只保留动态生成的版本（D 第 23 步）。
4. **模板残留：首页 hero 文案**。`src/pages/index.astro` 里仍是 `Mingalaba` + AstroPaper 的自我介绍 + 指向 `satnaing/astro-paper#readme` 的 README 链接，需要换成你自己的站点介绍。
5. **模板残留：`public/google5805ff1a5c362673.html`** 是模板作者的 Search Console 验证文件（D 第 22 步）；`README.md`、`.github/FUNDING.yml` 也仍是模板作者的内容。

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
- [x] 27. 确认 `.gitignore` 已忽略 `dist/`、`.astro/`、`public/pagefind`（已确认，三项都在）
- [ ] 28. `git add -A` 后按 Conventional Commits 提交（仓库已配 `cz.yaml`）→ **交给你**
- [ ] 29. `git push origin main` → **交给你**

### E 阶段构建验证结果

`pnpm build` 全绿：`astro check` 0 errors / 0 warnings / 0 hints，动态 OG 图生成正常（`dist/og.png` 1200×630），Pagefind 索引 + 复制脚本正常（索引 1 页，即测试文章；`about` 页按模板设计不进索引）。

## F. GitHub 网页侧设置（手动操作）

- [ ] 30. 仓库 → Settings → Pages → Build and deployment → Source 选 **GitHub Actions**
- [ ] 31. 到 Actions 页面看 Deploy workflow 是否变绿
- [ ] 32. 访问 `https://george-102.github.io/daily-delta/` 验证上线
- [ ] 33. 逐个点开：文章页、标签页、归档、搜索、`rss.xml`、`sitemap-index.xml`

## G. 日常写作循环（以后每次都这样）

- [ ] 34. 新建 md 到 `src/content/posts/`
- [ ] 35. `pnpm dev` 本地看效果
- [ ] 36. `git commit` + `git push`，自动部署

---

## 三个必须注意的坑

1. **本地 Node 版本低于 22.23.4 会直接失败**。`deploy.yml` 使用 Node 24 + pnpm 11.3.0，本地最好对齐。
2. **仓库名、`base`、`site.url` 三者必须一致**。一旦不一致，站点能构建成功，但线上会出现大量断链和样式丢失——这是新手用 AstroPaper 上 GitHub Pages 最常见的翻车点。
3. **本机开着系统代理时 `pnpm build` 会失败**（报 `RangeError: Offset is outside the bounds of the DataView`）。原因不是代码问题：Astro 在预渲染阶段起了一个本地 HTTP 服务（`http://[::]:端口`）供 `og.png` 拉取字体，而你的 `HTTP_PROXY` 把访问 `localhost` 的请求也转给了代理，代理返回 502 + 空响应体，字体解析随即崩溃。临时规避：

   ```powershell
   $env:HTTP_PROXY=''; $env:HTTPS_PROXY=''; $env:http_proxy=''; $env:https_proxy=''
   pnpm build
   ```

   或者把 `localhost` / `127.0.0.1` 加进代理软件的绕过列表。GitHub Actions 里没有代理，所以线上不受影响。