# 花神语文大对战 — Cloudflare 部署版

一个面向 3～6 年级的语文古诗词答题对战小游戏，纯静态单页，无需后端、无需数据库。
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
2. 左侧进入 **Workers & Pages** → **Create** → **Pages** → **Upload assets**（上传文件）
3. 给项目起个名字，例如 `huashen-yuwen`
4. 把本文件夹 `cloudflare-deploy` 里的文件（`index.html`、`_headers`）**直接拖进上传框**
   - 注意：拖文件夹里的“文件”，不要把外层文件夹一起拖
5. 点 **Deploy**，几秒后会得到一个网址，形如
   `https://huashen-yuwen.pages.dev`
6. 打开网址即可游玩并分享。

### 方法 B：命令行部署（用 Wrangler）

需要先装好 Node.js，然后：

```bash
# 安装 Cloudflare 官方 CLI
npm install -g wrangler

# 登录（会打开浏览器授权）
wrangler login

# 在 cloudflare-deploy 目录下执行部署
cd cloudflare-deploy
wrangler pages deploy . --project-name=huashen-yuwen
```

完成后终端会输出访问网址。

### 方法 C：连接 Git 仓库自动部署

1. 把 `cloudflare-deploy` 目录下的文件推到一个 GitHub/GitLab 仓库
2. Cloudflare Pages → **Create** → **Connect to Git** → 选中该仓库
3. 构建设置：
   - **Build command（构建命令）**：留空
   - **Build output directory（输出目录）**：`/`（根目录）
4. 点 Save and Deploy。之后每次推送代码都会自动更新网站。

## 常见问题

- **战绩会丢吗？** 战绩存在每个用户自己的浏览器里（localStorage）。同一台设备同一浏览器会保留；换设备、换浏览器或清除浏览器数据会重置。这是纯前端游戏的正常表现。
- **要不要数据库？** 不需要。如果将来想做“全班排行榜”那种跨设备共享的战绩，可以再升级成 Cloudflare Workers + KV/D1，届时可另行扩展。
- **能绑自己的域名吗？** 可以。Pages 项目里 **Custom domains** → 添加你的域名即可。
- **手机能玩吗？** 可以，界面是自适应的，手机/平板/电脑都支持。

## 自定义

游戏的题库、花神、段位规则全部在 `index.html` 内：

- 题库：搜索 `const BANK`，按现有格式增删题目即可
- 段位：搜索 `const RANKS`，可调整段位名称和所需胜场
- 花神难度：搜索 `function deityTurn`，可调整对手强度
- 每局题数 / 计时：搜索 `const ROUND` 和 `const TIME_LIMIT`
