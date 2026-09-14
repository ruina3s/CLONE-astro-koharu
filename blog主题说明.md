# Koharu 博客主题 — 文件功能详细分析
Koharu 是一个基于 Astro 5 + React 19 + TypeScript 构建的个人博客主题，支持多语言、AI 增强、Docker 部署等丰富功能。下面按功能模块对全部文件进行详细说明。

## 一、项目配置与构建工具（根目录）
| 文件                                               | 作用                                                                                                   |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------- |
| `package.json`                                   | 项目依赖管理，定义了 40+ 个核心依赖（Astro、React、Tailwind、Shiki、MDX 等）和 20+ 个开发脚本（dev/build/preview/各种 generate 脚本等） |
| `pnpm-lock.yaml`                                 | pnpm 包管理器的锁定文件，确保依赖版本一致性                                                                             |
| `pnpm-workspace.yaml`                            | pnpm 工作区配置，将 `cms/` 子项目纳入 monorepo 管理                                                                |
| `astro.config.mjs`                               | Astro 框架核心配置文件，集成 React、Tailwind、MDX、Sitemap、RSS、Open Graph 等插件，配置构建输出目录和站点元数据                       |
| `tailwind.config.mjs`                            | Tailwind CSS 配置文件，定义自定义颜色、字体、动画、主题变量等设计令牌                                                            |
| `tsconfig.json`                                  | TypeScript 编译配置，设置严格模式、路径别名（`@/*` 映射到 `src/*`）                                                       |
| `biome.json`                                     | Biome 代码格式化与 lint 配置，替代 ESLint/Prettier                                                              |
| `knip.json`                                      | Knip 工具配置，用于检测未使用的依赖和导出                                                                              |
| `cliff.toml`                                     | git-cliff 配置，用于根据 Git 提交历史自动生成 CHANGELOG                                                             |
| `components.json`                                | shadcn/ui 组件库配置，管理 UI 组件的安装和样式                                                                       |
| `.env.example`                                   | 环境变量模板，包含 `SITE_URL` 等必要配置示例                                                                         |
| `.gitignore` / `.dockerignore` / `.cursorignore` | 各工具的忽略文件配置                                                                                           |
| `.husky/`                                        | Git 钩子目录，用于提交前自动格式化代码                                                                                |

## 二、站点配置（config/）
| 文件                         | 作用                                                         |
| -------------------------- | ---------------------------------------------------------- |
| `config/site.yaml`         | **站点主配置文件**，定义网站标题、描述、作者、头像、社交链接、导航菜单、页脚信息、功能开关（评论/搜索/统计等） |
| `config/i18n-content.yaml` | **内容国际化配置**，定义各语言的内容翻译映射                                   |

## 三、页面路由（src/pages/）
Astro 的页面路由系统，每个 .astro 文件对应一个 URL 路由。
| 文件/目录                | 作用                                     |
| -------------------- | -------------------------------------- |
| `index.astro`        | **首页**，展示博客文章列表、Hero 区域、站点介绍           |
| `about.md`           | **关于页面**，Markdown 格式的自我介绍              |
| `music.md`           | **音乐页面**，展示个人音乐收藏/歌单                   |
| `friends.astro`      | **友链页面**，展示友情链接列表                      |
| `bangumi.astro`      | **番剧页面**，展示追番列表（对接 Bangumi API）        |
| `archives.astro`     | **归档页面**，按时间线展示所有文章                    |
| `404.astro`          | **404 错误页面**，自定义的未找到页面                 |
| `[seriesSlug].astro` | **系列页面动态路由**，根据系列 slug 展示系列文章集合        |
| `[lang]/`            | **多语言路由目录**，支持 `/en/`、`/zh/` 等前缀的国际化页面 |
| `post/`              | **文章详情页路由**，处理单篇文章的渲染                  |
| `posts/`             | **文章列表页路由**，分页展示文章列表                   |
| `categories/`        | **分类页面路由**，按分类聚合文章                     |
| `tags/`              | **标签页面路由**，按标签聚合文章                     |
| `rss.xml.ts`         | **RSS 订阅源生成**，输出 XML 格式的 RSS feed      |
| `rss/`               | **RSS 相关页面**，提供 RSS 订阅的 HTML 展示页       |
| `_shared/`           | **页面共享组件目录**，存放各页面复用的子组件               |

