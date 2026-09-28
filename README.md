# Corbel Songbook

Cécile Corbel 各专辑歌词原文与私藏中文翻译对照网站。单页静态站，纯个人欣赏与学习用途。

## 结构

```
corbel-songbook/
├── public/
│   ├── index.html      # 整个网站（单文件，含全部数据与样式）
│   └── _headers        # Cloudflare Pages 响应头
├── README.md
└── .gitignore
```

网站是**单文件**的：所有歌词数据、翻译、样式、脚本都在 `public/index.html` 里。歌词数据在文件底部 `<script>` 里的 `ALBUMS` 数组中，每首歌一个对象。

## 部署（Cloudflare Pages）

本仓库连接到 Cloudflare Pages，`main` 分支一 push 即自动部署。

- **Build command**：留空（无需构建）
- **Build output directory**：`public`
- **Framework preset**：None

本地预览：任意静态服务器指向 `public/`，例如
```
npx serve public
```

## 添加/修改一首歌的歌词

1. 在浏览器打开该歌在 lyricstranslate.com 的歌词页，`Ctrl+S` 存成 HTML（Cloudflare 挡直接抓取，必须真实浏览器保存）。
2. 歌词原文在保存文件的 `<div class="ltf">` 容器里，`<div class="ll-x-y">` 每个是一行。
3. 按下面的数据格式，把对象填进 `public/index.html` 的 `ALBUMS` 数组对应专辑的 `songs` 里。

### 数据格式

```js
{
  t: "歌名",
  lang: "FR",        // 语言标签：EN/FR/BR/SCO/IT/ES/LA-IT 等
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

歌词版权归原作者所有。本站仅供个人欣赏与学习，不对外分发。仓库设为私有。
