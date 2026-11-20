# cvPages — 极简个人主页（GitHub Pages）

一个零依赖的单页学术主页：一个 `index.html` + 一个 `stylesheet.css`，没有框架、没有构建步骤，
打开就能看，推到 GitHub 就能上线。

## 本地预览

直接双击 `index.html` 即可，或者起一个本地服务器（链接路径更接近线上环境）：

```bash
cd /Users/qiguo/cvPages
python3 -m http.server 8765
```

然后打开 <http://127.0.0.1:8765>。

## 要改的地方（都在 `index.html` 里）

| 位置 | 改什么 |
| --- | --- |
| `<title>`、`og:title`、`og:description` | 姓名与一句话介绍（也决定搜索结果和分享卡片的标题） |
| `<h1>`、`.subtitle` | 姓名、身份、单位 |
| `.portrait` 的 `src` / `alt` | 照片（见下一节） |
| `.links` | Email、GitHub、ORCID（CV 链接暂时移除，见下文） |
| 第一个 `<section>` | 自我介绍，两到三段就够，别写长 |
| `News` 段 | 现在内容留白，格式模板在注释里，取消注释即可 |
| `Publications` 段 | 内容留白；按时间倒序，自己的名字用 `<span class="me">` 包起来加粗 |
| `Education` / `Experience` | 每条是一个 `.entry`，左边内容、右边时间，复制一段即可增加 |
| `<footer>` | 更新时间 |

姓名目前是 **Guo Qi / 祁果**，一共出现在五处，改名字时别漏：
`<title>`、`<meta name="description">`、`og:title`、`<h1>`（中文名在 `<span class="name-zh">` 里）、照片的 `alt`。

### 暂时留白的段落

`News`、`Publications`、`Experience` 现在只有标题、没有内容，可用的 HTML 模板以注释形式
放在各段里（`<!-- ... -->`）。要填内容时把注释里的示例取消注释、改文字即可。

这三段现在都带 `hidden` 属性（`<section hidden>`），所以页面上不显示、也不占位置。
填好内容后，把对应 `<section>` 上的 `hidden` 删掉即可显示。

### CV 文件（暂时没有）

页头现在不放 CV 链接。等你有了 PDF，把文件放到 `files/cv.pdf`，然后在 `.links` 里加回：

```html
<a href="files/cv.pdf">CV</a>
```

### 换照片

页面右上角是一张圆形头像，读的是 `images/portrait.jpg`（现在是你的照片，480×480）。
换照片把它覆盖掉即可，HTML 不用改：

- 建议正方形，长边至少 480px，脸部居中偏上；
- 现在显示尺寸是 120px（手机 84px），大图不会被浪费太多；
- 想换尺寸就改 `stylesheet.css` 里的 `.portrait { width / height }`；
- 圆形裁切由 `border-radius: 50%` + `object-fit: cover` 完成，非正方形照片也不会变形；
- 不喜欢照片被轻微降饱和，删掉 `.portrait` 里的 `filter: saturate(0.92);` 即可；
- 不想要照片，把整个 `<img class="portrait" ...>` 删掉，文字会自动占满整行。

### 换配色

`stylesheet.css` 顶部的 `:root` 里集中了全部颜色和栏宽（`--measure`），
改这几行就够：`--ink` 正文、`--accent` 链接、`--bg` 背景、`--rule` 分隔线。
深色模式由 `prefers-color-scheme` 自动跟随系统。

## 部署到 GitHub Pages

站点已经在线上：**<https://77-ggg.github.io>**，仓库是
<https://github.com/77-GGG/77-GGG.github.io>（用户站，仓库名必须是 `<账号名>.github.io`，
地址前缀由账号名决定，没得选）。

原来那个 Jekyll 站点已被本目录的内容覆盖（`main` 分支强制推送），替换前的完整备份在
`.backup/`（`old-site.bundle` 是 git 历史，`old-site-files.tar.gz` 是文件），确认不要了可以直接删。

日常更新只有三步：

```bash
cd /Users/qiguo/cvPages
git add -A && git commit -m "update"
git push
```

推送后 1 分钟左右自动重新发布（Pages → Deploy from a branch，`main` / `(root)`）。
本目录带一个空的 `.nojekyll` 文件，用来跳过 Jekyll 处理、按原样发布静态文件。

### 可选：绑定自己的域名

在仓库根目录加一个 `CNAME` 文件，内容写你的域名（如 `www.example.com`），
然后在 DNS 里给该域名加一条 CNAME 指向 `<你的用户名>.github.io`，
最后在 Settings → Pages 勾选 Enforce HTTPS。

## 之后想扩展

- 材料变多（论文列表、博客）→ 迁到 Jekyll 主题 [minimal-light](https://github.com/yaoyao-liu/minimal-light)，用 Markdown 维护。
- 想多页（Publications / Projects / CV 分开）→ [Academic-Homepage-Template](https://github.com/Arvid-pku/Academic-Homepage-Template)。
