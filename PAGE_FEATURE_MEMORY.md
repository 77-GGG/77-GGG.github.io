# 页面与功能映射记忆文件

## 1. 全局页面骨架
- 全站基础布局：`_layouts/default.html`
  - 注入全局 `<head>`：`_includes/head.html`
  - 页面主体：`{{ content }}`（由具体 layout/page 决定）
  - 全局返回顶部按钮：`.top`
  - 全局脚本入口：`_includes/javascripts.html`
  - 全局统计：`_includes/analytics.html`

- 页面容器布局：
  - 首页/列表页容器：`_layouts/home-page.html`
  - 菜单页容器（about/archive/tags/tag）：`_layouts/menu-page.html`
  - 文章详情页：`_layouts/post.html`

## 2. 页面路由 -> 功能 -> 代码映射

### 2.1 首页 `/`
- 页面文件：`index.html`
- 使用布局：`layout: home-page` -> `_layouts/home-page.html`
- 主要功能：
  - 分页文章卡片列表（`paginator.posts`）
  - 每篇卡片显示封面图 + 标题 + 元信息
  - 页面下方分页导航
  - 标签云总览
  - 全局搜索浮层
- 关键代码：
  - 列表渲染：`index.html`
  - 卡片元信息：`_includes/post_meta.html`
  - 分页按钮：`_includes/pagination.html`
  - 标签聚合：`_includes/get-site-tags.html` + `_includes/tag_cloud.html`
  - 搜索框 UI：`_includes/search.html`

### 2.2 文章详情页 `/_posts/*` 生成的文章 URL
- 页面来源：`_posts/*.md`
- 使用布局：`layout: post` -> `_layouts/post.html`
- 主要功能：
  - 顶部封面、标题、日期、阅读时长
  - 正文 markdown 渲染
  - 侧边栏：文章标签 + 分享按钮
  - 可选 TOC（目录）
  - 可选 GitHub 按钮
  - 作者信息、近期文章、Newsletter、评论区
  - `<tweet>...</tweet>` 自动转“click to tweet”卡片
  - 复制链接按钮
  - 可选 MathJax / Mermaid
- 关键代码：
  - 页面拼装：`_layouts/post.html`
  - 侧边栏与分享：`_includes/sidebar.html` + `_includes/share-icons.html`
  - TOC 容器：`_includes/sidebar-toc.html` + `_includes/toc.html`
  - 文章元信息：`_includes/post_meta.html`
  - 作者/最近文章/订阅/评论：
    - `_includes/author-box.html`
    - `_includes/recent-post.html`
    - `_includes/newsletter.html`
    - `_includes/comments.html`
  - 评论 curtain、复制链接、tweet 标签转化：`_includes/javascripts.html`（post 分支脚本）
  - TOC 滚动高亮 Gumshoe：`_includes/javascripts.html`（`if page.toc`）
  - MathJax：`_includes/javascripts.html`（`if page.mathjax`）
  - Mermaid：`_includes/javascripts.html`（`site.extensions.mermaid_enable` + `page.mermaid`）

### 2.3 全部文章页 `/archive.html`
- 页面文件：`_pages/archive.html`
- 使用布局：`layout: menu-page`
- 主要功能：
  - 初始显示前 3 篇文章摘要
  - 内联搜索框（页面内）
  - 下拉滚动自动加载更多文章（无限滚动）
- 关键代码：
  - 页面结构：`_pages/archive.html`
  - 无限滚动脚本：`assets/js/infinite-jekyll.js`
  - 全量文章 URL 数据源：`all-posts.json`
  - 加载动画：`_includes/spinner.html`

### 2.4 单标签页 `/tag.html?tag=xxx`
- 页面文件：`_pages/tag.html`
- 使用布局：`layout: menu-page`
- 主要功能：
  - 根据 URL 参数 `tag` 显示对应标签区块
  - 初始显示该标签下前 3 篇
  - 下拉滚动继续加载同标签文章
  - 底部标签云快速切换
- 关键代码：
  - 页面骨架与每个 tag 的隐藏区块：`_pages/tag.html`
  - 标签文章 URL 数据：`posts-by-tag.json`
  - 参数解析与动态显示：`assets/js/infinite-jekyll.js`
  - 标签云：`_includes/tag_cloud.html`

### 2.5 标签总览页 `/tags.html`
- 页面文件：`_pages/tags.html`
- 使用布局：`layout: menu-page`
- 主要功能：
  - 展示所有标签云
  - 每个标签下列出全部文章链接与元信息
- 关键代码：
  - 页面结构：`_pages/tags.html`
  - 标签收集排序：`_includes/get-site-tags.html`
  - 标签云：`_includes/tag_cloud.html`
  - 元信息：`_includes/post_meta.html`

### 2.6 关于页 `/about.html`
- 页面文件：`_pages/about.html`
- 使用布局：`layout: menu-page`
- 主要功能：
  - 作者头像、简介
  - 社交链接图标（Twitter/LinkedIn/GitHub/Email/Bandcamp/StackOverflow）
- 关键代码：
  - 页面结构：`_pages/about.html`
  - 数据来源：`_config.yml` 的 `author*` 与社交字段

