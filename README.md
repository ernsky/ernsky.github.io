# ernsky.github.io

Ernsky 的个人博客，基于 **Hugo** + **[hugo-theme-diary](https://github.com/AmazingRise/hugo-theme-diary)** + **GitHub Actions** 自动部署。

在线访问：<https://ernsky.github.io>

## 目录结构

```
ernsky.github.io/
├── .github/workflows/hugo.yml     # GitHub Actions：自动构建 + 部署
├── archetypes/default.md          # `hugo new` 的默认 front matter
├── content/                       # 你的 Markdown 文章
│   ├── _index.md                  # 首页
│   └── posts/                     # 博客文章
├── layouts/                       # 可选：覆盖主题的局部模板
├── static/                        # 直接拷贝到站点的静态资源（favicon、头像…）
├── themes/diary/                  # 主题（git submodule，clone 后才有）
├── .gitignore
├── hugo.toml                      # Hugo 主配置
└── README.md
```

## 一次性配置（你只需要做这一回）

### 1. 安装主题为 submodule

主题的名字是 `diary`（不是 `hugo-theme-diary`），目录是 `themes/diary`：

```bash
git submodule add https://github.com/AmazingRise/hugo-theme-diary.git themes/diary
git submodule update --init --recursive
```

### 2. 推到 GitHub

```bash
git init
git add .
git commit -m "feat: 初始化 Hugo + Diary 博客"
git branch -M main
git remote add origin https://github.com/ernsky/ernsky.github.io.git
git push -u origin main
```

### 3. 在 GitHub 上启用 Pages

打开 <https://github.com/ernsky/ernsky.github.io/settings/pages>

- **Source** 选 **GitHub Actions**（不是 "Deploy from a branch"）
- 等几十秒，第一次 Action 跑完后会显示你的站点 URL

到此为止就上线了。以后 `git push`，约 30-60 秒后自动上线。

## 本地写文章（推荐，可选）

### 安装 Hugo（Windows）

> 该主题用 SCSS，**必须用 extended 版本**。

用 [winget](https://learn.microsoft.com/windows/package-manager/winget/)（Win 11 自带）：

```bash
winget install Hugo.Hugo.Extended
```

或用 [Chocolatey](https://chocolatey.org/)：

```bash
choco install hugo-extended
```

或直接从 [Hugo Releases](https://github.com/gohugoio/hugo/releases) 下载 `hugo_extended_*_windows-amd64.zip`，解压后把 `hugo.exe` 加入 PATH。

### 拉代码到本地

```bash
git clone --recursive https://github.com/ernsky/ernsky.github.io.git
cd ernsky.github.io
```

> `--recursive` 会自动把 diary 主题 submodule 拉下来。

### 启动本地预览

```bash
hugo server -D
```

打开 <http://localhost:1313>，修改任何文件会实时热更新。

### 写新文章

```bash
hugo new posts/my-new-post.md
```

然后编辑 `content/posts/my-new-post.md`，把 `draft: true` 改成 `draft: false` 后 `git push`。

## 主题专属配置（hugo.toml 中的常用 [params]）

| 参数 | 作用 | 默认 |
| --- | --- | --- |
| `subtitle` | 顶部副标题 | 空 |
| `keywords` | 侧栏标签 | `[]` |
| `description` | meta 描述 | 空 |
| `enableOpenGraph` | 开启 OG meta | `true` |
| `enableTwitterCards` | 开启 Twitter Card | `true` |
| `enableGitalk` | Gitalk 评论 | `false` |
| `enableGiscus` | Giscus 评论 | `false` |
| `enableLiveRe` | 来必力评论 | `false` |
| `enableWaline` | Waline 评论 | `false` |
| `enableTwikoo` | Twikoo 评论 | `false` |

需要哪个评论系统，把对应 `[params.xxx]` 段填上即可（详见 [主题 Wiki](https://github.com/AmazingRise/hugo-theme-diary/wiki)）。

## 切换主题

不喜欢 diary 的话，去 [Hugo Themes](https://themes.go Hugo.io/) 选一个，然后：

```bash
# 删除 diary
rm -rf themes/diary
git rm themes/diary
# 添加新主题
git submodule add https://github.com/<作者>/<主题>.git themes/<主题名>
```

再把 `hugo.toml` 里的 `theme = "diary"` 改成新主题名。

## 常用命令速查

| 命令 | 作用 |
| --- | --- |
| `hugo server -D` | 本地预览（含草稿） |
| `hugo server` | 本地预览（不含草稿） |
| `hugo` | 构建静态文件到 `public/` |
| `hugo new posts/xxx.md` | 新建文章 |

## 排错

- **部署后页面 404**：去 GitHub Actions 看构建日志；多半是 `baseURL` 写错了
- **样式没出来**：确认 diary submodule 是不是拉到了；本地 `hugo server` 看控制台
- **"extended" 报错**：你本地用了非 extended 版本；重新装 `hugo-extended`
- **菜单不显示**：检查 `[menu.main]` 里 `url` 是否对应真实存在的 section（如 `/posts/` 对应 `content/posts/_index.md`）

## 参考

- Hugo 官方文档：<https://gohugo.io/documentation/>
- Diary 主题 Wiki：<https://github.com/AmazingRise/hugo-theme-diary/wiki>
- peaceiris/actions-hugo：<https://github.com/peaceiris/actions-hugo>
- GitHub Pages：<https://docs.github.com/en/pages>
