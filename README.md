# ChromeWalk

**A real, private browser your AI can use, running on your own computer.**

Ask your AI assistant to open a page, check a site or collect data, and ChromeWalk does it in its own
browser on your machine. No cloud relay, no tunnel, and never your personal Chrome profile.

[Website](https://chromewalk.com) · [Docs](https://chromewalk.com/docs/) · [Download](https://chromewalk.com/download.html) · [FAQ](https://chromewalk.com/faq.html) · [Privacy](https://chromewalk.com/privacy.html) · [Terms](https://chromewalk.com/terms.html)

---

## What it does

- **Opens pages in a real browser**, including pages that block simple downloaders, and hands your AI the text,
  status and title. A challenge page is reported, never solved.
- **Takes screenshots and full evidence**: every screen size you need, every file a page loads, and the console and
  network log.
- **Checks your site for breakage**: quick self-checking tests across many pages and screen sizes, with 12 built-in
  problem detectors, plus scripted clicks and logins to reach the pages behind them.
- **Tests across browsers**: the same page in Chromium, Firefox and WebKit (Safari's engine), with the differences
  highlighted. You can also use your installed Chrome, Edge or Brave, always with ChromeWalk's own private profile.
- **Audits design and SEO**: colours, fonts, spacing and components across a site, an SEO check with ranked fixes,
  and visual comparisons against a saved version so you spot unexpected changes.
- **Collects store data**: products, prices, images, specs and filters from an online store, into JSON or CSV.
- **Stays light for your AI**: 35 tools with short descriptions, short results, and big outputs saved as files on
  your computer.

## Who it's for

Developers, QA and SEO teams, designers and researchers who already work with an AI assistant and want it to see
the real web.

**Works with AI apps that run on your computer.** One command, `cw connect`, sets ChromeWalk up in 16 of them:
Claude Desktop, Claude Code, Codex, Cursor, Windsurf, Cline, Gemini CLI, VS Code with GitHub Copilot, Zed, Continue,
GitHub Copilot CLI, Kiro, Amazon Q Developer, Kilo Code, Goose and Visual Studio (Windows only, so a Mac gets 15).
Any other app that supports local MCP servers can be set up by hand.

**Not yet:** chat apps that run in the cloud can't reach a program on your computer. That means ChatGPT (web,
desktop or phone), Claude on the web or phone, and the Gemini app. For OpenAI, use **Codex**: it works today.

**Runs on:** Windows 10 and 11 (x64), and macOS on Apple Silicon. There is no Intel Mac or Linux build yet.
Browsers: Chromium, Firefox and WebKit, plus your installed Chrome, Edge or Brave.

---

## Quick start

1. **Install ChromeWalk.**
   - Windows (PowerShell): `powershell -c "irm https://chromewalk.com/install.ps1 | iex"`
   - Mac (Terminal): `curl -fsSL https://chromewalk.com/install.sh | bash`
   - Claude Desktop on Windows: install the [ChromeWalk extension](https://chromewalk.com/download/claude) instead.
2. **Connect your AI app.** Run `cw connect` in a new terminal. It finds the AI apps on your computer and adds
   ChromeWalk to each one. Then restart the app.
3. **Sign up and activate.** Ask your AI: *"Sign me up for ChromeWalk with my email address."* The key arrives by
   email. Then: *"Activate ChromeWalk with the key from my email"*, or run `cw activate <key>` in a terminal.

Then try: *"Take phone and desktop screenshots of our home page."*

---

## Install ChromeWalk

Every client below talks to one ChromeWalk install on your computer. Both install scripts download the release
file, **check its SHA-256 against `SHA256SUMS.txt` and stop on a mismatch** (nothing is installed), install for
your user without admin rights, and tell you the next step.

**Windows:**

```powershell
powershell -c "irm https://chromewalk.com/install.ps1 | iex"
```

**Mac (Apple Silicon):**

```bash
curl -fsSL https://chromewalk.com/install.sh | bash
```

**Prefer an installer?** Download [ChromeWalk for Windows](https://chromewalk.com/download/windows)
(`ChromeWalk-Setup-Windows.exe`) or [ChromeWalk for Mac](https://chromewalk.com/download/mac)
(`ChromeWalk-macOS-AppleSilicon.pkg`), or pick them from the [download page](https://chromewalk.com/download.html).

<details>
<summary><b>Windows details: install folder, options, SmartScreen, uninstall</b></summary>

- The script installs the compiled `chromewalk.exe` (no Python, nothing to build) and a `cw.cmd` shortcut to
  `%LOCALAPPDATA%\Programs\ChromeWalk`, adds it to your **user** PATH, then runs `chromewalk setup`, which downloads
  the browsers ChromeWalk uses.
- Options (parameters, or environment variables set before the one-liner):
  - `-Version <version>` / `CW_VERSION` installs a specific release;
  - `-DryRun` / `CW_DRYRUN=1` downloads and verifies but installs nothing;
  - `-SkipSetup` / `CW_SKIP_SETUP=1`, `-InstallDir <folder>`, `-From <folder|url>`.
- **SmartScreen:** the Windows installer is not code-signed yet, so the first time you run it from a browser
  download, Windows may say "Windows protected your PC". Click **More info**, then **Run anyway**, or check the
  checksum first (below). The install script avoids the warning.
- **Uninstall:** Settings > Apps > Installed apps > ChromeWalk > Uninstall. This also removes ChromeWalk from your
  AI apps.

</details>

<details>
<summary><b>Mac details: install folder, options, Gatekeeper, uninstall</b></summary>

- The script installs for your user, with no admin password, into `~/.local/lib/chromewalk`, links `chromewalk`
  and `cw` into `~/.local/bin` (and adds that folder to `PATH` in `~/.zprofile`), then runs `chromewalk setup`.
- Options (after `bash -s --`):
  - `--version <version>` (or `CW_VERSION`) installs a specific release;
  - `--dry-run` downloads and verifies but installs nothing;
  - `--system` uses the package installer into `/usr/local` (asks for your admin password through `sudo`);
  - `--skip-setup`, `--no-path`, `--prefix DIR`, `--uninstall`.
- **Apple Silicon only** (M1 or later). Intel Macs are not supported.
- **The Mac package is unsigned** (no Apple Developer ID). The install script doesn't trigger Gatekeeper. If you
  download the `.pkg` in a browser and open it, macOS blocks it the first time ("cannot be opened because Apple
  cannot check it for malicious software"). Open **System Settings > Privacy & Security**, scroll to the message
  about the ChromeWalk package, click **Open Anyway**, and confirm with your password or Touch ID. Or install it
  from Terminal: `sudo installer -pkg ChromeWalk-macOS-AppleSilicon.pkg -target /` (system-wide:
  `/usr/local/lib/chromewalk`, `/usr/local/bin/chromewalk`, `/usr/local/bin/cw`).
- **Uninstall:** `bash install.sh --uninstall` (add `--system` for the package install). It also removes ChromeWalk
  from your AI apps (`chromewalk connect --remove`). Your licence, settings and downloaded browsers stay in
  `~/Library/Application Support/ChromeWalk` until you delete that folder.

</details>

**Updates:** the Windows app updates itself. Each update is signed and checked before it installs, and if the new
version fails its self-test your previous version comes back automatically (`cw update --defer <days>` waits a few
days first). On a Mac, `cw update --check` tells you when a new release is out; re-run the installer to update.
The Claude Desktop extension tells you when a new one is ready; install it over the old one.
[More about updates](https://chromewalk.com/docs/auto-update.html).

---

## Connect your AI app

> **The easy way:** run `cw connect` (or `cw connect <app>`, for example `cw connect codex`). It finds each AI app
> on your computer and adds ChromeWalk to its settings. It's safe to repeat, it saves a backup of a settings file
> before its first change, and it never writes your licence key into any app. Preview what it would do with
> `cw connect --dry-run --json`; undo with `cw connect --remove <app>`.
>
> **Setting it up by hand?** Every app uses the same launch command: **command `chromewalk`, args `["mcp"]`**.
> Don't use `cw`, and don't wrap it in a shell (`cmd /c` or otherwise). On Windows `cw` is a `.cmd` file, which AI
> apps can't start directly. If an app can't find `chromewalk` on PATH, use the full path:
> - Windows: `%LOCALAPPDATA%\Programs\ChromeWalk\chromewalk.exe`
> - Mac: `~/.local/bin/chromewalk` (install script) or `/usr/local/bin/chromewalk` (package installer)

<details>
<summary><b>Claude Desktop</b></summary>

**Windows: use the extension.**

1. Download it: https://chromewalk.com/download/claude (`ChromeWalk-Claude-Extension.mcpb`).
2. Double-click it, or drag it onto Claude Desktop's Settings > Extensions panel.
3. Paste your licence key into its optional License Key box, or leave it empty and ask Claude to sign you up.

**Mac:** the extension is Windows only for now. Install ChromeWalk, then run `cw connect claude-desktop`.

**Pick one.** If you use the extension, you don't need `cw connect claude-desktop` as well; doing both gives
Claude two copies of ChromeWalk. If that happens, run `cw connect --remove claude-desktop`.

Full walkthrough: https://chromewalk.com/docs/install-claude-desktop.html

</details>

<details>
<summary><b>Claude Code</b></summary>

Install the plugin from this repository's marketplace:

```
/plugin marketplace add chromewalk/app
/plugin install chromewalk@chromewalk
```

Or run `cw connect claude-code`, which runs `claude mcp add` with the full path to ChromeWalk; it registers
ChromeWalk for the folder you run it in. To make it available everywhere, add it by hand:

```
claude mcp add --scope user chromewalk -- chromewalk mcp
```

`--scope user` makes ChromeWalk available in every project, not only the folder you were in. A project-level
[`.mcp.json`](./.mcp.json) you can copy into your own repo is included.

Full walkthrough: https://chromewalk.com/docs/install-claude-code.html

</details>

<details>
<summary><b>OpenAI Codex (app, command line and editor extension)</b></summary>

Run `cw connect codex`. All three Codex apps read the same settings file, so one setup covers them. To add it by
hand, put this in `~/.codex/config.toml`:

```toml
[mcp_servers.chromewalk]
command = 'chromewalk'
args = ['mcp']
startup_timeout_sec = 30
tool_timeout_sec = 1200
```

Keep the two timeouts. Codex's own defaults (about 10 seconds to start and 60 seconds per tool call) are too short
for real-browser work, which can take several minutes. Use the full path if `chromewalk` isn't on PATH, for
example `'C:\Users\<you>\AppData\Local\Programs\ChromeWalk\chromewalk.exe'` or `'/usr/local/bin/chromewalk'`.

Full walkthrough: https://chromewalk.com/docs/install-codex.html

</details>

<details>
<summary><b>Cursor</b></summary>

Click **Add to Cursor**, or run `cw connect cursor`:

[![Add ChromeWalk to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=chromewalk&config=eyJjb21tYW5kIjoiY2hyb21ld2FsayIsImFyZ3MiOlsibWNwIl19)

To add it by hand, put this in `~/.cursor/mcp.json` or your project's `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "chromewalk": {
      "command": "chromewalk",
      "args": ["mcp"]
    }
  }
}
```

Cursor asks you once to trust the new tool server. Directory listing text:
[`docs/cursor-directory-listing.md`](./docs/cursor-directory-listing.md).

Full walkthrough: https://chromewalk.com/docs/install-cursor.html

</details>

<details>
<summary><b>VS Code with GitHub Copilot</b></summary>

Run `cw connect vscode`, or add this to your user or workspace `mcp.json`. VS Code uses `servers`, not
`mcpServers`:

```json
{
  "servers": {
    "chromewalk": {
      "type": "stdio",
      "command": "chromewalk",
      "args": ["mcp"]
    }
  }
}
```

Use Copilot in Agent mode. Full walkthrough: https://chromewalk.com/docs/install-vscode.html

</details>

<details>
<summary><b>Gemini CLI</b></summary>

Install the extension from this repository:

```
gemini extensions install https://github.com/chromewalk/app
```

It uses [`gemini-extension.json`](./gemini-extension.json) and [`GEMINI.md`](./GEMINI.md), and needs ChromeWalk
installed (above) so that `chromewalk` is on PATH. Or run `cw connect gemini-cli`, which adds ChromeWalk to
`~/.gemini/settings.json` for you.

Full walkthrough: https://chromewalk.com/docs/install-gemini-cli.html

</details>

<details>
<summary><b>Windsurf, Cline and Continue</b></summary>

Run `cw connect windsurf`, `cw connect cline` or `cw connect continue`, or add this block by hand:

```json
{
  "mcpServers": {
    "chromewalk": {
      "command": "chromewalk",
      "args": ["mcp"]
    }
  }
}
```

- Windsurf: `~/.codeium/windsurf/mcp_config.json`. Guide: https://chromewalk.com/docs/install-windsurf.html
- Cline: VS Code's `.../saoudrizwan.claude-dev/settings/cline_mcp_settings.json`. Guide: https://chromewalk.com/docs/install-cline.html
- Continue: `~/.continue/config.json`. Guide: https://chromewalk.com/docs/install-continue.html

</details>

<details>
<summary><b>Zed, GitHub Copilot CLI, Kiro, Amazon Q Developer, Kilo Code, Goose and Visual Studio</b></summary>

Run `cw connect` and it sets up each of these it finds. To do one app: `cw connect zed`, `cw connect copilot-cli`,
`cw connect kiro`, `cw connect amazon-q`, `cw connect kilo-code`, `cw connect goose` or `cw connect visual-studio`
(Visual Studio is Windows only). `cw connect --list` shows which apps are installed and which already have
ChromeWalk.

</details>

<details>
<summary><b>Any other MCP app</b></summary>

Use the launch command `chromewalk mcp` (not `cw`), with no shell around it, on Windows and Mac alike. This is
exactly what `cw connect` writes. `cw connect --dry-run --json` prints it and changes nothing.
Guide: https://chromewalk.com/docs/install-generic-mcp.html

</details>

<details>
<summary><b>If ChromeWalk doesn't show up in your AI app</b></summary>

1. Open the AI app once, then close it: `cw connect` only sees apps that have created their settings folder.
2. Run `cw connect --list`, then `cw connect <app>` again.
3. Fully quit and restart the AI app. Most apps only load their tools when they start.
4. Approve the new tool server if the app asks (Cursor, VS Code and Cline do).
5. Ask the app to "run the ChromeWalk doctor". If it answers, the connection works.
6. Still nothing? Run `cw mcp-selftest` in a terminal. It should end with "SELFTEST PASS".

</details>

**Skills:** ready-made instructions for Claude Code, Cursor and Codex (release checks, cross-browser testing,
onboarding and troubleshooting) are in [`skills/`](./skills/). Copy them into your agent's skills folder.

---

## Licence and activation

ChromeWalk is **proprietary software and needs a licence key**. There is no keyless trial: until a key is activated
on the computer, every tool that opens a browser or checks a page replies "license required" and tells your AI how
to sign you up.

**What works before you activate:** installing, connecting your AI apps, help on every tool, and these tools:
sign-up and activation (`register`, `activate`), the health check (`doctor`), process clean-up (`procs`, `reset`),
settings (`config`), the job list and cancel (`calls`, `cancel`) and saving a problem note (`recommend`).

**How to get and activate a key:**

1. **Sign up** at https://chromewalk.com/register.html, or ask your AI to sign you up (the `register` tool). The key
   is emailed to you and is tied to that email address. It is never shown in the chat.
2. **Activate** in any one of these ways:
   - ask your AI to activate it (the `activate` tool), or run `cw activate <key>` in a terminal if you'd rather keep
     the key out of the chat;
   - paste it into the Claude Desktop extension's License Key box;
   - set the `CHROMEWALK_LICENSE` environment variable (read when the ChromeWalk server starts; it activates the
     device if it isn't already);
   - for CI, point `CHROMEWALK_LICENSE_FILE` at a key file.
3. **Windows: activate once per computer.** `cw activate <key> --remember` keeps the key in Windows Credential
   Manager, and every other ChromeWalk install for your user activates itself.

**Good to know:**

- **Two devices per licence.** To move to a new computer, run `chromewalk license deactivate` on the old one (or
  reset the device from your account page), then activate the new one.
- **Online once.** Activation needs the internet. After that ChromeWalk renews in the background every 90 days, and
  keeps working offline for up to 90 days past a renewal it couldn't make. This grace period is only for an
  already-activated licence; it is not a trial. A cancelled licence stops working straight away.

Full terms: [`LICENSE`](./LICENSE) and https://chromewalk.com/LICENSE.txt. Terms of Use:
https://chromewalk.com/terms.html.

---

## Privacy

ChromeWalk runs on your computer. The pages it opens stay there.

**What stays on your computer:** page content, screenshots and captured files, file paths, your browser profiles,
logins and saved sessions, and your proxy and Nimble passwords.

**What ChromeWalk sends to chromewalk.com:**

- **Licence checks**: your key, the email you gave with it (if any), a salted hash of your machine ID, your install
  ID, ChromeWalk version and operating system, sent to the licence server when you activate and when it renews.
- **Update checks**: your release channel and installed version.
- **Usage events**, linked to your licence. These are **a condition of the licence and can't be turned off**. Each
  one carries:
  - an install ID, a salted hash of your machine ID, and a SHA-256 hash of your licence key (`license_hash`), never
    the key itself;
  - which tool ran, whether from the command line or an AI app, its exit code and a rough duration, and for an
    error only its type and a fixed failure label;
  - the **names** of the options used, never their values;
  - **the web address the tool worked on and its domain**, with logins, the port, the query string, anything after
    `#` and secret-looking path parts removed on your computer first;
  - your ChromeWalk version, operating system and CPU type, which browser engines are installed, CPU cores and a
    memory range; at install and activation also your language, time-zone offset, OS build, whether ChromeWalk runs
    as administrator, a disk-space range and how many days ago it was installed.
- **Feedback**, only when you send it with `chromewalk feedback`.

**Never sent:** page content, screenshots, files, file paths, option values, query strings, passwords, proxy or
Nimble credentials, your user or machine name, or your licence key in usage events.

**Only if you turn it on:** with the Nimble network option, your browser's traffic goes through your own Nimble
account; with the Nimble cloud fallback, Nimble opens the page for you.

Run `cw telemetry status` to see exactly what your install sends. Full field list:
https://chromewalk.com/docs/privacy-telemetry.html. Privacy Policy: https://chromewalk.com/privacy.html.

---

## Verify your download

Every [GitHub Release](https://github.com/chromewalk/app/releases) has one `SHA256SUMS.txt` with the SHA-256 of every
release file: `ChromeWalk-Setup-Windows.exe`, `ChromeWalk-Claude-Extension.mcpb`, `ChromeWalk-macOS-AppleSilicon.pkg`,
`install.ps1` and `install.sh`. The install scripts check this for you; to check by hand, compare the hash, read the
script, then run it.

**Windows:**

```powershell
irm https://chromewalk.com/SHA256SUMS.txt -OutFile SHA256SUMS.txt
irm https://chromewalk.com/install.ps1 -OutFile install.ps1
(Get-FileHash install.ps1 -Algorithm SHA256).Hash.ToLower()      # must equal the install.ps1 line in SHA256SUMS.txt
Select-String install.ps1 SHA256SUMS.txt
powershell -ExecutionPolicy Bypass -File .\install.ps1 -DryRun   # verifies the installer, installs nothing
powershell -ExecutionPolicy Bypass -File .\install.ps1
```

**Mac:**

```bash
curl -fsSLO https://chromewalk.com/SHA256SUMS.txt
curl -fsSLO https://chromewalk.com/install.sh
shasum -a 256 install.sh && grep install.sh SHA256SUMS.txt       # the two hashes must match
bash install.sh --dry-run                                        # verifies the package, installs nothing
bash install.sh
```

**Installer files:** download the file and https://chromewalk.com/SHA256SUMS.txt, then compare
`Get-FileHash <file> -Algorithm SHA256` (Windows) or `shasum -a 256 <file>` (Mac) with its line in `SHA256SUMS.txt`.

---

<details>
<summary><b>The 35 tools</b></summary>

Help works on every tool before you activate (`help: "brief"` or `help: "full"`); it never runs the tool.

| Tool | What it does | Before activation |
|---|---|---|
| `register` | Emails you a licence key | Yes |
| `activate` | Activates your licence key (never echoed back) | Yes |
| `fetch` | Opens a page in a real browser and returns its text, status and title | |
| `screenshot` | Screenshots of a page, a list of pages, or many screen sizes | |
| `render` | Shows a page in Chromium, Firefox and WebKit and highlights pixel differences | |
| `webinspect` | Console errors, network log, loading speed (Core Web Vitals) and basic accessibility | |
| `pwa` | Checks whether a web app is installable and works offline | |
| `notify` | Tests a site's browser-notification prompt | |
| `jsonquery` | Searches, flattens and counts JSON data, without a browser | |
| `api` | Runs a set of API tests in parallel, slowing down if the server asks | |
| `flow` | Follows scripted clicks and typing to reach logged-in pages | |
| `dump` | Saves everything a page loads, plus screenshots | |
| `drive` | Controls a live ChromeWalk browser window: go to, read, run, screenshot | |
| `smoke` | Quick, self-checking tests of many pages and screen sizes at once | |
| `harvest` | Collects a store's products (prices, images, specs, fitment) into JSON or CSV | |
| `facets` | Lists a store's filters and their options | |
| `xengine` | Runs the same checks in Chromium, Firefox and WebKit and reports which differs | |
| `resources` | Shows memory and CPU, and how many browsers can safely run | |
| `sites_list` | Lists your saved site presets | |
| `logs` | Reads ChromeWalk's event log | |
| `doctor` | Health check of the installation | Yes |
| `procs` | Lists ChromeWalk's own browsers and closes abandoned ones | Yes |
| `schema` | Reads and checks a page's structured data (JSON-LD, microdata, RDFa, Open Graph) | |
| `cms` | Detects the platform a site runs on | |
| `config` | Shows ChromeWalk's settings | Yes |
| `compare` | Compares two pages, or a page with a saved version, across visual, content, SEO, design and more | |
| `design` | Site-wide design audit: colours, type, spacing, components and drift | |
| `audit` | Consistency and SEO audit in one pass, with ranked fixes | |
| `baseline` | Saves how pages look and flags unexpected visual changes later | |
| `probe` | Checks, without a browser, whether a site is reachable, blocks automated visits or needs a login | |
| `targets` | What ChromeWalk has learned about each site from earlier runs | |
| `reset` | Soft reset: closes stuck browsers and clears stale locks | Yes |
| `recommend` | Saves a problem note on your computer (secrets removed) | Yes |
| `calls` | Lists running and recent jobs | Yes |
| `cancel` | Stops a running job and its browsers | Yes |

Per-tool reference: https://chromewalk.com/docs/ (one `tool-*.html` page per tool, for example
https://chromewalk.com/docs/tool-fetch.html).

</details>

<details>
<summary><b>Command line and exit codes</b></summary>

Everything your AI can do is also a terminal command (`chromewalk <command>`, or `cw <command>`), for scripts and CI.
`--json` prints pure JSON. Every command shares one set of exit codes:

| Code | Meaning |
|---|---|
| 0 | OK |
| 2 | Usage or bad arguments |
| 3 | Checked failure: the tool ran and the target failed a check; still a valid result |
| 4 | Licence required |
| 5 | Infrastructure failure (browser launch or a missing dependency); retry, or run `doctor` |
| 6 | Timed out or cancelled |
| 1 | Unexpected internal error |

Release notes for each version: [CHANGELOG.md](./CHANGELOG.md) and the
[GitHub Releases](https://github.com/chromewalk/app/releases).

</details>

<details>
<summary><b>What's in this repository</b></summary>

This is the public listing and install package for ChromeWalk. It holds no source code, no signing keys and no
server code; the compiled installers are attached to [GitHub Releases](https://github.com/chromewalk/app/releases).

```
README.md                        - this file
LICENSE                          - licence notice (full EULA at chromewalk.com/LICENSE.txt)
SECURITY.md                      - how to report a vulnerability
CHANGELOG.md                     - where to find the release notes
server.json                      - MCP Registry server descriptor
gemini-extension.json            - Gemini CLI extension manifest
GEMINI.md                        - context file loaded by the Gemini CLI extension
.mcp.json                        - example Claude Code project MCP config
.claude-plugin/plugin.json       - Claude Code plugin manifest
.claude-plugin/marketplace.json  - Claude Code marketplace listing
skills/                          - skills for Claude Code, Cursor and Codex
.github/ISSUE_TEMPLATE/          - bug report and feature request templates
docs/cursor-directory-listing.md - Cursor directory submission text
```

</details>

---

## Support and links

- Website: https://chromewalk.com
- What is ChromeWalk: https://chromewalk.com/docs/what-is-chromewalk.html
- Docs: https://chromewalk.com/docs/
- FAQ: https://chromewalk.com/faq.html
- Install or packaging problems: this repository's [issue tracker](https://github.com/chromewalk/app/issues)
- Licence or account questions: the contact details in the [Privacy Policy](https://chromewalk.com/privacy.html)
  and [Terms of Use](https://chromewalk.com/terms.html)
- Security: [`SECURITY.md`](./SECURITY.md)
- Licence: [`LICENSE`](./LICENSE)