### 2.7 404 页面 `/404.html`
- 页面文件：`404.html`
- 使用布局：`layout: home-page`
- 主要功能：
  - 响应式 404 图片
  - recent posts 推荐
  - 搜索输入与结果
- 关键代码：
  - 页面结构：`404.html`
  - recent posts：`_includes/recent-post.html`
  - 搜索数据源：`search.json`

### 2.8 IPFS 404 页面 `/ipfs-404.html`
- 页面文件：`ipfs-404.html`
- 使用布局：`layout: home-page`
- 主要功能：
  - 与普通 404 相同
  - 额外处理深层目录下的相对路径修复
- 关键代码：
  - 页面结构：`ipfs-404.html`
  - 路径修复逻辑：`_includes/javascripts.html` 中 `if page.url contains "ipfs-404"` 脚本

## 3. 站点级功能 -> 代码入口

### 3.1 导航菜单与移动端抽屉
- 代码：
  - 结构：`_includes/header.html`
  - 打开/关闭、遮罩交互、Esc 关闭：`assets/js/main.js`
- 涉及页面：所有使用 `home-page` / `menu-page` / `post` 的页面。

### 3.2 全站搜索
- 代码：
  - 搜索 UI：`_includes/search.html`
  - 初始化：`_includes/javascripts.html` 中 `SimpleJekyllSearch(...)`
  - 数据：`search.json`（包含标题、描述、正文文本）
  - 交互：`assets/js/main.js`（search icon 打开/关闭）
- 404/archive 页还有内联搜索框（不依赖浮层 UI）。

### 3.3 暗色模式
- 代码：
  - 开关 UI：`_includes/theme-toggle.html`
  - 逻辑（auto/manual/on/off + local/sessionStorage）：`_includes/javascripts.html`
  - 样式变量：`assets/css/main.css`
  - logo 切换：`site.logo` / `site.logo-dark`
- 配置：`_config.yml` -> `night_mode`, `logo-dark`。

### 3.4 无限滚动
- 代码：`assets/js/infinite-jekyll.js`
- 启用条件：页面 front matter `infinite: yes`（archive/tag）
- 数据源：`all-posts.json` 与 `posts-by-tag.json`
- 依赖标识：页面必须有 `.spinner` 与 `.post-list`。

### 3.5 评论系统
- 代码：`_includes/comments.html`
- 支持：`utteranc` / `disqus`
- curtain 展开控制：`toggle_comments()`（`_includes/javascripts.html`）
- 配置：`_config.yml` -> `comments`, `comments_opts.*`

### 3.6 社交分享与复制链接
- 代码：
  - 按钮模板：`_includes/share-icons.html`
  - 复制链接函数：`copyToClipboard()`（`_includes/javascripts.html`）
- 使用位置：文章页侧边栏和文末 inline 栏（`_includes/sidebar.html`）。

### 3.7 文章目录（TOC）
- 代码：
  - 目录生成：`_includes/toc.html`（Liquid 解析标题）
  - 容器：`_includes/sidebar-toc.html`
  - 滚动高亮 + 折叠：`_includes/javascripts.html`（Gumshoe + 自定义折叠）
- 启用条件：文章 front matter 设置 `toc: true`。

### 3.8 SEO / Schema / Analytics
- 代码：
  - 页面 SEO 元信息：`_includes/SEO.html`
  - 页面级 schema 元信息：`_includes/schema-meta.html`
  - Google Analytics：`_includes/analytics.html`
  - `<head>` 汇总：`_includes/head.html`

## 4. 数据与配置来源总览
- 站点全局：`_config.yml`
  - 站点标题、作者、社交链接、分页、评论、主题、分析等。
- 内容数据：`_posts/*.md`
  - 文章封面、标题、标签、分类、描述、日期、开关参数。
- 前端检索/动态数据：
  - `search.json`（搜索）
  - `all-posts.json`（archive 无限滚动）
  - `posts-by-tag.json`（tag 无限滚动）

## 5. 维护时的快速定位建议
1. 改“某个页面结构”：先看 `_pages/*.html` 与对应 `_layouts/*.html`。
2. 改“全站组件”（头部、底部、搜索、侧栏）：看 `_includes/*.html`。
3. 改“行为逻辑”（菜单、搜索弹层、滚动加载、暗色）：看 `assets/js/main.js`、`assets/js/infinite-jekyll.js`、`_includes/javascripts.html`。
4. 改“显示文案和站点身份”：看 `_config.yml`。
5. 改“样式”：优先改 `assets/css/sass/*`，注意与 `assets/css/main.css` 同步策略。

## 6. 已知耦合点（记忆重点）
- `tag.html` 的显示依赖 URL 参数 `tag` 与 DOM 中同 id 的隐藏区块。
- 无限滚动追加内容依赖每篇文章页中隐藏的 `.post` 摘要块（见 `_layouts/post.html` 顶部 hidden section）。
- 搜索功能强依赖页面存在 `#search-input` 和 `#results-container`。
- 404/IPFS 页通过 `calculate_relative_base` + `escape-404` + 专用脚本共同处理路径问题。
