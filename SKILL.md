---
name: cloudflare-pages-site
description: Build or update a static product website and publish its built files with Cloudflare Pages Direct Upload, including an optional custom domain and direct downloadable files. Use for landing pages, portfolios, and app showcase sites when the user wants this deployment route; not for Workers APIs or server-rendered applications.
---

# Cloudflare Pages Site

Use the user's product, audience, visual preferences, and existing repository as the brief. The Pianyu Studio site is an example of the workflow, not a template that every site must resemble.

## Decide the route

- Inspect the current project and identify the **deployable output directory**. For plain HTML it may be `site/`; for a framework, run its build and use its output such as `dist/`. Do not upload a source tree by assumption.
- Use Direct Upload when the user wants the local build published as-is. If the project already uses Git integration, or the user specifically wants automatic deployment from Git, keep that route. Direct Upload and Git integration are project-creation choices; check current Cloudflare documentation before creating a project.
- This skill covers a static public site. If the request needs login, server rendering, database, or dynamic API routes, choose the appropriate Pages Functions or Workers architecture before following the static deployment reference.

## Build and review the site

1. Establish the site's purpose, primary action, page order, content, imagery, and mobile behavior. Make design choices from the product rather than copying Pianyu's look. When a UI-design skill is available and applicable, use it.
2. Produce the actual site or update the existing one. Keep images appropriately sized for phones; use responsive variants, defer offscreen media, and preserve readable layouts and accessible controls. If an installer or other file should download directly, place it under a versioned path in the output directory and link to that path.
3. Preview the **built output**, inspect meaningful phone and desktop widths, and exercise the primary interaction. A successful build or upload is not evidence that the live site works.
4. Confirm the output contains no secrets, private source material, debug artifacts, or unintended files. Check current Pages upload limits before placing large binaries in the deployment directory.

## Publish and verify

For exact PowerShell commands and response-header examples, read [references/direct-upload.md](references/direct-upload.md) when deploying or giving a copyable deployment procedure.

- Before any Cloudflare mutation, identify the intended account, existing Pages project, production branch, target hostname, and whether the hostname already serves another site. Reuse an existing project when updating it.
- Follow the user's authorization for external changes. A request to deploy authorizes the deployment; a request only for a plan or guide does not.
- Use Wrangler or the connected Cloudflare capability available in the session. Check current official documentation for command flags, limits, and domain setup; do not rely on remembered values.
- Associate a custom hostname in the Pages project's **Custom domains** area before treating a DNS record as sufficient. Preserve any existing DNS/site use.
- After publishing, open the live hostname, check the main flow and responsive layout, and verify critical files and download links. For a downloadable installer, check its HTTP response and compare hashes when exact file identity matters.
- Report the live URL, what was deployed, what was checked, and any remaining limitation. Do not imply a deployment completed when only local files or a preview URL exist.