## 四、布局组件（src/layouts/）
| 文件                      | 作用                                                            |
| ----------------------- | ------------------------------------------------------------- |
| `Layout.astro`          | **根布局组件**，所有页面的基础 HTML 结构，包含 `<html>`、`<head>`、`<body>`       |
| `AppShell.astro`        | **应用外壳布局**，包裹页面内容，管理全局的侧边栏、导航等结构                              |
| `PageLayout.astro`      | **通用页面布局**，用于普通内容页面（关于、友链等）                                   |
| `TwoColumnLayout.astro` | **双栏布局**，文章详情页使用的左右分栏布局（内容区 + 目录/侧边栏）                         |
| `HeadMeta.astro`        | **头部元数据组件**，统一管理 SEO 相关的 `<meta>`、Open Graph、Twitter Card 等标签 |
| `BootScripts.astro`     | **启动脚本注入**，在页面加载时注入初始化 JavaScript（主题切换、字体加载等）                 |
| `page-meta.ts`          | **页面元数据工具函数**，辅助生成页面的 title、description 等 SEO 信息              |

## 五、UI 组件（src/components/）
这是项目中最大的组件目录，包含数十个可复用的 UI 组件。
### 5.1 核心布局组件
| 文件              | 作用                                 |
| --------------- | ---------------------------------- |
| `Header.astro`  | **顶部导航栏**，包含站点 Logo、导航菜单、搜索按钮、主题切换 |
| `Footer.astro`  | **页脚组件**，包含版权信息、备案号、社交链接           |
| `Sidebar.astro` | **侧边栏组件**，文章页右侧的目录、作者信息、相关文章       |
| `MobileNav.tsx` | **移动端导航菜单**，响应式的抽屉式导航              |
| `Search.tsx`    | **搜索组件**，全站文章搜索功能（支持快捷键 `/` 唤起）    |

### 5.2 文章相关组件
| 文件                    | 作用                                |
| --------------------- | --------------------------------- |
| `PostCard.astro`      | **文章卡片**，首页/列表页展示的文章预览卡片          |
| `PostList.astro`      | **文章列表**，文章列表的容器组件                |
| `PostContent.astro`   | **文章内容渲染**，负责渲染 Markdown/MDX 文章内容 |
| `PostMeta.astro`      | **文章元信息**，展示文章的发布日期、阅读时间、标签等      |
| `PostToc.tsx`         | **文章目录（TOC）**，右侧悬浮的文章大纲导航         |
| `RelatedPosts.astro`  | **相关文章推荐**，基于相似度算法推荐相关文章          |
| `ReadingProgress.tsx` | **阅读进度条**，顶部显示的阅读进度指示器            |
| `Comment.tsx`         | **评论系统组件**，集成 Twikoo 评论系统         |
| `SeriesNav.astro`     | **系列导航**，系列文章间的上一篇/下一篇导航          |

### 5.3 内容展示组件
| 文件                          | 作用                                  |
| --------------------------- | ----------------------------------- |
| `CodeBlock.tsx`             | **代码块组件**，基于 Shiki 的语法高亮，支持复制、行号、折叠 |
| `Image.astro` / `Image.tsx` | **图片组件**，支持懒加载、LQIP（低质量图片占位）、灯箱效果   |
| `Video.tsx`                 | **视频播放器组件**，自定义的视频播放 UI             |
| `AudioPlayer.tsx`           | **音频播放器组件**，音乐页面使用的播放器              |
| `LinkCard.astro`            | **链接卡片**，用于展示外部链接的预览卡片              |
| `FriendCard.astro`          | **友链卡片**，友链页面的单个友链展示                |
| `BangumiCard.tsx`           | **番剧卡片**，展示番剧信息的卡片                  |
| `Gallery.astro`             | **图片画廊**，图片集的网格展示                   |
| `Spoiler.tsx`               | **剧透折叠组件**，可折叠隐藏的内容块                |

### 5.4 交互组件
| 文件                    | 作用                            |
| --------------------- | ----------------------------- |
| `ThemeToggle.tsx`     | **主题切换按钮**，深色/浅色/跟随系统三种模式     |
| `BackToTop.tsx`       | **回到顶部按钮**，滚动到一定位置后显示         |
| `CopyButton.tsx`      | **复制按钮**，用于代码块、链接等的复制功能       |
| `Modal.tsx`           | **模态框组件**，通用的弹窗容器             |
| `Tooltip.tsx`         | **工具提示组件**，基于 Floating UI 的定位 |
| `Toast.tsx`           | **消息提示组件**，操作成功/失败的轻量提示       |
| `Announcement.tsx`    | **公告栏组件**，顶部可关闭的站点公告          |
| `ChristmasEffect.tsx` | **圣诞特效组件**，节日期间的雪花/装饰特效       |

