# Yunliang Zang Lab Website

这是 Yunliang Zang Lab 官网 `https://yunliangzanglab.github.io/` 的仓库。

网站使用 [Hexo](https://hexo.io/) 生成静态页面。仓库根目录保存当前 GitHub Pages 直接托管的静态网站文件，`lab_page/` 保存真正需要编辑的 Hexo 源码。合并到 `main` 后，GitHub Actions 会自动从 `lab_page/` 构建，并把生成结果同步回仓库根目录。

## 仓库结构

```text
.
├── index.html                   # GitHub Pages 当前发布的首页，自动生成
├── People/                      # GitHub Pages 当前发布的页面，自动生成
├── Publications/                # GitHub Pages 当前发布的页面，自动生成
├── Research/                    # GitHub Pages 当前发布的页面，自动生成
├── css/ js/ img/ attaches/      # GitHub Pages 当前发布的静态资源，自动生成
├── lab_page/                    # Hexo 源码，日常只改这里
│   ├── _config.yml              # Hexo 站点配置
│   ├── package.json             # 本地开发和构建命令
│   ├── source/                  # 网站正文内容
│   ├── source/images/           # 正文中引用的图片
│   └── themes/Academia/         # 当前使用的 Hexo 主题
├── .github/workflows/deploy.yml # 自动构建并更新根目录静态文件
└── README.md
```

除非你在处理部署脚本，否则不要手动编辑根目录的 `index.html`、`People/`、`css/`、`js/` 等自动生成文件。日常修改应在 `lab_page/` 下完成。

## 本地预览

第一次使用时，先安装 Node.js 20 或更新版本，然后运行：

```bash
git clone https://github.com/yunliangzanglab/yunliangzanglab.github.io.git
cd yunliangzanglab.github.io/lab_page
npm ci
npm run server
```

打开 `http://localhost:4000/` 即可预览网站。

如果安装依赖时遇到 npm 镜像问题，可以临时使用官方 registry：

```bash
npm ci --registry=https://registry.npmjs.org
```

## 修改内容

常见修改位置：

- 首页：编辑 `lab_page/source/_posts/Home.md`
- 成员介绍：编辑 `lab_page/source/People/index.md`
- 论文列表：编辑 `lab_page/source/Publications/index.md`
- 研究方向：编辑 `lab_page/source/Research/index.md`
- 招聘信息：编辑 `lab_page/source/Job_Openings/index.md`
- 资助信息：编辑 `lab_page/source/Funding/index.md`
- 正文图片：放到 `lab_page/source/images/`，然后在 Markdown 中引用
- 导航菜单、头像、社交链接：编辑 `lab_page/themes/Academia/_config.yml`
- 主题样式：编辑 `lab_page/themes/Academia/source/css/`

修改后建议先本地验证：

```bash
cd lab_page
npm run clean
npm run build
npm run server
```

`npm run build` 成功后会生成 `lab_page/public/`，但 `lab_page/public/` 不需要提交。

## 提交和部署流程

推荐所有成员使用 Pull Request：

```bash
git checkout -b update-people-page
# 修改 lab_page/ 下的内容
cd lab_page
npm run clean
npm run build
cd ..
git add .
git commit -m "Update people page"
git push origin update-people-page
```

然后在 GitHub 上创建 Pull Request。PR 合并到 `main` 后，GitHub Actions 会自动执行：

1. 进入 `lab_page/`
2. 安装依赖
3. 构建 Hexo 网站到 `lab_page/public/`
4. 把 `lab_page/public/` 同步到仓库根目录
5. 自动提交生成后的静态文件

部署状态可以在 GitHub 仓库的 `Actions` 页面查看。生成文件提交信息一般是 `Site updated from Hexo source [skip deploy]`。

## 组织成员权限

组织管理员可以在 GitHub organization 中创建一个网站维护团队，例如 `website-maintainers`，并给该团队授予本仓库权限：

- `Read`：只能查看和 fork
- `Write`：可以创建分支、提交代码、发 Pull Request
- `Maintain`：可以管理 issue、PR 和部分仓库设置
- `Admin`：可以管理 Pages、Actions、权限等所有设置

建议普通维护成员使用 `Write` 权限，通过 Pull Request 修改网站；少数负责人使用 `Maintain` 或 `Admin` 权限负责审核和仓库设置。

## 注意事项

- 不要提交 `node_modules/`、`lab_page/public/`、`lab_page/db.json` 或 `.deploy_git/`。
- 不再使用 `hexo deploy` 手动推送构建产物。
- 日常只编辑 `lab_page/`，根目录静态文件由 GitHub Actions 自动更新。
- 如果自动部署失败，先查看 GitHub 的 `Actions` 日志，再在本地进入 `lab_page/` 运行 `npm ci && npm run build` 复现问题。
- 如果 Actions 提示没有写入权限，需要组织管理员在仓库 `Settings` → `Actions` → `General` 中允许 workflow 使用 `Read and write permissions`。
