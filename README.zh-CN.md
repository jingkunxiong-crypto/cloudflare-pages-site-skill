# Cloudflare Pages Site

**从本地静态官网制作，到 Cloudflare Pages 部署与线上验收的 Agent Skill。**

[English](README.md) · [Skill 正文](SKILL.md) · [部署命令参考](references/direct-upload.md)

这个 Skill 来源于 [Pianyu Studio 官网](https://pianyu.jacksakuya.com/)的实际制作与发布流程。它保留了可复用的判断和操作步骤，不会把片屿的配色、域名或项目结构强加给其他项目。

## 能做什么

- 根据项目目标、受众和主要按钮规划网站内容与页面顺序。
- 找到真正要上传的静态输出目录，检查手机和桌面布局。
- 处理图片加载、可直接下载的安装包等静态资源。
- 使用 Wrangler 或当前可用的 Cloudflare 连接部署到 Pages Direct Upload。
- 绑定自定义域名，并检查正式网址、关键资源和下载链接。

适合产品介绍页、作品集、应用官网等公开静态站。需要服务端渲染、登录或动态 API 的项目，应先选择合适的 Pages Functions / Workers 架构。安装 Skill 本身不会执行部署，也不包含任何账号密钥。

## 安装

Codex 用户可以用 [skills CLI](https://github.com/vercel-labs/skills) 全局安装：

```bash
npx skills add jingkunxiong-crypto/cloudflare-pages-site-skill --skill cloudflare-pages-site -a codex -g
```

也可以手动放进 Codex 的 Skills 目录：

```powershell
git clone https://github.com/jingkunxiong-crypto/cloudflare-pages-site-skill.git "$env:USERPROFILE\.codex\skills\cloudflare-pages-site"
```

仓库根目录的 `SKILL.md` 与 `references/` 文件夹需要一起保留。其他支持 `SKILL.md` 的 Agent 客户端也可以使用同一份 Skill。

## 怎么用

例如对 Codex 说：

> 使用 `$cloudflare-pages-site` 给这个 Android 项目做官网，部署到 Cloudflare Pages，绑定 `app.example.com`，并让安装包可以从网站直接下载。

已有网站也可以说：

> 使用 `$cloudflare-pages-site` 更新当前静态网站，并检查手机版和正式域名上的新版本。

Skill 会先检查项目。纯 HTML 网站可能上传 `site/`；使用构建工具的项目应先构建，再上传 `dist/` 等真正的输出目录。遇到部署选项或限制时，会核对 Cloudflare 的最新文档。

## 文件结构

```text
SKILL.md                      Agent 的工作指引与决策边界
references/direct-upload.md   PowerShell 命令、域名、下载、验收参考
```

参考文件包含版本化 APK 下载的 `_headers` 示例；这些是按需读取的说明，不会自动执行。

## 发布时的原则

- Direct Upload 与 Git 自动部署是不同的项目创建方式；已有项目保持原有方式。
- 绑定子域名时，要先在 Pages 项目中关联该域名，单独添加 DNS 记录不够。
- 上传目录中的内容会公开提供访问；不要放入密钥或私有素材。
- 上传成功后，仍要打开正式 HTTPS 域名检查页面与主要操作。

具体规则以 Cloudflare 的 [Direct Upload](https://developers.cloudflare.com/pages/get-started/direct-upload/)、[自定义域名](https://developers.cloudflare.com/pages/configuration/custom-domains/)和[响应头](https://developers.cloudflare.com/pages/configuration/headers/)文档为准。

## 许可

[MIT](LICENSE)。作者 Sakuya_Jack。本项目与 Cloudflare、GitHub 无官方关联。