### 5.5 特殊功能组件
| 文件              | 作用                              |
| --------------- | ------------------------------- |
| `Katex.astro`   | **数学公式渲染**，集成 KaTeX 渲染 LaTeX 公式 |
| `Mermaid.astro` | **Mermaid 图表渲染**，支持流程图、时序图等     |
| `Quiz.tsx`      | **测验/问卷组件**，文章内嵌的交互式测验          |
| `Meting.tsx`    | **MetingJS 音乐组件**，对接网易云音乐等平台的歌单 |
| `OgImage.tsx`   | **Open Graph 图片生成**，动态生成社交分享图   |
| `Sitemap.astro` | **站点地图页面**，人类可读的站点地图            |

## 六、工具库（src/lib/）
| 文件/目录                       | 作用                                                 |
| --------------------------- | -------------------------------------------------- |
| `utils.ts`                  | **通用工具函数**，字符串处理、数组操作、防抖节流等                        |
| `date.ts`                   | **日期处理工具**，格式化日期、相对时间（如"3 天前"）、时区转换                 |
| `slug.ts`                   | **Slug 生成工具**，将标题转换为 URL 友好的 slug                  |
| `route.ts`                  | **路由工具函数**，生成多语言路由路径                               |
| `content.ts`                | **内容处理工具**，读取和解析文章内容数据                             |
| `content-scanner.ts`        | **内容扫描器**，扫描 `src/content/blog/` 目录，建立文章索引         |
| `content-enhancer-utils.ts` | **内容增强工具**，为文章添加额外功能（如图片优化）                        |
| `image-enhancer.ts`         | **图片增强处理**，生成图片的多种尺寸、格式、LQIP                       |
| `lqip.ts`                   | **LQIP 生成工具**，生成低质量图片占位符（Base64 模糊图）               |
| `link-preview-enhancer.ts`  | **链接预览增强**，为外部链接生成预览卡片数据                           |
| `spoiler-enhancer.ts`       | **剧透内容增强处理**，处理文章中的剧透标记                            |
| `toc.ts` / `toc.test.ts`    | **目录生成工具** + 单元测试，从 Markdown 提取标题生成目录结构            |
| `heading-scroll-lock.ts`    | **标题滚动锁定**，点击目录时平滑滚动到对应标题                          |
| `collapse-animation.ts`     | **折叠动画工具**，手风琴/展开收起动画的实现                           |
| `sanitize.ts`               | **HTML 净化工具**，防止 XSS，清理不安全的 HTML 内容                |
| `css-string.ts`             | **CSS 字符串处理工具**，动态生成 CSS 样式字符串                     |
| `rss-utils.ts`              | **RSS 生成工具函数**，辅助生成 RSS XML 内容                     |
| `stats.ts`                  | **统计工具**，文章阅读量、字数统计等                               |
| `umami-stats.ts`            | **Umami 统计对接**，从 Umami 分析平台获取访问数据                  |
| `timezone.ts`               | **时区处理工具**，处理不同时区的时间显示                             |
| `playback-time-store.ts`    | **播放时间存储**，音频/视频播放进度的本地存储                          |
| `meting.ts`                 | **Meting API 对接**，获取网易云音乐等平台的歌单数据                  |
| `markdown/`                 | **Markdown 处理目录**，自定义 Markdown 解析插件                |
| `config/`                   | **配置解析目录**，读取和解析 `config/site.yaml`                |
| `content/`                  | **内容类型定义目录**，Astro Content Collections 的 schema 定义 |
| `bangumi/`                  | **Bangumi API 对接目录**，获取番剧数据                        |
| `crypto/`                   | **加密工具目录**，数据加密解密相关                                |
| `quiz/`                     | **测验系统目录**，测验的逻辑和数据处理                              |
| `seo/`                      | **SEO 工具目录**，生成 sitemap、robots.txt 等               |

