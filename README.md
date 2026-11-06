# WeCream 的个人主页

从 [Alakazam 的 stardew 主页](https://alakazamc.github.io/) 扒下来改造的**骨架版**：版式、皮肤切换、唱片机、农场面板、三维樱花书屋全部沿用原版，原作者的书影音 / 游戏库 / 照片 / 文章 / 网易云歌单 / 留言板凭据已全部清空，只留可见的留白提示。

目标站点：`https://wecream.github.io/`（仓库 `WeCream.github.io`，`index.html` 在仓库根目录）。

---

## 本地预览（重要）

页面用了 ES module + importmap 加载三维场景，**双击 index.html 会因为 CORS 打不开 3D**。必须起一个本地静态服务器：

```powershell
cd "D:\personal web"
python -m http.server 8765
```

然后浏览器访问 <http://127.0.0.1:8765/>。

## 目录结构

```
D:\personal web\
├─ index.html                                   ← 主页（单文件，CSS 与大部分 JS 内联）
├─ font.css                                     ← 像素字体声明
├─ fusion-pixel-12px-proportional-zh_hans.woff2 ← 正文像素字体（917 KB）
├─ assets\
│  ├─ fonts\fusion-core.woff2                   ← 数字/拉丁像素字体（83 KB）
│  ├─ projects\placeholder-a.svg / -b.svg       ← 两张项目截图占位图
│  └─ toon\                                     ← 三维场景全部依赖（15 个文件，约 1.1 MB）
│     ├─ main.js  bookshop.js
│     ├─ vendor\core\{post,toon,sky,outline,util,palette,textures}.js
│     ├─ vendor\world\{trees,buildings}.js
│     └─ vendor\three\{three.module.js, three.core.min.js, addons\...}
├─ _source\                                     ← 扒下来的原件与构建脚本，**不要上传**
└─ README.md
```

`assets/` 里没有原作者的封面、插画、项目截图 —— 那些是私人收藏，已随内容一起清掉了，所以不会 404。

## 改名字

全站显示名统一用的 **WeCream**。要换成中文名（比如「王小明」），在编辑器里对 `WeCream` 做**全局替换**即可，涉及：

| 位置 | 大概在第几行 | 原文 |
|---|---|---|
| `<title>` | 15 | `WeCream · 个人主页` |
| SEO 块 | 1174–1180 | `description` / `og:title` / `og:url` |
| 招牌大标题 `h1` | 1212 | `<h1>WeCream</h1>` |
| 座右铭 | 1213 | `WeCream · fake it til u make it` |
| 一句话简介 | 1214 | `这里记录我的项目，和正在学着写下的东西。` |
| GitHub 按钮与各处跳转 | 1200、工坊卡片、页脚 | `https://github.com/WeCream` |

替换时**不要动** `https://github.com/WeCream` 里的用户名（除非你改了 GitHub 账号），否则链接会断。建议只替换 `WeCream` 的可见文案，链接手动确认一遍。

## 换内容：每一块该改哪里

| 区块 | 位置 | 现在是什么 |
|---|---|---|
| 概览的三个计数 | `id="board"` 里的 `.counts` | 2 个仓库 / 1 篇文章 / 0 张图像 |
| 工坊 · 精选项目 | `id="featured-projects"` 两张 `.pjcard` | 你两个真实仓库，截图是 `assets/projects/*.svg` 占位，说明文字写着「占位」 |
| 工坊 · 仓库列表 | `id="projects"` 里的 `.rgrid` | 同上两个仓库 |
| 最新文章 | `id="timeline"` | 一条真实文章卡片（`posts/2026-11-06-hello.html`）|
| 博物馆 | `id="museum"` | 只有一句留白提示（原站这里 1300 行书影游） |
| 相馆 | `id="gallery"` | 空白提示 |
| 专精 | `id="skills"` 的 `.stack` | Python 45% / C# 30% / JS 15% / HTML 10%，**是示例值**，改成你自己的 |
| 友链 | `id="friends"` | 一张「留给第一位友邻」的空卡 |
| 农场 | `id="farm"` + `#harvest-summary` | 年度统计 JSON 全是 0 |
| 唱片机 | `id="music"` + `#music-data` | `week` / `all` 是空数组，界面上写「还没有听歌记录」 |
| 留言板 | `id="comments-home"` | 静态说明文字，giscus 已摘掉 |

## 加一篇文章（三步）

站点没有后台，文章就是一个个 HTML 文件。已经放了一篇样板：`posts/2026-11-06-hello.html`。

**第一步：复制样板并改名**

```powershell
cd "D:\personal web\posts"
copy 2026-11-06-hello.html 2026-11-20-my-post.html
```

要改的是这几处（都在文件里，且都有注释/明显上下文）：

| 改什么 | 在哪 |
|---|---|
| 浏览器标题 | `<title>… · WeCream</title>` |
| 分享/搜索信息 | `<link rel="canonical">`、`<meta name="description">`、`og:title`、`og:description`、`og:url` |
| 文章大标题 | `<h1 class="arttitle">` |
| 日期与标签 | `<p class="artmeta"><span class="mi">2026-11-06</span> · …` |
| 正文 | `<div class="artbody">` 里面，用 `<h2>` / `<p>` / `<ul>` / `<blockquote>` / `<code>` 就行 |
| 署名 | `<div class="artfoot">` |

图片放到 `assets/` 下，正文里用相对路径引用，比如 `<img src="../assets/posts/2026-11-20-1.png" alt="…">`（`.artbody img` 的样式已经写好，不用手动设宽度）。

**第二步：在主页时间线加一条卡片**

打开 `index.html`，找到 `id="timeline"` 那一块，里面已经有一条 `.tl-item`。整段复制一份，改四处：

```html
<li class="tl-item">                                   <!-- lead 只给最新那条，旧的去掉 -->
  <div class="tl-when"><b>11.20</b><i>2026</i></div>    <!-- 月.日 / 年 -->
  <div class="tl-axis"><span class="tl-dot">…</span></div>
  <a class="tl-card" style="--i:1" href="posts/2026-11-20-my-post.html">
    <div class="tl-body">
      <b class="tl-title">你的标题</b>
      <p class="tl-exc">一两句摘要，两行以内最好看。</p>
      <div class="tl-tags"><span class="tl-tag">本站</span><span class="tl-tag">标签</span></div>
    </div>
    <svg class="ic sm tl-go" viewBox="0 0 16 16"><use href="#px-play"></use></svg>
  </a>
</li>
```

**第三步：改两个数字**

- 概览 `.counts` 里 `<b>1</b><span>篇文章</span>` 改成实际篇数
- 概览「最近写下」那句 `已写下 1 篇：…` 换成最新一篇的标题和链接

最后 `git add -A; git commit -m "post: …"; git push`，一两分钟后线上就有了。

> 注意：`index.html` 是由 `_source/build.py` 生成的。如果你以后重跑那个脚本，会把主页恢复成「只有一篇样板文章」的状态 —— 到时候把这段卡片改动同步回 `build.py` 的 `tl_new` 里，或者干脆以后只改 `index.html` 不再重跑脚本。

### 补内容时的两个坑

1. **项目截图**替换成真图时，`.pjshot img` 建议保持 **800×500**（4:2.5），否则卡片高度会跳。
2. **博物馆**要重新启用的话，卡片格式是 `.exc` + `data-kind="book|movie|game"`，筛选按钮带 `data-filter-kind`；这套 JS 还在页面里，加回元素就会自动生效。

## 接自己的留言板（giscus）

1. 仓库建好后，在 **Settings → General → Features** 勾上 **Discussions**。
2. 装 <https://github.com/apps/giscus>，只授权给 `WeCream.github.io` 这个仓库。
3. 打开 <https://giscus.app/>，填仓库名，拿到 `data-repo` / `data-repo-id` / `data-category` / `data-category-id`。
4. 把这段贴回 `id="comments-home"` 的 `.cmtbox` 里，删掉原来的留白提示：

```html
<script src="https://giscus.app/client.js"
        data-repo="WeCream/WeCream.github.io"
        data-repo-id="R_xxx"
        data-category="Announcements"
        data-category-id="DIC_xxx"
        data-mapping="pathname"
        data-reactions-enabled="1"
        data-theme="preferred_color_scheme"
        crossorigin="anonymous" async>
</script>
```

原站那套「皮肤跟着 giscus 换主题」的代码（`commentsTheme`）还在 `index.html` 末尾，接上 client.js 后会自动生效 —— 但它引用的 8 个 css 是原作者的，需要你自己做或直接用 `preferred_color_scheme`。

## 部署到 GitHub Pages

仓库必须**叫 `WeCream.github.io`**，这是根站（`wecream.github.io`）的硬性要求。

**第一步：在 GitHub 上建仓库**（网页操作，30 秒）

打开 <https://github.com/new>，Repository name 填 `WeCream.github.io`，选 **Public**，**不要**勾 Add a README / .gitignore / license（本地已经有了），点 Create repository。

**第二步：把本地这份推上去**（PowerShell）

```powershell
cd "D:\personal web"
git remote add origin https://github.com/WeCream/WeCream.github.io.git
git push -u origin main
```

第一次推会弹 GitHub 登录窗口（浏览器授权），登录 `WeCream` 账号即可。

**第三步：开 Pages**

仓库 **Settings → Pages** → Source 选 **Deploy from a branch** → Branch 选 `main`、目录 `/ (root)` → Save。等 1–2 分钟，访问 <https://wecream.github.io/>。

> 以后要改内容：改完 `git add . ; git commit -m "update" ; git push`，Pages 会自动重新发布。

## 许可与致谢

- 版式与交互来自 **Alakazam** 的 [stardew 主页](https://alakazamc.github.io/)，页脚保留了一条署名链接。
- 三维场景的卡通材质、墨线、天空与树，改编自 **Sakura Crossing**（MIT），见 `assets/toon/main.js` 头部注释。
- 像素字体为 **Fusion Pixel Font**（缝合像素字体），随 `font.css` 一起分发；商用前请自行确认其授权条款。

`_source/` 是扒站时的原件（原站 HTML、字体、构建脚本 `build.py`、校验脚本 `check.py`），已在 `.gitignore` 里排除，不会上传。想从原件重新生成主页：`python _source\build.py`（会覆盖 `index.html`）。
