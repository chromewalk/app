# Changelog

Release notes for ChromeWalk live with each [GitHub Release](https://github.com/chromewalk/app/releases) —
every tagged release (`vX.Y.Z`) has its own notes describing what changed, alongside the
downloadable per-platform `.mcpb` bundle(s) (`ChromeWalk-Claude-Extension.mcpb` for
Windows x64) and platform installers.

This repository is a listing/install package, not the ChromeWalk source tree, so it does not
carry the full internal development changelog (which tracks build tooling, internal
refactors, and packaging work across many pre-release forks). What you get here is:

- The current `server.json` / `gemini-extension.json` / plugin manifests, versioned to match
  the latest release.
- The **user-facing** highlights for that release, copied into each GitHub Release's notes.

## 14.0.1

Organisation move and polish. This repository is the new home of the ChromeWalk downloads. Release files now have stable,
readable names (`ChromeWalk-Setup-Windows.exe`, `ChromeWalk-Claude-Extension.mcpb`, `ChromeWalk-macOS-AppleSilicon.pkg`,
`install.ps1`, `install.sh`, `SHA256SUMS.txt`). Downloads and install commands come from `chromewalk.com` (`/install.ps1`,
`/install.sh`, `/download/windows`, `/download/claude`, `/download/mac`, `/SHA256SUMS.txt`); the installers still verify every
file against the release's `SHA256SUMS.txt`. Quieter output: a clean page walk prints nothing, and `screenshot --viewports` works for local page
files too.

## 14.0.0

**Measure the page, walk it politely, keep your AI's context small.** Still 35 MCP tools; every new feature is an option on a
tool you already have.

- **Content and SEO** - word, sentence and readability counts and image alt text on every audit; `compare --prose` (shingle
  overlap, containment, longest shared passages); `audit --dedupe` (near-duplicate pages, duplicate titles / descriptions / H1s,
  merge the results of several agents); `audit --layers seo` with `--check-links` (robots.txt, sitemap, llms.txt, canonical,
  hreflang, Open Graph, structured data, image SEO, a score and ranked fixes).
- **Smart file names and a navigation log** - captures are named `{seq}_{host}_{path}_{title}_{viewport}` and a
  `nav-log.jsonl` maps every file to its URL, referrer and step (`--name-template`, `--no-smart-names` for the old names).
- **One error collector for every walk** - hidden breaks (API 4xx/5xx, broken images, 404 scripts, CSP, mixed content, long
  tasks) and a WAF verdict in `result.walk`, `--walk-profile scrape|audit|parity`, polite per-host pacing, `--interact` /
  `--keep-alive` and stale-page detection. No humanised or randomised behaviour.
- **Proxy and Nimble** - `--proxy`, and an opt-in `--network nimble` (your own Nimble account) with geo-targeting; the engine
  parity matrix in the docs on chromewalk.com.
- **Lean for your AI** - the tool list is about a tenth of its former size (one line per tool, core arguments, an `advanced`
  object), `help: "brief" | "full"` on every tool, short results with file paths, and an idle-free server.
- **Zero zombies** - a hard-killed run takes its Chromium, Firefox and WebKit browsers with it.
- Behaviour changes (file names, always-on pacing, `--interact` in the audit/parity profiles, new report fields) are listed in
  the release notes.

## 13.0.0

**Test the flows, not just the pages.** 35 MCP tools (was 33).

- **Sign up and activate inside the chat** - new tools `register` (emails your license key; it is never
  shown in the chat) and `activate` (never echoes the key); both work without a license, and a "license
  required" answer now tells the assistant to offer them. The Claude Desktop extension's license key field
  is optional.
- **Activate once per computer** - `cw activate <key> --remember` stores the key in Windows Credential
  Manager; other installs for the same Windows user activate themselves. `CHROMEWALK_LICENSE_FILE` reads
  the key from a file (CI). `cw connect` never writes a key into an AI client's config.
- **Setup finishes the job** - it installs the Firefox / WebKit / Chromium test engines too and ends with
  a `doctor` verdict.
- **Click-flow coverage in smoke** - per-step `expect` (element present / gone, text, URL change),
  automatic checks after every step (new errors, failed requests), `explore` (clicks up to N safe controls
  per page - never delete, log out, pay, submit, form submits, downloads or other sites), and a per-page
  `coverage` report. **A failed step now fails the page** (it used to be logged only).
- **Logged-in cross-engine testing** - `xengine` and `render` accept `session`, `auth` and
  `local_storage_file`; `xengine` runs steps, `expect`, `explore` and `coverage` on Chromium, Firefox and
  WebKit.
- **Microsoft Edge and Brave** - `browser` = `chrome` | `edge` | `brave` | `cft` (default) | a path, on
  smoke, fetch, dump, screenshot, compare, design, audit and webinspect; ChromeWalk's own private profile,
  never yours. An unknown `CHROMEWALK_BROWSER` value is now an error instead of a silent fallback.
- **Reliability** - a hard-killed run no longer leaves its Chrome running; design and audit runs above 160
  states run in batches with checkpoints and `resume`; smoke / design / audit report phase timings;
  `doctor` lists the install folders (warns on a mixed install) and explains slow starts.
- **API redaction** - secret-named fields inside JSON strings, escaped JSON fragments and XML elements are
  masked too.
- **More clients for `cw connect`** - GitHub Copilot CLI, Kiro, Amazon Q Developer, Kilo Code, Goose,
  Visual Studio.
- **Skills** for Claude Code, Cursor and Codex: the [`skills/`](./skills/) folder of this repository.

## 12.0.0

**Feedback-driven polish + auto-update.** 33 MCP tools.

- **New tool `audit`** - a one-pass UI-uniformity audit: every page ranked by uniformity against a
  reference page, an idiom census (which sections skip your approved components), element crops and a
  component sheet, design-token compliance per CSS selector, responsive differences between viewports,
  and light/dark themes - as one `audit.md` + `audit.json` + `shots/` bundle.
- **New tool `baseline`** - stored-baseline visual regression per page and per selector (SSIM), with a
  gate for the pages a change should NOT have touched.
- **Smoke tells slow from failed** - a clean but slow page is reported `slow`, not failed; every failed or
  slow page gets an artifact bundle (screenshot, console, failed requests, DOM, timings); runs can be
  limited to the pages a change touches (`changed` / `git_diff` with a route map) and pointed at another
  environment (`base_url`); fewer infrastructure flakes on busy or slow-disk machines.
- **The same pre-capture hooks and browser options everywhere** - wait for a selector or a quiet network,
  run a script, set an attribute (e.g. a dark theme), ignore known console noise; language, time zone,
  device scale, reduced motion, third-party blocking, CPU / network throttling, and `auth` (a
  localStorage token, never logged).
- **Headless by default** - no browser window pops up unless you ask for one (`headful`).
- **The Windows app updates itself** - signed and checksum-verified, self-tested with a real page load
  after installing, rolled back automatically if that fails. The check always runs; applying a release
  can be deferred up to 30 days. Claude Desktop extension users get a one-line notice when a newer
  version exists.
- **API testing** - an expected status (e.g. 503) is never treated as a block; secret values in responses
  are redacted everywhere unless you explicitly opt in to raw bodies.
- **Telemetry** - required by the license with no opt-out; each tool run now reports the target URL
  (scheme, host and path; credentials, query strings and fragments are removed on your computer first)
  and its domain. Page content, option values, file paths and credentials are never sent. Telemetry
  carries only a SHA-256 hash of your license key (`license_hash`), never the key itself. See the
  updated [Privacy Policy](https://chromewalk.com/privacy.html).
- **Docs** - end-to-end recipes (`chromewalk help recipes`: UI uniformity audit, release-gate smoke,
  cross-engine check, authed capture, reading design results) and a "Related tools" footer on every tool
  page.

## 11.0.0

**Design-drift release.** Answers "did the UI/UX drift?" across pages, templates, sites and
time, even when the CSS itself is different.

- New tool: `design` - a site-wide UI/UX audit over a URL list or a sitemap: a design-token
  inventory (palette with near-duplicate colour clusters, type scale, spacing grid, radii,
  shadows, breakpoints, CSS custom properties that differ between pages), conformance against
  your own design-token file (W3C Design Tokens / Style Dictionary), component variant clusters
  with a gallery of element crops, UX heuristic failures (WCAG 2.2 contrast incl. dark mode, tap
  targets, visible focus, overflow, layout shift, above-the-fold call to action, alt text,
  heading order), drift against a saved baseline, and a score per page. HTML and JUnit reports;
  `fail_on` gates the exit code (error, warn, drift or none).
- `compare` gains opt-in design layers - tokens, components, css, ux and perceptual (SSIM
  heatmap) - with `layers=design` as the preset, plus the capture options `css`, `coverage`,
  `states` (forced hover/focus/active/disabled) and `color_schemes` (dark mode).
- Smoke suites gain a page check `{"design": {"baseline": NAME, "fail_on": "error"}}`.
- Snapshots are schema version 2 (additive: version 1 snapshots and baselines keep working).

## 10.0.0

**Reliability release.** MCP calls now run concurrently instead of serially; a browser-layer
overhaul (pooled profiles, launch slots, an orphan reaper) plus one error/exit-code contract
for every tool close out the field reports about failed launches, orphaned browser processes,
and hung calls wedging the server.

- MCP calls run concurrently: a hung or cancelled call no longer blocks the calls behind it, and
  its whole browser process tree is always cleaned up. 8 pipelined calls that used to run fully
  serial (p50 24.4s, max 47.4s) now overlap (p50 8.9s, max 15.1s); a forced tool timeout that
  used to wedge the server with 10-14 browser processes left now returns a clean error in 5-10s
  with none left behind.
- New tools: `compare` (page/baseline diff across visual, DOM, text, network, schema, SEO and
  more layers), `doctor` (read-only environment health check), `procs` (browser-process census
  with orphan cleanup), `schema` (structured-data validation/diff), `cms` (platform fingerprint),
  and `config` (view every tuning setting and its source). The diagnostics work without a
  license.
- One exit-code contract for every tool, CLI and MCP: `0` ok, `2` usage error, `3` checked
  failure (still a valid result), `4` license required, `5` infrastructure failure, `6` timed
  out or cancelled.
- An unreachable page is now reported as a real failure (`3`, not a false success against the
  browser's own error page), and navigation now actually honors your timeout.
- Security hardening: closed a license-bypass path in the compiled build's `help` command;
  compiled-build endpoint overrides now only take effect on loopback addresses; the signature
  verifier runs a startup self-test before it is trusted.

For what's new in the current release, see the latest entry at
https://github.com/chromewalk/app/releases/latest.