## 七、自定义 Hooks（src/hooks/）
React 自定义 Hooks，封装可复用的状态逻辑。
| 文件                          | 作用                                      |
| --------------------------- | --------------------------------------- |
| `index.ts`                  | Hooks 统一导出入口                            |
| `useActiveHeading.ts`       | **活跃标题 Hook**，监听当前视口中的标题，用于目录高亮         |
| `useCurrentHeading.ts`      | **当前标题 Hook**，获取当前正在阅读的标题               |
| `useHeadingTree.ts`         | **标题树 Hook**，构建文章标题的层级树结构               |
| `useHeadingClickHandler.ts` | **标题点击处理**，点击标题时的滚动和 URL 更新             |
| `headingObserverStore.ts`   | **标题观察器状态**，管理多个标题的交叉观察状态               |
| `useTocController.ts`       | **目录控制器 Hook**，管理目录的展开/折叠状态             |
| `useScrollTrigger.ts`       | **滚动触发 Hook**，监听元素进入视口触发动画              |
| `useMediaQuery.ts`          | **媒体查询 Hook**，响应式断点检测（移动端/桌面端）          |
| `useIsDarkTheme.ts`         | **深色主题检测 Hook**，检测当前是否为深色模式             |
| `useIsMounted.ts`           | **挂载检测 Hook**，避免服务端渲染时的 hydration 不匹配   |
| `useCopyToClipboard.ts`     | **剪贴板复制 Hook**，封装复制到剪贴板功能               |
| `useControlledState.ts`     | **受控状态 Hook**，管理受控/非受控组件状态              |
| `useExpandedState.ts`       | **展开状态 Hook**，管理折叠面板的展开状态               |
| `useFloatingUI.ts`          | **浮动定位 Hook**，基于 Floating UI 的弹窗/下拉定位   |
| `useKeyboardShortcut.ts`    | **键盘快捷键 Hook**，监听全局键盘快捷键（如 `Ctrl+K` 搜索） |
| `useAudioPlayer.ts`         | **音频播放器 Hook**，封装音频播放控制逻辑               |
| `useVideoPlayer.ts`         | **视频播放器 Hook**，封装视频播放控制逻辑               |
| `useMediaPlayer.ts`         | **媒体播放器通用 Hook**，音频/视频播放的通用逻辑           |
| `usePlaybackTime.ts`        | **播放时间 Hook**，记录和恢复播放进度                 |
| `useRetimer.ts`             | **重定时器 Hook**，管理定时器的重置和清理               |
| `useTranslation.ts`         | **翻译 Hook**，国际化文本的获取和切换                 |
| `useZoomPan.ts`             | **缩放平移 Hook**，图片灯箱的缩放和拖拽功能              |
| `useBangumiData.ts`         | **番剧数据 Hook**，获取和缓存 Bangumi 番剧数据        |

## 八、状态管理（src/store/）
基于 Nanostores 的全局状态管理。
| 文件                      | 作用                         |
| ----------------------- | -------------------------- |
| `app.ts`                | **应用状态**，全局应用级别的状态         |
| `settings.ts`           | **用户设置状态**，主题、字体、布局等用户偏好设置 |
| `settings-constants.ts` | **设置常量**，设置项的默认值和可选值定义     |
| `locale.ts`             | **语言状态**，当前选中的语言           |
| `modal.ts`              | **模态框状态**，全局模态框的打开/关闭和内容管理 |
| `player.ts`             | **播放器状态**，全局音频/视频播放器状态     |
| `bgm.ts`                | **背景音乐状态**，背景音乐播放控制        |
| `announcement.ts`       | **公告状态**，公告的显示/隐藏和内容       |
| `christmas.ts`          | **圣诞特效状态**，节日特效的开关和配置      |

## 九、类型定义（src/types/）
| 文件                   | 作用                                         |
| -------------------- | ------------------------------------------ |
| `blog.ts`            | **博客相关类型**，文章、标签、分类、系列等数据类型                |
| `bangumi.ts`         | **番剧相关类型**，Bangumi API 返回的数据类型             |
| `announcement.ts`    | **公告类型**，公告数据结构                            |
| `umami-stats.ts`     | **Umami 统计类型**，访问统计数据类型                    |
| `custom-events.d.ts` | **自定义事件类型**，扩展 DOM 事件的类型定义                 |
| `navigator.d.ts`     | **Navigator 扩展类型**，扩展浏览器 Navigator 对象的类型   |
| `twikoo.d.ts`        | **Twikoo 类型**，评论系统的类型定义                    |
| `yaml.d.ts`          | **YAML 模块类型**，让 TypeScript 识别 `.yaml` 文件导入 |

