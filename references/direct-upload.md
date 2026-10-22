# Pages Direct Upload: copyable procedure

Use this reference only for a static Pages deployment or when the user wants a reusable deployment guide. Replace every example project, directory, hostname, and download filename. Check the linked Cloudflare documentation for current limits and flags before use.

## 1. Output directory and local inspection

The uploaded directory should contain the final `index.html` at its root. A plain site can use `site/`; a Vite or similar project should deploy its build output, commonly `dist/`. Do not include credentials or working files.

```powershell
# Example for plain static files, from the project root:
python -m http.server 4173 --directory site
# Open http://localhost:4173/
```

Review the main flow at narrow phone, typical phone, and desktop widths. Inspect image transfer sizes, text overlap, menus, links, motion, and reduced-motion behavior when relevant.

Before adding large downloads, inspect files in the output directory. Cloudflare's [Direct Upload documentation](https://developers.cloudflare.com/pages/get-started/direct-upload/) gives the current per-file and file-count limits. If a file exceeds them, consider R2 with a custom download domain rather than a Pages static asset; verify the current [R2 public bucket guidance](https://developers.cloudflare.com/r2/buckets/public-buckets/).

```powershell
$OutputDir = 'site'
Get-ChildItem $OutputDir -File -Recurse |
  Sort-Object Length -Descending |
  Select-Object -First 10 FullName, Length
```

## 2. Authenticate and create a project once

```powershell
npx --yes wrangler@latest login
npx --yes wrangler@latest whoami

$Project = 'my-project'
npx --yes wrangler@latest pages project create $Project --production-branch main
```

Check account selection if more than one Cloudflare account is available. In a PowerShell session, `$env:CLOUDFLARE_ACCOUNT_ID = 'account-id'` can select a specific account when needed; never store access tokens in site files. If the project already exists, skip creation. Direct Upload projects are not simply switched to Git integration later, so choose the deployment mode deliberately. See [Direct Upload](https://developers.cloudflare.com/pages/get-started/direct-upload/) and [Wrangler Pages commands](https://developers.cloudflare.com/workers/wrangler/commands/pages/).

## 3. Deploy or update

```powershell
$Project = 'my-project'
$OutputDir = 'site'  # Use the actual build output, e.g. dist
npx --yes wrangler@latest pages deploy $OutputDir --project-name $Project --branch main
```

The branch should match the project's production branch. Keep the deployment URL reported by Wrangler. For future updates, repeat this deployment command after inspecting the new output. A Git dirty warning is a reminder to inspect what is being uploaded, not a failure.

## 4. Custom hostname

In Cloudflare Dashboard: **Workers & Pages → project → Custom domains → Set up a domain**. Enter the intended hostname, e.g. `app.example.com`, and complete activation. If its DNS zone is already in the same Cloudflare account, Cloudflare can add the CNAME during setup. With external DNS, create the CNAME requested by Pages, typically targeting `<project>.pages.dev`. Check for an existing record or service on that hostname first. Merely creating the CNAME without associating the domain in Pages is insufficient. Wait for the domain and certificate to show active, then open the HTTPS URL. See [Pages custom domains](https://developers.cloudflare.com/pages/configuration/custom-domains/).

## 5. Same-domain downloads

Place a versioned file in the build output, for example `site/download/MyApp-v1.0.apk`, and link to `/download/MyApp-v1.0.apk`. For an APK, an optional `site/_headers` rule is:

```text
/download/MyApp-v1.0.apk
  Content-Type: application/vnd.android.package-archive
  Content-Disposition: attachment; filename="MyApp-v1.0.apk"
  Cache-Control: public, max-age=31536000, immutable
```

Only use long immutable caching if a changed binary gets a **new filename**. `_headers` must be in the deployed output directory and applies to static asset responses; Pages Functions need their own response headers. See [Pages headers](https://developers.cloudflare.com/pages/configuration/headers/).

## 6. Live verification

```powershell
$Base = 'https://app.example.com'
(Invoke-WebRequest -Uri "$Base/" -UseBasicParsing).StatusCode
(Invoke-WebRequest -Uri "$Base/download/MyApp-v1.0.apk" -Method Head -UseBasicParsing).StatusCode
```

Inspect the live page visually on a phone-sized viewport and test its main action. If installer identity matters, download a temporary copy and compare its SHA-256 against the local file:

```powershell
$LocalFile = '.\site\download\MyApp-v1.0.apk'
$RemoteFile = Join-Path $env:TEMP 'MyApp-v1.0-online-check.apk'
Invoke-WebRequest -Uri "$Base/download/MyApp-v1.0.apk" -OutFile $RemoteFile
(Get-FileHash $LocalFile -Algorithm SHA256).Hash
(Get-FileHash $RemoteFile -Algorithm SHA256).Hash
```

The two hashes should match. A Pages deployment success message is not a substitute for checking the custom hostname and the file actually served there.
