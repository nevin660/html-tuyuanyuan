# 花神语文大对战 — Cloudflare 部署版

面向 3～6 年级的语文古诗词答题对战小游戏，纯静态单页，无需后端、无需数据库。
玩家战绩保存在浏览器本地（localStorage），可直接部署到 **Cloudflare Pages**。

## 目录结构

```
cloudflare-deploy/
├─ index.html      # 游戏本体（单文件，含全部 HTML/CSS/JS）
├─ _headers        # Cloudflare Pages 的 HTTP 头/缓存配置
├─ wrangler.toml   # 命令行部署配置（用 wrangler 时需要）
└─ README.md       # 本说明
```

## 部署方法（任选其一）

### 方法 A：网页拖拽上传（最简单，零命令）

1. 登录 https://dash.cloudflare.com
2. 左侧 **Workers & Pages** → **Create** → **Pages** → **Upload assets**
3. 起个项目名，例如 `huashen-yuwen`
4. 把本文件夹里的文件（`index.html`、`_headers`）拖进上传框
5. 点 **Deploy**，几秒后得到网址，形如 `https://huashen-yuwen.pages.dev`

### 方法 B：命令行（Wrangler）

```bash
npm install -g wrangler
wrangler login
cd cloudflare-deploy
wrangler pages deploy . --project-name=huashen-yuwen
```

### 方法 C：连接 Git 仓库自动部署

推到 GitHub/GitLab 仓库后，在 Cloudflare Pages 选 **Connect to Git**，
构建命令留空、输出目录填 `/`，保存即可。之后每次推送自动更新。

## 说明

- **战绩存哪里？** 存在每个用户自己的浏览器（localStorage），换设备/换浏览器/清缓存会重置。这是纯前端游戏的正常表现。
- **花的图片？** 用 Wikimedia Commons 真实照片（在线加载），断链时自动回退到内置 SVG 插画，不会空白。
- **能绑域名吗？** 可以，Pages 项目里 Custom domains 添加即可。
- **手机能玩吗？** 可以，界面自适应。

## 自定义（都在 index.html 内）

- 题库：搜索 `const BANK`
- 段位：搜索 `const RANKS`
- 花神难度：搜索 `function deityTurn`
- 桂花特困难模式：搜索 `HARD_KEY` / `HARD_TIME`
- 每局题数 / 计时：搜索 `const ROUND` / `timeLimit`