## 十、常量定义（src/constants/）
| 文件/目录               | 作用                             |
| ------------------- | ------------------------------ |
| `site-config.ts`    | **站点配置常量**，从 YAML 读取并导出的站点配置对象 |
| `design-tokens.ts`  | **设计令牌常量**，颜色、间距、字体大小等设计系统常量   |
| `router.ts`         | **路由常量**，预定义的路由路径和参数           |
| `layout.ts`         | **布局常量**，布局相关的尺寸、断点等常量         |
| `category.ts`       | **分类常量**，预定义的文章分类              |
| `code-block.ts`     | **代码块常量**，代码高亮的默认配置            |
| `content-config.ts` | **内容配置常量**，内容集合的配置常量           |
| `friends-config.ts` | **友链配置常量**，友链页面的默认配置           |
| `enum.ts`           | **枚举常量**，项目中使用的枚举值             |
| `announcements.ts`  | **公告常量**，预定义的站点公告内容            |
| `anim/`             | **动画常量目录**，预定义的动画时长、缓动函数等      |

## 十一、国际化（src/i18n/）
| 文件/目录              | 作用                              |
| ------------------ | ------------------------------- |
| `config.ts`        | **i18n 配置**，定义支持的语言列表、默认语言、路由策略 |
| `index.ts`         | **i18n 入口**，导出翻译函数和配置           |
| `types.ts`         | **翻译类型定义**，翻译键值对的 TypeScript 类型 |
| `utils.ts`         | **i18n 工具函数**，语言切换、路径生成、文本翻译    |
| `content.ts`       | **内容翻译工具**，文章内容的翻译处理            |
| `content-types.ts` | **内容翻译类型**，内容翻译相关的类型定义          |
| `translations/`    | **翻译文件目录**，各语言的 JSON/YAML 翻译文件  |

## 十二、样式（src/styles/）
| 文件/目录         | 作用                       |
| ------------- | ------------------------ |
| `index.css`   | **样式入口文件**，导入所有全局样式      |
| `global/`     | **全局样式目录**，重置样式、基础样式、工具类 |
| `theme/`      | **主题样式目录**，深色/浅色主题的颜色变量  |
| `components/` | **组件样式目录**，特定组件的自定义样式    |
| `christmas/`  | **圣诞主题样式**，节日期间的专属样式     |

## 十三、内容数据（src/content/）
| 目录      | 作用                                                        |
| ------- | --------------------------------------------------------- |
| `blog/` | **博客文章目录**，存放所有 Markdown/MDX 格式的文章，每篇文章包含 frontmatter 元数据 |

## 十四、构建脚本（src/scripts/）
| 文件                        | 作用                              |
| ------------------------- | ------------------------------- |
| `generateLqips.ts`        | **生成 LQIP 脚本**，为所有图片生成低质量占位图    |
| `generateSimilarities.ts` | **生成相似度脚本**，计算文章间的相似度用于"相关文章"推荐 |
| `generateSummaries.ts`    | **生成摘要脚本**，使用 AI 为文章生成摘要        |
| `locale-filter.ts`        | **语言过滤脚本**，过滤特定语言的内容            |

## 十五、CLI 工具（scripts/koharu/）
这是一个独立的 CLI 工具集，使用 Ink（React for Terminal）构建的终端交互界面。
| 文件/目录          | 作用                             |
| -------------- | ------------------------------ |
| `new.tsx`      | **新建文章命令**，交互式创建新博客文章          |
| `generate.tsx` | **生成内容命令**，批量生成文章摘要、相似度、LQIP 等 |
| `update.tsx`   | **更新命令**，更新文章元数据、修复内容等         |
| `backup.tsx`   | **备份命令**，备份博客内容和配置             |
| `restore.tsx`  | **恢复命令**，从备份恢复博客数据             |
| `clean.tsx`    | **清理命令**，清理缓存、临时文件等            |
| `migrate.tsx`  | **迁移命令**，数据格式迁移/升级             |
| `list.tsx`     | **列表命令**，列出所有文章、标签、分类等         |
| `help.tsx`     | **帮助命令**，显示 CLI 使用帮助           |
| `components/`  | **终端 UI 组件**，复用的终端交互组件         |
| `hooks/`       | **终端 Hooks**，终端交互的状态管理         |
| `utils/`       | **终端工具函数**，文件操作、路径处理等          |
| `creators/`    | **内容创建器**，文章、页面等内容的生成逻辑        |
| `constants/`   | **CLI 常量**，CLI 的默认配置和常量        |

