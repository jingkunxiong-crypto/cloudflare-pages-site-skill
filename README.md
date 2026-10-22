# Cloudflare Pages Site

**An Agent Skill for taking a static product website from a local project to a verified Cloudflare Pages deployment.**

[简体中文](README.zh-CN.md) · [Read the skill](SKILL.md) · [Deployment reference](references/direct-upload.md)

This skill grew out of the workflow used to build and deploy the [Pianyu Studio website](https://pianyu.jacksakuya.com/). It is written for reuse: it does not impose Pianyu's design, domain, or project structure on another site.

## What it covers

- Shape a product site around its audience, content, and primary action.
- Find the actual static build output and review phone and desktop layouts.
- Keep images and optional direct-download files suitable for delivery on mobile.
- Publish with Cloudflare Pages **Direct Upload** using Wrangler or an available Cloudflare connection.
- Attach a custom domain and verify the live site, critical assets, and downloads.

It is intended for landing pages, portfolios, app showcases, and other public static sites. A site that needs server rendering, authentication, or dynamic APIs needs a suitable Pages Functions or Workers design first. The skill does not contain credentials or deploy anything merely by being installed.

## Install

For Codex, use the [open skills CLI](https://github.com/vercel-labs/skills) to install this single skill globally:

```bash
npx skills add jingkunxiong-crypto/cloudflare-pages-site-skill --skill cloudflare-pages-site -a codex -g
```

Or clone the repository directly into Codex's skills directory:

```powershell
git clone https://github.com/jingkunxiong-crypto/cloudflare-pages-site-skill.git "$env:USERPROFILE\.codex\skills\cloudflare-pages-site"
```

The repository root contains `SKILL.md`; keep its `references/` folder beside it. Clients other than Codex can install the same Agent Skill wherever they load `SKILL.md` folders.

## Use

Ask your agent, for example:

> Use `$cloudflare-pages-site` to build a website for this Android app, deploy it to Cloudflare Pages, connect `app.example.com`, and let visitors download the APK directly.

Or for an existing site:

> Use `$cloudflare-pages-site` to update this static site and verify the new version on mobile and on its custom domain.

The agent inspects the project first. For a plain HTML site it may upload `site/`; for a framework it should build and upload the output directory, such as `dist/`. It checks the current Cloudflare documentation when deployment settings or limits matter.

## Files

```text
SKILL.md                    Agent instructions and decision points
references/direct-upload.md  PowerShell commands, domain setup, downloads, verification
```

The reference includes an optional `_headers` example for versioned APK downloads. It is a guide, not a script that runs automatically.

## Publishing notes

- Cloudflare Pages Direct Upload and Git-connected deployment are distinct project setup choices. The skill follows an existing project's mode and uses Direct Upload only when it fits the request.
- A custom hostname must be associated with the Pages project; a DNS record by itself is not enough.
- The upload directory must contain only files intended to be public. Never place credentials or private source material in it.
- Successful upload is followed by a check of the real HTTPS hostname and the main user flow.

These points follow the [Cloudflare Direct Upload](https://developers.cloudflare.com/pages/get-started/direct-upload/), [custom domain](https://developers.cloudflare.com/pages/configuration/custom-domains/), and [response header](https://developers.cloudflare.com/pages/configuration/headers/) documentation. Cloudflare limits and Wrangler options can change; check the current docs before deploying.

## License

[MIT](LICENSE). Created by Sakuya_Jack. Not affiliated with Cloudflare or GitHub.
