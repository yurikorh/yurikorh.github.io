# yuriko.github.io

个人博客网站，基于 [Hugo](https://gohugo.io/) 静态网站生成器构建。

## 项目简介

这是一个使用 Hugo 构建的个人博客网站，采用 Tailwind 主题。网站支持中文内容，包含文章发布、分类、标签等功能。

## 技术栈

- **静态网站生成器**: [Hugo](https://gohugo.io/)
- **主题**: Tailwind
- **语言**: 中文 (zh-cn)
- **部署平台**: Cloudflare Pages

## 项目结构

```
.
├── archetypes/          # 内容模板
├── content/             # 网站内容
│   └── posts/          # 博客文章
├── static/             # 静态资源文件
│   └── images/         # 图片资源
├── themes/             # 主题目录（如使用 submodule）
├── hugo.toml           # Hugo 配置文件
├── .gitignore          # Git 忽略文件配置
└── README.md           # 项目说明文档
```

---

## 📚 完整部署教程：从零开始使用 Hugo + GitHub + Cloudflare Pages

这是一份逐步详尽的中文教程，教你如何使用 Hugo 本地创建网站源码、推到 GitHub、然后用 Cloudflare Pages 的内置构建自动部署。每一步都包含命令、说明和常见注意事项，适合从零开始操作。

### 准备工作（先做这些）

#### 1. 账户与工具

- **GitHub 账户**（若没有，先注册 https://github.com）
- **Cloudflare 账户**（若没有，先注册 https://dash.cloudflare.com）
- **在本地安装 Git**
  - macOS（Homebrew）：`brew install git`
  - Windows（Git for Windows）：https://gitforwindows.org/
  - Linux（Ubuntu）：`sudo apt install git`
- **在本地安装 Hugo**（推荐 Extended 版本，用于 SCSS 支持）
  - macOS（Homebrew）：`brew install hugo`
  - Windows（Chocolatey）：`choco install hugo -confirm`
  - 或前往 https://gohugo.io/getting-started/installing/ 下载并安装
- **可选**：代码编辑器（VS Code、Vim 等）

---

### 步骤 1 — 在本地创建 Hugo 站点

1. **打开终端**（或 PowerShell / CMD）

2. **创建站点目录并初始化 Hugo**：
   ```bash
   hugo new site myblog
   cd myblog
   ```
   - 说明：`myblog` 会被创建为 Hugo 项目根目录，包含 `config`、`content`、`layouts`、`themes` 等默认目录。

3. **初始化 Git 仓库**：
   ```bash
   git init
   ```
   
   创建 `.gitignore` 文件，推荐内容：
   ```
   /public
   /resources
   .DS_Store
   node_modules
   ```
   
   然后提交初始文件：
   ```bash
   git add .
   git commit -m "Initial Hugo site"
   ```

---

### 步骤 2 — 选择并安装主题

你可以用官方/第三方主题，常见中文友好主题有 PaperMod、Ananke（简单）、wowchemy（功能强大但复杂）等。以 PaperMod 为例（推荐用于博客）：

1. **使用 Git submodule**（推荐，便于更新主题）：
   ```bash
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   ```
   - 如果不想 submodule，可以直接 `git clone` 到 `themes/` 下。

2. **设置主题**：编辑 `config.toml`（如果是 YAML 就改相应文件）
   
   例如创建 `config.toml`（最简）：
   ```toml
   baseURL = "https://example.com/"
   languageCode = "zh-cn"
   title = "我的 Hugo 博客"
   theme = "PaperMod"
   paginate = 10
   ```
   - 保存。

---

### 步骤 3 — 创建你的第一篇文章并本地预览

1. **新建文章**（示例）：
   ```bash
   hugo new posts/my-first-post.md
   ```
   - 编辑 `content/posts/my-first-post.md`，把 front matter 的 `draft: true` 改为 `draft: false`，填入标题和内容。

2. **本地运行预览**：
   ```bash
   hugo server -D
   ```
   - 打开浏览器访问 `http://localhost:1313` 查看效果
   - 修改文件后会自动热重载查看效果

#### ⚠️ 注意点

- 如果主题需要 Node / npm（例如一些主题有前端构建），按主题 README 安装依赖并构建（通常可在本地预览时按照主题文档操作）。
- 如果站点使用 SCSS/资源管线，需要确保使用 Hugo Extended 版本（`hugo version` 输出中会包含 `extended`）。

---

### 步骤 4 — 推送源码到 GitHub（仓库为主分支 main）

1. **在 GitHub 新建仓库**
   - 打开 https://github.com/new
   - 仓库名例如 `username.github.io` 或 `hugo-blog`（如果你想用 Cloudflare Pages 自定义域，仓库名不限）
   - 不勾选初始化 README（因为本地已有内容更方便）

2. **将本地仓库与远程关联并推送**：
   ```bash
   git remote add origin git@github.com:your-username/your-repo.git
   # 或使用 HTTPS：https://github.com/your-username/your-repo.git
   git branch -M main
   git add .
   git commit -m "Initial commit"
   git push -u origin main
   ```

   **确认**：在 GitHub 上能看到项目文件（`content`、`config.toml`、`themes` 等）。

---

### 步骤 5 — 在 Cloudflare Pages 创建项目并配置内置构建

现在把 GitHub 仓库连接到 Cloudflare Pages，让 Cloudflare 负责构建并托管静态站点。

1. **登录 Cloudflare 仪表盘，进入 Pages**：
   - 打开 https://dash.cloudflare.com/
   - 在左侧选择 **"Pages"** -> **"Create a project"**

2. **连接 Git 提供者**
   - 点击 **"Connect to Git provider"** -> 选择 **GitHub**
   - 授权 Cloudflare 访问你的 GitHub 账号（如果是首次连接，会要求授权，选择要授权的仓库：建议选择你的站点仓库或全部仓库）
   - 完成授权后，Cloudflare 会列出你的仓库列表

3. **选择仓库并设置项目**
   - 在仓库列表中选择你刚刚推送的仓库（例如 `your-username/your-repo`）
   - Cloudflare Pages 会尝试检测框架，若检测到 Hugo 会自动填入默认值。如果没有检测到，手动填写：
     - **Framework preset**：`Hugo`
     - **Production branch**：`main`
     - **Build command**：`hugo --minify`
       - 说明：`hugo` 或 `hugo --minify`（去除空白、压缩资源）
     - **Build output directory**：`public`

4. **设置环境变量**（可选，但推荐）
   - 如果你需要指定 Hugo 版本或启用 Extended 版本，可添加环境变量：
     - `HUGO_VERSION` = `0.XX.X` （例如 `0.114.2`，或填写 `latest`）
     - `HUGO_ENV` = `production`
     - 若需确保 Extended：`HUGO_EXTENDED` = `true`（Cloudflare 会根据此尝试安装 Extended 版本）
   - 如果主题需要 Node 构建步骤（例如 `npm run build`），需要在 Cloudflare 的 Build command 中新增相应命令，或在仓库根目录添加 `package.json` 并让 Cloudflare 安装依赖（Pages 会自动安装 Node 依赖，前提是 `package.json` 存在）。

5. **创建项目并触发首次部署**
   - 点击 **"Save and deploy"** 或 **"Start build"**
   - Cloudflare Pages 会拉取仓库、执行 build command 并把生成的 `public` 文件部署
   - 部署日志会显示在界面上（Build & Deploy logs），若构建失败可在此查看错误信息

6. **部署成功后**
   - Cloudflare 会给出一个 `pages.dev` 子域名（例如 `your-project.pages.dev`），可以点击查看已部署站点
   - 这是你的站点线上地址，Cloudflare 会自动启用 HTTPS

---

### 步骤 6 — 配置自定义域（可选）

若有自有域名并想绑定：

1. 在 Cloudflare Pages 项目页面，选择 **Custom domains** -> **Add custom domain**，输入你的域名（例如 `blog.example.com` 或 `example.com`）

2. Cloudflare 会提示 DNS 记录设置：
   - 若你的域名已经由 Cloudflare 管理：系统会自动添加必要记录并为你验证
   - 若你的域名不在 Cloudflare 管理：你需在你当前域名的 DNS 提供商处添加指定的 CNAME 或 A 记录，Cloudflare 页面会给出具体记录值

3. 验证完成后，Cloudflare 会为自定义域配置 TLS/HTTPS（通常自动启用）

4. 如果你添加的是根域（apex domain）可能需要 A 记录或 ALIAS/ANAME 记录，注意 Pages 的提示

---

### 步骤 7 — 日常写作与自动部署过程（工作流说明）

- 当你在本地创建新文章并 push 到 GitHub（main 分支）时，Cloudflare Pages 会自动触发构建并部署最新网站（内置构建触发器）

- **推荐写作流程**：
  ```bash
  hugo new posts/new-post.md
  # 编辑文件，保存并 commit：
  git add content/posts/new-post.md
  git commit -m "Add new post"
  git push origin main
  ```
  - 等待 Cloudflare Pages 自动部署（几分钟内完成），访问 `pages.dev` 子域名查看

---

### 步骤 8 — 常见构建问题与排查

1. **构建失败**：查看 Cloudflare Pages 的 Build logs（Dashboard -> Pages -> 项目 -> Deployments -> 点击失败的部署）
   - **常见错误**：
     - **Hugo 版本不兼容**：在 Pages 的环境变量中设置 `HUGO_VERSION` 或在项目中指定一个兼容版本
     - **主题依赖未安装**：若主题需要 Node 依赖，确保 `package.json` 存在并在 Build command 中运行 `npm ci && npm run build`（或让 Pages 自动安装）
     - **Hugo Extended 相关问题**：若出现 SCSS/资源编译错误，确保 `HUGO_EXTENDED=true` 并指定 Extended 版本（或设置 `HUGO_VERSION` 对应 extended 版本）

2. **静态资源 404 或路径错误**：
   - 检查 `config.toml` 的 `baseURL` 是否正确（尤其当使用自定义域或子目录时）
   - 如果你在本地用相对路径而线上用绝对路径，适时调整 site config

3. **主题子模块未拉取**：
   - 如果你通过 Git submodule 添加主题，Cloudflare 在构建时可能不会自动初始化子模块。解决方法：
     - 不使用 submodule（直接 clone themes 到仓库）
     - 或在仓库中使用 GitHub Actions 预构建并推到 gh-pages（但这会跳出"Cloudflare 内置构建"的范围）
     - 或把主题作为普通目录提交到仓库

---

### 步骤 9 — 常用 Build command 示例（根据不同情况）

- **只用 Hugo**（最简单）：
  - Build command: `hugo`

- **启用 minify**：
  - Build command: `hugo --minify`

- **若使用 Node 工具链**（例如主题需要打包）：
  - Build command: `npm ci && npm run build && hugo --minify`
  - 在这种情况下，你需要在仓库根目录添加 `package.json`，Pages 会安装 Node（或在 Build settings 指定 Node 版本）

---

### 步骤 10 — 推荐的项目结构（示例）

```
myblog/
├── archetypes/
├── content/
│   └── posts/
├── data/
├── layouts/
├── static/
├── themes/
│   └── PaperMod/  (主题目录，若用 submodule 此目录由 submodule 管理)
├── config.toml
├── package.json (可选)
├── .gitignore
└── README.md
```

---

### 步骤 11 — 下一步优化建议

- 添加 CI 校验（可选）：语法/链接校验（GitHub Actions）
- 使用 SEO、社交预览（Open Graph）设置
- 启用分页、评论系统（如 Giscus）、站内搜索（Lunr.js 或 Algolia）
- 优化图片（Hugo Image Processing）与缓存策略
- 自动生成 RSS、sitemap（Hugo 默认支持）

---

## 快速开始（本地开发）

### 前置要求

- 安装 [Hugo](https://gohugo.io/installation/) (推荐使用扩展版本)

### 本地开发

1. **克隆仓库**
   ```bash
   git clone https://github.com/yurikorh/yurikorh.github.io.git
   cd yurikorh.github.io
   ```

2. **安装主题依赖**（如果使用 Git Submodule）
   ```bash
   git submodule update --init --recursive
   ```

3. **启动本地开发服务器**
   ```bash
   hugo server -D
   ```

4. 在浏览器中访问 `http://localhost:1313` 查看网站

### 构建静态网站

```bash
hugo
```

构建后的文件将输出到 `public/` 目录。

---

## 配置说明

主要配置文件为 `hugo.toml`，包含以下配置：

- **网站信息**: 标题、语言、基础 URL
- **主题设置**: Tailwind 主题配置
- **内容类型**: 文章内容类型设置为 `posts`
- **菜单配置**: 导航菜单项（Post、About）
- **分类系统**: 支持分类、标签、系列

---

## 内容管理

### 创建新文章

使用 Hugo 的 archetype 创建新文章：

```bash
hugo new posts/文章标题/index.md
```

文章模板位于 `archetypes/default.md`。

### 文章格式

文章使用 Markdown 格式，包含前置元数据（Front Matter）：

```markdown
+++
date = '2026-02-01T02:31:06+08:00'
draft = true
title = "文章标题"
+++

文章内容...
```

---

## 许可证

本项目采用 MIT 许可证。

## 联系方式

如有问题或建议，欢迎通过 GitHub Issues 联系。
