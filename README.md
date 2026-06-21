# 我的博客

基于 **Hugo** + **Theme Stack** 搭建的个人博客，部署于 **Cloudflare Pages**。

## 技术栈

| 层 | 选型 | 说明 |
|----|------|------|
| 静态站点生成器 | Hugo 0.163.3 (extended) | Go 编写，构建速度毫秒级 |
| 主题 | [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack) v3 | 卡片式布局，支持深色模式、多语言 |
| 托管 | Cloudflare Pages | 免费，全球 CDN，不限流量 |
| 版本控制 | Git + GitHub | 源码托管，推送即部署 |
| 多语言 | 6 种语言 | 简体中文、English、正體中文、日本語、한국어、Français |

## 项目结构

```
blog/
├── archetypes/default.md          # 新文章模板
├── config/_default/
│   ├── hugo.toml                  # 站点配置（URL、语言、分页、永久链接）
│   ├── languages.toml             # 多语言定义
│   ├── menu.toml                  # 社交链接
│   └── params.toml                # 主题参数（侧边栏、小组件、头像）
├── content/
│   ├── _index.md                  # 首页（.en.md = English，.ja.md = 日本語...）
│   ├── page/
│   │   ├── about/index.md         # 关于页面
│   │   ├── archives/index.md      # 归档页面
│   │   └── search/index.md        # 搜索页面
│   └── post/                      # 博客文章，每篇一个子目录
├── static/img/
│   ├── avatar.jpg                 # 侧边栏头像
│   └── icon.jpg                   # 浏览器 favicon
└── themes/stack/                  # 主题（git submodule）
```

## 本地开发

### 环境要求

- Hugo extended 0.163.3+
- Git

### 克隆项目

```bash
git clone --recurse-submodules https://github.com/nixiaohui1983/blog.git
cd blog
```

### 启动开发服务器

```bash
hugo server -D
# 访问 http://localhost:1313
```

`-D` 会包含草稿文章。

### 写新文章

```bash
# 创建文章
hugo new content post/<slug>/index.md

# 编辑生成的文件，把 draft: true 改为 draft: false
# 本地预览确认无误后提交
```

文章图片放在文章目录内，用相对路径引用：

```markdown
![截图](screenshot.png)
```

## 多语言

### 已支持的语言

| 语言 | 文件名后缀 | URL 路径 |
|------|-----------|----------|
| 简体中文 | `.md`（默认） | `/` |
| English | `.en.md` | `/en/` |
| 正體中文 | `.zh-hant-tw.md` | `/zh-hant-tw/` |
| 日本語 | `.ja.md` | `/ja/` |
| 한국어 | `.ko.md` | `/ko/` |
| Français | `.fr.md` | `/fr/` |

### 给文章添加翻译

在文章目录里创建带语言后缀的对应文件即可：

```
content/post/my-article/
├── index.md        # 中文版（默认语言）
├── index.en.md     # English 版
└── index.ja.md     # 日文版
```

如果某种语言没有翻译，该语言版本会自动跳过这篇文章，不会显示空白页。

### 添加新语言

1. 在 `config/_default/languages.toml` 中追加语言配置
2. 为所需页面/文章创建 `.<语言代码>.md` 文件
3. 界面文字由主题内置的 `themes/stack/i18n/` 自动翻译，无需手动处理

## 配置说明

### 站点配置 (`config/_default/hugo.toml`)

| 字段 | 说明 |
|------|------|
| `baseURL` | 部署后改为你的域名 |
| `hasCJKLanguage` | `true` → 中文摘要/字数统计准确 |
| `pagerSize` | 首页每页文章数 |
| `permalinks.post` | 文章永久链接格式 |

### 主题参数 (`config/_default/params.toml`)

| 字段 | 说明 |
|------|------|
| `sidebar.avatar` | 头像路径，放在 `static/img/` |
| `sidebar.emoji` | 头像下方的小图标 |
| `sidebar.subtitle` | 博客副标题 |
| `widgets.homepage` | 首页侧边栏小组件 |
| `footer.since` | 页脚版权起始年份 |

### 社交链接 (`config/_default/menu.toml`)

支持任何 Tabler Icon，改 `icon` 字段即可。图标名参考 [tabler.io/icons](https://tabler.io/icons)。

## 部署

### 方式 1：Cloudflare Pages（推荐）

1. 把代码推到 GitHub
2. Cloudflare Dashboard → Workers & Pages → Create → Pages → Connect to Git
3. 构建配置：

| 字段 | 值 |
|------|-----|
| Build command | `hugo --gc --minify` |
| Build output directory | `public` |

4. 添加环境变量：`HUGO_VERSION` = `0.163.3`
5. 部署完成后可绑定自定义域名

每次 `git push` 到 `main` 分支会自动触发部署。

### 方式 2：GitHub Pages

```bash
# 在 .github/workflows/ 下添加 CI 文件，或手动构建推送 gh-pages 分支
```

## 更新主题

```bash
cd themes/stack
git pull origin main
cd ../..
git add themes/stack
git commit -m "Update theme"
git push
```

## 常见问题

**Q: 构建失败？**
检查 Cloudflare Pages 环境变量是否设置了 `HUGO_VERSION=0.163.3`，否则会使用旧版 Hugo。

**Q: 文章不显示？**
检查 frontmatter 中 `draft: true` 是否改为了 `false`。

**Q: 如何关闭评论？**
`config/_default/params.toml` 中 `[comments] enabled = false`。
