# 流萤 Firefly 项目结构说明

> **Firefly（流萤）** 是一款基于 **Astro 7** 的静态博客主题：页面用 Astro 组件（类 HTML 模板）编写，核心逻辑用 **TypeScript**，交互组件用 **Svelte 5** 岛屿架构，文章内容用 **Markdown / MDX**，样式用 Tailwind CSS 4 + Stylus。
>
> 本文以结构树形式列出每个文件夹和文件并注释其作用。表格版见同目录下 `Firefly项目结构说明.xlsx`。
>
> 生成日期：2026-10-02

---

## 项目文件树

```text
Firefly-master/
│
├── package.json                  # 项目清单：依赖列表 + pnpm dev / build / new-post 等脚本命令
├── pnpm-lock.yaml                # 依赖版本锁定，保证安装可复现
├── pnpm-workspace.yaml           # pnpm 工作区声明
├── astro.config.mjs              # Astro 主配置：集成插件（Svelte、MDX、Tailwind、Swup 等）、
│                                 #   Markdown 管道（remark/rehype 插件）、可选 Cloudflare 适配器
├── tsconfig.json                 # TypeScript 配置，含 @/ 路径别名
├── svelte.config.js              # Svelte 编译配置
├── postcss.config.mjs            # PostCSS 配置
├── biome.json                    # Biome 格式化 / lint 规则（代替 Prettier + ESLint）
├── pagefind.yml                  # Pagefind 静态搜索配置
├── vercel.json                   # Vercel 部署配置（与 wrangler.jsonc 二选一）
├── wrangler.jsonc                # Cloudflare Workers 部署配置
├── _frontmatter.json             # VS Code Frontmatter 插件配置，写文章 frontmatter 时提供补全
├── README.md / README.en.md      # 项目说明（中文 / 英文）
├── CONTRIBUTING.md               # 贡献指南
├── LICENSE                       # 开源协议
├── AGENTS.md / CLAUDE.md         # 给 AI 编程助手看的项目规范
│
├── src/                          # ★ 全部源码
│   ├── content.config.ts         # 内容集合 schema：文章 / 说说 / 项目各需哪些 frontmatter 字段
│   ├── env.d.ts                  # Astro 环境类型声明
│   ├── global.d.ts               # 全局类型声明
│   ├── astro-modules.d.ts        # Astro 模块类型声明
│   │
│   ├── assets/                   # 源码管理的图片（构建时自动压缩优化）
│   │   └── images/
│   │       ├── DesktopWallpaper/ # 桌面端壁纸 6 张
│   │       ├── MobileWallpaper/  # 移动端壁纸 6 张
│   │       └── logo/             # 站点 logo：firefly-dark.png / firefly-light.png
│   │
│   ├── components/               # ★ 组件库（按功能分 9 类）
│   │   ├── README.md             # 组件目录说明
│   │   ├── firefly-mdx.ts        # MDX 文章可用的组件注册
│   │   ├── common/               # 通用积木组件
│   │   │   ├── Badge.astro             # 徽章
│   │   │   ├── ButtonLink.astro        # 链接按钮
│   │   │   ├── ButtonTag.astro         # 标签按钮
│   │   │   ├── CoverImage.astro        # 文章封面图
│   │   │   ├── DropdownItem.astro / .svelte   # 下拉菜单项
│   │   │   ├── DropdownPanel.astro / .svelte  # 下拉面板
│   │   │   ├── FilterControls.svelte   # 列表筛选控件
│   │   │   ├── FloatingButton.astro    # 浮动按钮
│   │   │   ├── GridSkeleton.svelte     # 网格加载骨架屏
│   │   │   ├── Icon.svelte             # 通用图标
│   │   │   ├── ImageWrapper.astro      # 图片包装（优化 / 灯箱接入）
│   │   │   ├── Markdown.astro          # Markdown 片段渲染
│   │   │   ├── PageJump.svelte         # 跳页输入
│   │   │   ├── Pagination.astro        # 分页
│   │   │   ├── PioMessageBox.astro     # 看板娘消息弹窗
│   │   │   ├── StepItem.astro / Steps.astro   # 步骤条
│   │   │   ├── TabGroup.svelte / TabNav.svelte # 选项卡
│   │   │   ├── Timeline.astro / TimelineItem.astro  # 时间线
│   │   │   ├── WidgetLayout.astro      # 侧栏小部件通用外壳
│   │   │   └── mdx-icons.ts            # MDX 图标映射
│   │   ├── layout/               # 页面骨架组件
│   │   │   ├── Navbar.astro            # 顶部导航栏
│   │   │   ├── HeaderTopRow.astro      # 导航栏顶行（logo / 开关）
│   │   │   ├── NavMenuPanel.astro      # 移动端导航抽屉
│   │   │   ├── DropdownMenu.astro      # 导航下拉菜单
│   │   │   ├── CategoryBar.astro       # 分类栏
│   │   │   ├── Footer.astro            # 页脚
│   │   │   ├── SideBar.astro / SidebarColumn.astro  # 侧栏及其列容器
│   │   │   ├── PostCard.astro          # 文章卡片（列表页核心）
│   │   │   ├── PostMeta.astro          # 文章元信息（日期 / 分类 / 字数）
│   │   │   ├── PostPage.astro          # 文章详情页组装
│   │   │   ├── PostStats.astro         # 文章统计信息
│   │   │   ├── BannerHomeTextOverlay.astro      # 首页横幅文字层
│   │   │   ├── BannerPostMetaOverlay.astro      # 文章页横幅信息层
│   │   │   ├── WallpaperSection.astro  # 壁纸背景区
│   │   │   └── ConfigCarrier.astro     # 配置传递载体（服务端 → 浏览器）
│   │   ├── widget/               # 侧栏小部件
│   │   │   ├── Profile.astro           # 博主资料卡
│   │   │   ├── Announcement.astro      # 公告
│   │   │   ├── Calendar.astro          # 日历
│   │   │   ├── Categories.astro        # 分类云
│   │   │   ├── Tags.astro              # 标签云
│   │   │   ├── SiteInfo.astro          # 站点信息
│   │   │   ├── SiteStats.astro         # 站点统计（文章数 / 字数）
│   │   │   ├── Music.astro             # 侧栏音乐
│   │   │   ├── Dynamic.astro / DynamicSidebar.svelte  # 最新说说
│   │   │   ├── Advertisement.astro     # 广告位
│   │   │   ├── SidebarTOC.astro        # 侧栏文章目录
│   │   │   └── SpineModel.astro        # Spine 看板娘（侧栏版）
│   │   ├── controls/             # 浮动 / 页面控件
│   │   │   ├── BackToTop.astro         # 返回顶部
│   │   │   ├── BackToHome.astro        # 返回主页
│   │   │   ├── BackToComment.astro     # 回到评论
│   │   │   ├── LightDarkSwitch.svelte  # 明暗模式切换
│   │   │   ├── Search.svelte           # 搜索框（Pagefind）
│   │   │   ├── FloatingControls.astro  # 悬浮控件容器
│   │   │   ├── FloatingTOC.astro / ImmersiveTOC.astro  # 悬浮 / 沉浸式文章目录
│   │   │   ├── ImmersiveReading.astro  # 沉浸式阅读模式
│   │   │   ├── DisplaySettingsIntegrated.svelte  # 显示设置面板
│   │   │   ├── ArchivePanel.astro      # 归档面板
│   │   │   └── ScrollDownIndicator.astro  # 下滑提示箭头
│   │   ├── features/             # 大型功能组件
│   │   │   ├── MusicPlayer.astro / MusicPlayerView.astro / MusicManager.astro  # 音乐播放器
│   │   │   ├── BackgroundPlayer.astro  # 背景音乐播放
│   │   │   ├── Live2DWidget.astro / SpineModel.astro  # Live2D / Spine 看板娘
│   │   │   ├── SakuraEffect.astro      # 樱花飘落特效
│   │   │   ├── WavesEffect.astro       # 底部波浪特效
│   │   │   ├── TypewriterText.astro    # 打字机文字
│   │   │   ├── EncryptedPost.astro / EncryptedContent.astro  # 文章 / 内容密码加密
│   │   │   ├── FancyboxManager.astro   # 图片灯箱管理
│   │   │   ├── GithubCardManager.astro # GitHub 仓库卡片管理
│   │   │   ├── KatexManager.astro      # KaTeX 数学公式
│   │   │   ├── CodeGroupManager.astro  # 代码选项卡组
│   │   │   └── FontSetup.astro         # 字体加载
│   │   ├── comment/              # 评论系统（5 套可切换）
│   │   │   ├── index.astro             # 按 commentConfig 渲染其中一套
│   │   │   ├── Giscus.astro            # GitHub Discussions 评论
│   │   │   ├── Waline.astro            # Waline 评论
│   │   │   ├── Twikoo.astro            # Twikoo 评论
│   │   │   ├── Artalk.astro            # Artalk 评论
│   │   │   └── Disqus.astro            # Disqus 评论
│   │   ├── analytics/            # 统计分析（按配置接入其一）
│   │   │   ├── GoogleAnalytics.astro   # Google Analytics
│   │   │   ├── UmamiAnalytics.astro    # Umami
│   │   │   ├── MicrosoftClarity.astro  # Microsoft Clarity
│   │   │   └── La51Analytics.astro     # 51.la
│   │   ├── misc/                 # 杂项组件
│   │   │   ├── License.astro           # 文章许可协议声明
│   │   │   ├── RecommendedPost.astro   # 相关 / 推荐文章
│   │   │   ├── SeriesNav.astro         # 系列文章导航
│   │   │   └── SharePoster.svelte      # 生成分享海报
│   │   └── pages/                # 专用页面的功能组件（与 src/pages 路由配套）
│   │       ├── AdvancedSearch.svelte   # 高级搜索面板
│   │       ├── bangumi/                # 追番页：BangumiGrid / BangumiSection / Card (Svelte)
│   │       ├── bilibili/               # B 站收藏页：BilibiliGrid / BilibiliCard / BilibiliDetailModal
│   │       ├── mal/                    # MyAnimeList 页：MalGrid / MalSection / Card
│   │       ├── vndb/                   # VNDB 游戏库页：VndbGrid / VndbSection / Card
│   │       ├── dynamic/                # 说说页：DynamicFeed / DynamicItem / DynamicGallery /
│   │       │                           #   DynamicInlineComments（.astro + .ts 逻辑）
│   │       ├── gallery/                # 相册页：AlbumCard / PhotoCard
│   │       └── projects/               # 项目页：ProjectCard
│   │
│   ├── config/                   # ★ 全站配置中心（每个功能一个文件）
│   │   ├── index.ts              # 统一导出全部配置
│   │   ├── README.md             # 配置说明文档
│   │   ├── FooterConfig.html     # 页脚内容（直接写 HTML 片段）
│   │   ├── siteConfig.ts         # 站点名称 / URL / 语言 / 横幅标语
│   │   ├── profileConfig.ts      # 博主资料：头像 / 昵称 / 简介 / 社交链接
│   │   ├── navBarConfig.ts       # 导航栏菜单
│   │   ├── sidebarConfig.ts      # 侧栏模块与顺序
│   │   ├── displaySettingsConfig.ts  # 显示设置面板选项
│   │   ├── backgroundWallpaper.ts    # 壁纸配置（桌面 / 移动端）
│   │   ├── coverImageConfig.ts   # 文章封面图策略
│   │   ├── effectsConfig.ts      # 樱花 / 波浪等特效开关
│   │   ├── pioConfig.ts          # 看板娘：模型 / 位置 / 提示语
│   │   ├── musicConfig.ts        # 音乐播放器歌单
│   │   ├── commentConfig.ts      # 评论系统选择与参数
│   │   ├── analyticsConfig.ts    # 统计平台选择与 ID
│   │   ├── announcementConfig.ts # 公告内容
│   │   ├── booknavConfig.ts      # 书签导航数据
│   │   ├── expressiveCodeConfig.ts   # 代码块高亮配置
│   │   ├── fontConfig.ts         # 字体加载与子集
│   │   ├── mermaidConfig.ts      # Mermaid 图表主题
│   │   ├── plantumlConfig.ts     # PlantUML 服务器与主题
│   │   ├── licenseConfig.ts      # 文章许可协议
│   │   ├── friendsConfig.ts      # 友链数据
│   │   ├── galleryConfig.ts      # 相册配置
│   │   ├── sponsorConfig.ts      # 赞赏收款码
│   │   └── dynamicConfig.ts      # 说说数据源配置
│   │
│   ├── types/                    # ★ TypeScript 类型定义（与 config/ 同名文件一一对应）
│   │   ├── analyticsConfig.ts / announcementConfig.ts / booknavConfig.ts /
│   │   │   commentConfig.ts / coverImageConfig.ts / displaySettingsConfig.ts /
│   │   │   dynamicConfig.ts / effectsConfig.ts / expressiveCodeConfig.ts /
│   │   │   fontConfig.ts / friendsConfig.ts / galleryConfig.ts /
│   │   │   immersiveReadingConfig.ts / licenseConfig.ts / mermaidConfig.ts /
│   │   │   musicConfig.ts / navBarConfig.ts / pioConfig.ts / plantumlConfig.ts /
│   │   │   profileConfig.ts / sidebarConfig.ts / siteConfig.ts / sponsorConfig.ts /
│   │   │   backgroundWallpaper.ts    # 以上均对应 config/ 下同名配置的类型
│   │   ├── config.ts             # 通用配置类型
│   │   ├── bangumi.ts / bilibili.ts / mal.ts / vndb.ts  # 追番 / 收藏页数据类型
│   │   ├── nsfw.ts / waves.ts / sakura-worker.ts        # NSFW 标记 / 波浪特效 / Worker 消息类型
│   │   └── iconify-svelte-offline.d.ts  # 图标离线声明
│   │
│   ├── constants/                # 构建脚本生成的常量（自动生成，勿手改）
│   │   ├── constants.ts          # 通用常量
│   │   ├── icon.ts               # 生成的 SVG 图标数据
│   │   ├── icons-data.json       # 图标原始数据
│   │   ├── lqips.json            # 全站图片的模糊占位 base64（LQIP）
│   │   └── github-card-data.json # GitHub 卡片数据
│   │
│   ├── content/                  # ★ Markdown 内容
│   │   ├── posts/                # 博客文章
│   │   │   ├── firefly.md            # 主题介绍文章
│   │   │   ├── markdown-tutorial.md / markdown-extended.md       # Markdown 教程与扩展语法演示
│   │   │   ├── markdown-mermaid.md / markdown-plantuml.md        # Mermaid / PlantUML 图表演示
│   │   │   ├── code-examples.md / katex-math-example.md / video.md  # 代码 / 公式 / 视频示例
│   │   │   ├── mdx-example.mdx       # MDX（文中嵌组件）示例
│   │   │   ├── encrypted-demo.md     # 密码加密文章示例
│   │   │   ├── draft.md              # 草稿示例
│   │   │   ├── guide/                # 主题使用指南系列（index.md + 布局 / 双链教程 + 配图）
│   │   │   └── images/               # 文章示例图片 14 张（avif）
│   │   ├── dynamic/              # 说说 / 动态（4 条示例 md，文件名即时间戳）
│   │   ├── projects/             # 项目展示（example-project.md / firefly.md + 配图）
│   │   └── spec/                 # 特殊页面正文：about.md 关于 / friends.mdx 友链 / guestbook.md 留言板
│   │
│   ├── i18n/                     # ★ 多语言
│   │   ├── i18nKey.ts            # 所有界面文案的键名定义
│   │   ├── translation.ts        # 翻译加载入口
│   │   └── languages/            # en / ja / ko / ru / zh_CN / zh_TW 六种语言
│   │
│   ├── icons/                    # SVG 图标目录（供 astro-icon 使用，当前为空）
│   │
│   ├── layouts/                  # ★ 布局
│   │   ├── Layout.astro          # 全站骨架：head、字体、全局特效、脚本
│   │   └── MainGridLayout.astro  # 带侧栏的正文布局
│   │
│   ├── pages/                    # ★ 路由层（文件即 URL）
│   │   ├── [...page].astro       # 首页与文章分页（/1、/2…）
│   │   ├── 404.astro             # 404 页
│   │   ├── about.astro           # 关于页（正文取自 content/spec/about.md）
│   │   ├── archive.astro         # 归档页
│   │   ├── friends.astro         # 友链页
│   │   ├── guestbook.astro       # 留言板
│   │   ├── search.astro          # 搜索页
│   │   ├── sponsor.astro         # 赞赏页
│   │   ├── booknav.astro         # 书签导航页
│   │   ├── bangumi.astro / bilibili.astro / myanimelist.astro / vndb.astro  # 追番 / 收藏页
│   │   ├── rss.astro / rss.xml.ts / atom.astro / atom.xml.ts   # RSS / Atom 订阅源
│   │   ├── llms.txt.ts           # 给 AI 爬虫看的 llms.txt
│   │   ├── robots.txt.ts         # 爬虫协议
│   │   ├── posts/[...slug].astro     # 文章详情路由
│   │   ├── categories/index.astro    # 分类页
│   │   ├── tags/index.astro          # 标签页
│   │   ├── series/index.astro        # 系列文章页
│   │   ├── dynamic/index.astro / comments.astro   # 说说页及其评论页
│   │   ├── gallery/index.astro / [album].astro    # 相册列表 / 单个相册
│   │   ├── projects/index.astro / [slug].astro    # 项目列表 / 项目详情
│   │   ├── api/allPostMeta.json.ts   # JSON 接口：全站文章元数据
│   │   ├── api/dynamic.json.ts       # JSON 接口：说说数据
│   │   └── og/[...slug].ts           # 每篇文章动态生成社交分享卡片图
│   │
│   ├── plugins/                  # ★ 自写 remark / rehype Markdown 插件
│   │   ├── remark-reading-time.mjs        # 计算阅读时长
│   │   ├── remark-excerpt.js              # 提取文章摘要
│   │   ├── remark-wiki-link.js            # [[双链]] 语法支持
│   │   ├── remark-image-grid.js           # 图片网格语法
│   │   ├── remark-mermaid.js / rehype-mermaid.mjs          # Mermaid 图表预处理与渲染
│   │   ├── remark-plantuml.js / rehype-plantuml.mjs        # PlantUML 图表
│   │   ├── remark-directive-rehype.js     # directive 指令通用解析
│   │   ├── rehype-component-github-card.mjs  # GitHub 仓库卡片指令
│   │   ├── rehype-diagram-panzoom.mjs     # 图表缩放平移
│   │   ├── rehype-email-protection.mjs    # 邮箱地址防爬混淆
│   │   ├── rehype-external-links.mjs      # 外链新窗口打开 + 安全属性
│   │   ├── rehype-figure.mjs              # 图片转 figure / 图注
│   │   ├── rehype-image-referrerpolicy.mjs  # 图片 referrer 策略（防盗链）
│   │   ├── diagram-panzoom-script.js      # 注入浏览器的图表缩放脚本
│   │   ├── plantuml-encoder.js / plantuml-render-script.js / plantuml-theme-switch.js  # PlantUML 运行时
│   │   └── utils/                         # diagramConstants.js / extractText.js 插件内部工具
│   │
│   ├── styles/                   # ★ 样式
│   │   ├── main.css              # 样式入口
│   │   ├── variables.styl        # 颜色 / 尺寸变量（Stylus）
│   │   ├── layout-base.css / layout-styles.css    # 布局骨架样式
│   │   ├── navbar.css / toc.css / tags.css / categories.css  # 导航栏 / 目录 / 标签 / 分类
│   │   ├── markdown.css / markdown-extend.styl    # 正文 Markdown 样式与扩展
│   │   ├── expressive-code.css   # 代码块
│   │   ├── fancybox-custom.css / photoswipe.css   # 灯箱
│   │   ├── banner-home.css / banner-post.css / banner-title.css  # 各页横幅
│   │   ├── transition.css / waves.css / immersive-reading.css / display-settings.css
│   │   ├── custom-scrollbar.css / scrollbar.css   # 滚动条
│   │   └── pages/                # 专用页样式：booknav / card-grid / common / dynamic(-comments) /
│   │                             #   friends / gallery-album / media-grid / sponsor
│   │
│   ├── utils/                    # ★ 50 个工具函数（名字即用途）
│   │   ├── banner-utils.ts / banner-visibility-utils.ts   # 横幅数据处理与显隐
│   │   ├── bilibili-utils.ts / mal-utils.ts / vndb-utils.ts / github-card-utils.ts  # 外部平台数据拉取
│   │   ├── booknav-utils.ts / projects-utils.ts / gallery-utils.ts  # 书签 / 项目 / 相册数据处理
│   │   ├── build-platform.ts     # 构建平台判断（CI 环境）
│   │   ├── content-overflow-utils.ts / content-utils.ts   # 内容溢出处理 / 文章集合读取统计
│   │   ├── crypto-utils.ts       # 内容密码加密
│   │   ├── date-utils.ts         # 日期格式化（dayjs 封装）
│   │   ├── display-settings-utils.ts / setting-utils.ts   # 显示设置与用户偏好读写
│   │   ├── dynamic-utils.ts / memos-adapter.ts  # 说说数据源适配
│   │   ├── email-utils.ts        # 邮箱防爬混淆
│   │   ├── failed-covers.ts      # 封面拉取失败兜底
│   │   ├── feed-utils.ts         # RSS / Atom 生成辅助
│   │   ├── fetch-dedup.ts        # 请求去重
│   │   ├── floating-panel-utils.ts  # 浮动面板交互
│   │   ├── fontHelper.ts         # 字体变量收集（配合构建裁剪字体）
│   │   ├── fullscreen-wallpaper-utils.ts  # 全屏壁纸
│   │   ├── grid-layout-utils.ts  # 网格布局计算
│   │   ├── icon-loader.ts        # 图标加载
│   │   ├── image-utils.ts / lqip-utils.ts  # 图片处理 / 模糊占位图
│   │   ├── immersive-reading-utils.ts  # 沉浸阅读模式
│   │   ├── language-utils.ts / navbar-i18n.ts  # 多语言辅助
│   │   ├── layout-init.ts / layout-utils.ts  # 布局初始化与辅助
│   │   ├── nav-menu-utils.ts / navigation-utils.ts / url-utils.ts  # 导航与 URL 处理
│   │   ├── nsfw-utils.ts         # NSFW 内容标记 / 模糊
│   │   ├── page-toggle-utils.ts / swup-transitions.ts  # 页面切换与转场
│   │   ├── responsive-utils.ts   # 响应式判断
│   │   ├── schema-image.ts / schema-utils.ts  # SEO 结构化数据
│   │   ├── scroll-utils.ts       # 滚动监听
│   │   ├── sidebar-effective-utils.ts / site-config-utils.ts  # 侧栏生效配置 / 站点配置辅助
│   │   ├── toc-shared.ts / toc-utils.ts  # 文章目录生成与联动
│   │   ├── touch-copy-utils.ts   # 移动端触屏复制代码
│   │   └── waves-draw.ts         # 波浪特效绘制
│   │
│   └── workers/
│       └── sakura.worker.ts      # Web Worker：樱花特效粒子运算，避免阻塞 UI
│
├── scripts/                      # ★ 构建与脚手架脚本（Node / tsx）
│   ├── new-post.js               # pnpm new-post：新建文章骨架
│   ├── new-dynamic.js            # pnpm new-d：新建说说
│   ├── generate-lqips.ts         # 生成全站图片模糊占位图 → src/constants/lqips.json
│   ├── subset-fonts.ts           # 按实际用字裁剪中文字体子集
│   ├── run-pagefind.ts           # 生成 Pagefind 搜索索引
│   ├── generate-github-card-data.ts  # 拉取 GitHub 仓库数据 → github-card-data.json
│   ├── generate-vndb-covers.ts   # 拉取 VNDB 游戏封面
│   ├── minify-inline-scripts.ts  # 压缩内联脚本
│   ├── prune-pio-assets.ts       # 清理未使用的看板娘资源
│   ├── quarantine-bad-posts.mjs  # 隔离格式有问题的文章
│   ├── site-root.ts              # 项目根路径定位工具
│   └── subset-font.d.ts          # subset-font 类型声明
│
├── public/                       # ★ 原样拷贝的静态资源（不经构建处理）
│   ├── anime-list.json           # 追番页本地数据
│   ├── favicon/                  # 12 个多尺寸 favicon（亮色 / 暗色 / 彩色）
│   ├── assets/
│   │   ├── css/                  # highlight 主题色 + Twikoo 评论自定义样式
│   │   ├── js/                   # highlight.min.js / marked.min.js（动态页渲染用）
│   │   ├── fonts/                # GreatVibes 手写英文字体
│   │   ├── images/               # ad/ 广告图、effects/sakura.png 樱花贴图、sponsor/ 收款码
│   │   └── music/                # 背景音乐 mp3 + 专辑封面
│   ├── gallery/                  # 相册原图：firefly-2026/（urls.txt 图片清单 + 封面）、
│   │                             #   encrypted-test/（加密相册示例）
│   └── pio/                      # 看板娘资源
│       ├── README.md             # 使用说明
│       ├── models/live2d/snow_miku/  # Live2D 初音模型（贴图 + idle 动作）
│       ├── models/spine/firefly/     # Spine 流萤模型（64 张切片图 + 音频）
│       └── static/               # Spine 官方播放器 runtime（js / css）
│
├── docs/                         # 文档
│   ├── README.zh-TW.md / README.ja.md / README.ko.md  # 繁中 / 日 / 韩 说明
│   └── images/                   # README 配图
│
└── .github/                      # GitHub 配置
    ├── workflows/biome.yml       # CI：Biome lint 检查
    ├── workflows/build.yml       # CI：构建测试
    ├── workflows/deploy.yml      # CI/CD：自动部署
    └── ISSUE_TEMPLATE/           # Issue 模板：bug 报告 / 功能建议 / 自定义
```

---

## 补充说明

### 自动生成的目录（不参与手改）

| 目录 | 说明 |
|---|---|
| `node_modules/` | `pnpm install` 安装的依赖包 |
| `dist/` | `pnpm build` 的构建产物，部署用 |
| `.astro/` | Astro 开发期缓存（字体、集合类型提示） |
| `src/constants/` 下的 json / icon.ts | 构建脚本产物，每次构建自动更新 |

### 数据流向（一条文章的旅程）

```text
src/content/posts/*.md ──content.config.ts 校验──▶ remark/rehype 插件管道
    ──▶ src/pages/posts/[...slug].astro 渲染 ──▶ src/layouts/MainGridLayout.astro 布局
    ──▶ dist/ 静态 HTML（+ Pagefind 索引 + LQIP 占位 + 字体子集）
```

### 读代码的建议路径

1. `src/config/siteConfig.ts` —— 看懂配置如何驱动全站
2. `src/layouts/MainGridLayout.astro` —— 页面骨架如何拼装
3. `src/components/layout/PostCard.astro` —— 一个典型组件的写法
4. `src/content/posts/guide/` —— 主题自带的写作指南示例文章
5. `astro.config.mjs` —— 插件与 Markdown 管道全貌