## 十六、CMS 子项目（cms/）
一个独立的内容管理系统，基于 Vite + React 构建。
| 文件               | 作用                           |
| ---------------- | ---------------------------- |
| `package.json`   | CMS 子项目的独立依赖                 |
| `server.ts`      | **CMS 服务端入口**，提供文章管理的 API 服务 |
| `index.html`     | CMS 前端入口 HTML                |
| `vite.config.ts` | Vite 构建配置                    |
| `tsconfig.json`  | CMS 的 TypeScript 配置          |
| `pnpm-lock.yaml` | CMS 的依赖锁定文件                  |
| `src/`           | CMS 前端源码目录                   |

## 十七、Docker 部署（docker/）
| 文件/目录                        | 作用                                                            |
| ---------------------------- | ------------------------------------------------------------- |
| `Dockerfile`                 | **Docker 镜像构建文件**，多阶段构建，基于 Node.js 镜像构建静态站点，再用 Nginx  serving |
| `docker-compose.yml`         | **Docker Compose 配置**，静态站点部署配置                                |
| `docker-compose.dynamic.yml` | **动态模式 Compose 配置**，支持 SSR/动态渲染的部署                            |
| `rebuild.sh`                 | **重建脚本**，自动拉取代码、构建、重启容器的脚本                                    |
| `smoke-dynamic.sh`           | **动态模式健康检查脚本**，验证动态服务是否正常                                     |
| `nginx/`                     | **Nginx 配置目录**，反向代理、缓存、Gzip 等配置                               |

## 十八、部署配置（deploy/）
| 目录        | 作用                                             |
| --------- | ---------------------------------------------- |
| `deploy/` | 各种部署平台的配置文件（Vercel、Netlify、Cloudflare Pages 等） |

## 十九、静态资源（public/）
| 目录        | 作用                                                |
| --------- | ------------------------------------------------- |
| `public/` | **静态资源目录**，图片、字体、favicon、robots.txt 等不经过构建直接复制的文件 |

## 二十、文档（docs/）
| 目录      | 作用                             |
| ------- | ------------------------------ |
| `docs/` | **项目文档目录**，包含使用指南、配置说明、API 文档等 |

## 二十一、测试（tests/）
| 目录       | 作用                                |
| -------- | --------------------------------- |
| `tests/` | **测试目录**，存放端到端测试（Playwright）和单元测试 |

## 二十二、GitHub 工作流（.github/）
| 目录         | 作用                                         |
| ---------- | ------------------------------------------ |
| `.github/` | **GitHub Actions 工作流**，CI/CD 自动化构建、测试、发布流程 |

## 二十三、AI 助手配置
| 文件          | 作用                                        |
| ----------- | ----------------------------------------- |
| `AGENTS.md` | **AI Agent 配置指南**，指导 AI 助手如何修改项目的规则       |
| `CLAUDE.md` | **Claude 专用配置**，给 Claude AI 的详细项目上下文和开发规范 |
| `.claude/`  | **Claude 相关配置目录**                         |

## 总结：项目架构图
```plain
Koharu Blog Theme
├── 配置层: config/ + 根目录配置文件
├── 路由层: src/pages/ (Astro 文件路由)
├── 布局层: src/layouts/ (页面骨架)
├── 组件层: src/components/ (UI 组件库)
├── 逻辑层: src/hooks/ + src/lib/ (业务逻辑 + 工具函数)
├── 状态层: src/store/ (全局状态)
├── 数据层: src/content/ + src/types/ (内容 + 类型)
├── 样式层: src/styles/ + tailwind.config.mjs (样式系统)
├── 国际化: src/i18n/ (多语言支持)
├── 构建脚本: src/scripts/ (构建时数据处理)
├── CLI 工具: scripts/koharu/ (终端交互工具)
├── CMS 系统: cms/ (独立内容管理)
├── 部署层: docker/ + deploy/ (容器化 + 多平台部署)
└── 静态资源: public/ (图片、字体等)
```
这个项目结构非常清晰，采用了现代前端最佳实践：Astro 岛架构（静态内容用 Astro，交互部分用 React）、Content Collections 管理文章、TypeScript 全类型覆盖、Monorepo 管理主项目 + CMS、Docker 容器化部署，以及完整的 CLI 工具链支持内容创作。
