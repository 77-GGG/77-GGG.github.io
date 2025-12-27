# iFlow 上下文文档

## 项目概述

这是一个基于 Jekyll 的静态网站项目，名为 "The Minimum Viable Model" (MVM)，是一个专注于人工智能趋势和概念的技术博客。该项目使用 Adam Blog 2.0 主题，一个专为 GitHub Pages 优化的 Jekyll 主题。

### 技术栈
- **静态站点生成器**: Jekyll
- **CSS 预处理器**: Sass/SCSS
- **构建工具**: Gulp
- **包管理器**: npm (Node.js), Bundler (Ruby)
- **部署平台**: GitHub Pages
- **IPFS 支持**: 已配置 IPFS 镜像

### 主要特性
- 响应式设计，支持移动设备
- 深色/浅色主题切换（自动/手动模式）
- SEO 优化
- 代码语法高亮（多种主题可选）
- MathJax 数学公式支持
- 评论系统支持（Disqus/Utteranc）
- 邮件订阅（MailChimp）
- 搜索功能
- 标签云
- 无限滚动
- 自动生成站点地图
- 社交媒体分享

## 项目结构

```
/Users/qiguo/Documents/pages/
├── _config.yml              # Jekyll 配置文件
├── _includes/               # 可重用的 HTML 片段
├── _layouts/                # 页面布局模板
├── _pages/                  # 静态页面
├── _posts/                  # 博客文章
├── assets/                  # 静态资源
│   ├── css/                 # 样式文件
│   │   ├── main.css         # 主样式文件
│   │   ├── sass/            # SCSS 源文件
│   │   └── highlighter/     # 代码高亮主题
│   ├── js/                  # JavaScript 文件
│   ├── img/                 # 图片资源
│   │   ├── branding/        # 品牌相关图片
│   │   ├── posts/           # 文章配图
│   │   └── ...
│   └── fonts/               # 字体文件
├── index.html               # 首页
├── 404.html                 # 404 页面
├── gulpfile.js              # Gulp 构建脚本
├── package.json             # Node.js 依赖
├── Gemfile                  # Ruby 依赖
└── README.md                # 项目说明
```

## 构建和运行

### 本地开发

1. **安装依赖**:
   ```bash
   # 安装 Ruby 依赖
   bundle install
   
   # 安装 Node.js 依赖
   npm install
   ```

2. **运行开发服务器**:
   ```bash
   # 使用 Gulp 启动开发服务器（推荐）
   npm run dev
   
   # 或者直接使用 Jekyll
   bundle exec jekyll serve
   ```

3. **构建生产版本**:
   ```bash
   bundle exec jekyll build
   ```

### Gulp 任务

- `gulp sass`: 编译 SCSS 文件并添加浏览器前缀
- `gulp img`: 压缩图片
- `gulp jekyll-build`: 构建 Jekyll 站点
- `gulp jekyll-rebuild`: 重新构建并刷新浏览器
- `gulp browser-sync`: 启动浏览器同步服务
- `gulp watch`: 监听文件变化并自动构建
- `gulp default`: 默认任务，启动浏览器同步和文件监听

### 部署

项目配置为自动部署到 GitHub Pages，每次推送到主分支时会自动构建和部署。

## 配置说明

### 站点基本配置 (_config.yml)

- `title`: 网站标题
- `description`: 网站描述
- `url`: 网站基础 URL
- `baseurl`: 子路径（如有）
- `logo`: 品牌标志
- `night_mode`: 深色模式设置（"auto"/"manual"/"on"/"off"）

### 作者和联系方式

- `author`: 作者名称
- `email`: 电子邮件
- `social`: 各种社交媒体账户（GitHub、LinkedIn、Twitter 等）

### 功能配置

- `paginate`: 每页显示文章数量
- `comments`: 评论系统配置
- `analytics`: Google Analytics 配置
- `mailchimp`: 邮件订阅配置

## 内容管理

### 添加新文章

1. 在 `_posts` 目录中创建新文件，命名格式为 `YYYY-MM-DD-title.md`
2. 添加前置元数据 (front matter):
   ```yaml
   ---
   layout: post
   title: 文章标题
   date: YYYY-MM-DD HH:MM:SS
   description: 文章描述
   img: 图片路径
   tags: [标签1, 标签2]
   author: 作者名
   toc: yes  # 是否显示目录
   ---
   ```
3. 使用 Markdown 编写文章内容

### 修改样式

主要样式文件位于 `assets/css/main.css`，使用 CSS 变量定义主题颜色：
```css
html {
  --accent: #DB504A;      /* 主色调 */
  --main: #326273;        /* 主要颜色 */
  --text: #201E1E;        /* 文本颜色 */
  --background: #ffffff;  /* 背景色 */
}
```

深色主题通过 `html[data-theme="dark"]` 选择器覆盖。

## 自定义选项

### 代码高亮主题

在 `_config.yml` 中配置：
```yaml
highlight_theme: syntax-base16.monokai.dark
```

可用主题位于 `assets/css/highlighter/` 目录。

### 数学公式

在文章前置元数据中添加 `mathjax: true` 即可启用 MathJax 支持。

### 点击推文功能

在文章中使用 `<tweet>推文内容</tweet>` 标签创建可点击推文块。

## 开发约定

1. **文件命名**: 使用小写字母和连字符
2. **图片优化**: 使用 Gulp 自动压缩图片
3. **CSS 组织**: 使用 SCSS 模块化组织样式
4. **响应式设计**: 确保所有页面适配移动设备
5. **SEO 优化**: 每个页面都应包含适当的 meta 标签

## 故障排除

1. **构建失败**: 检查 Ruby 和 Node.js 依赖是否正确安装
2. **样式不更新**: 确保运行 `gulp sass` 或 `gulp watch`
3. **本地预览问题**: 清除 `_site` 目录并重新构建
4. **GitHub Pages 部署问题**: 检查 `_config.yml` 配置和 GitHub 仓库设置