# Corbel Songbook

Cécile Corbel 各专辑歌词原文与私藏中文翻译对照网站。单页静态站，纯个人欣赏与学习用途。

## 结构

```
corbel-songbook/
├── public/
│   ├── index.html      # 页面结构 + 样式 + 渲染逻辑（不含歌词数据）
│   ├── data/
│   │   └── albums.js   # 全部专辑与歌词数据（window.ALBUMS 数组）
│   ├── robots.txt      # 禁止搜索引擎收录
│   └── .nojekyll       # 关闭 GitHub Pages 的 Jekyll 处理
├── .github/workflows/
│   └── deploy.yml      # GitHub Actions 自动部署到 Pages
├── README.md
└── .gitignore
```

**数据与代码分离，但仍是零依赖静态站**（双击 `index.html` 即可打开，无需服务器或构建）：
- `public/data/albums.js` —— 全部歌词数据，一个 `window.ALBUMS` 数组，每首歌一个对象。**改歌词只动这个文件。**
- `public/index.html` —— 页面骨架、样式（`<style>`）和渲染逻辑（`buildNav`/`selectSong` 等），用 `<script src="data/albums.js">` 引入数据。改样式或交互只动这里。

## 部署（GitHub Pages via Actions）

本仓库通过 `.github/workflows/deploy.yml` 自动部署：`main` 分支一 push，GitHub Actions 就把 `public/` 目录发布为站点。

**首次启用**（在 GitHub 仓库网页上）：
1. 仓库 **Settings → Pages → Build and deployment → Source** 选 **GitHub Actions**。
2. push 后在 **Actions** 标签看部署进度，成功后 Pages 里显示网址（形如 `https://<用户名>.github.io/corbel-songbook/`）。

> 仓库虽公开，但 `robots.txt` + `index.html` 里的 `noindex` meta 会阻止搜索引擎收录。

本地预览：任意静态服务器指向 `public/`，例如 `npx serve public`。

## 添加/修改一首歌的歌词

1. 在浏览器打开该歌在 lyricstranslate.com 的歌词页，`Ctrl+S` 存成 HTML（Cloudflare 挡直接抓取，必须真实浏览器保存）。
2. 歌词原文在保存文件的 `<div class="ltf">` 容器里，`<div class="ll-x-y">` 每个是一行。
3. 按下面的数据格式，把对象填进 `public/data/albums.js` 的 `ALBUMS` 数组对应专辑的 `songs` 里。

### 数据格式

```js
{
  t: "歌名",
  lang: ["FR"],      // 语言标签数组，一码一语言；多语言并列如 ["FR","EN"]
                     // 可用码：EN/FR/BR/GA/GD/SCO/IT/LA/ES/LAD/JP/HE/TR/Instr.
  note: "一句背景介绍（可选）",
  lyrics: [
    { role: "Verse 1",          // 段落标记（可选）
      chorus: false,            // true = 副歌样式（金色竖线高亮）
      orig:  ["原文第一行", "原文第二行"],
      trans: ["译文第一行", "译文第二行"] },  // 留空 [] 显示"待译"
  ]
}
```

- `lyrics: null`（或不写 lyrics 字段）= 整首待录入，页面显示占位提示，导航标"待录"。
- 反复的副歌只列一次，在 `role` 或 `note` 里注明"全曲反复"。
- 拟声/衬词段落合并示意即可，不逐次重复。

## 版权

歌词版权归原作者所有。本站仅供个人欣赏与学习。仓库公开托管于 GitHub Pages，但已用 robots/noindex 阻止搜索引擎收录，不主动对外分发。
