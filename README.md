# RegCool Enhancer Analysis

This repository documents the analysis of a suspicious RegCool Enhancer distribution.

## Contents

- [C2 analysis report](reports/c2-20261107.txt) — Turkish technical report describing observed behavior, indicators, and experimental limitations.
- [GitHub abuse report](reports/github-abuse-report-en.txt) — English summary of the distribution chain and findings.
- [Original encoded index.html](evidence/original-index.html) — preserved HTML from the investigated repository. Treat it as untrusted content and do not open it in a normal browsing environment.

## Malware archive

The [Releases section](../../releases) contains `regcool-enhancer.zip`, an archive containing the malicious file for analysis. Handle the archive only in an isolated analysis environment.

Archive SHA-256: `372b131d162c12e5fc895d547aee5d523880f0f5a00e1bc5463851451d814958`

The analyzed executable, `release-x64-clean.exe`, is identified in the report by SHA-256 `b0b01b8dd5a7f2e92b90e07b49a46ef9d3f9d5f719ffe5fb8ed463c2d379b74f`. This is the executable hash, not the archive hash.

## Analysis scope

Network behavior was observed in a controlled lab after environment checks were overridden in a debugger. The report does not establish successful exfiltration to the real endpoint or receipt of remote commands.

## Observed distribution chain

The investigated repository's encoded HTML was decoded statically. The decoded page presents a PowerShell command that downloads and immediately executes content from `hxxps://activation-gateway[.]cc/update/ps-release`, including a variant using an execution policy bypass. It also advises pausing real-time antivirus protection if the installer is blocked. These instructions were not executed during the static analysis.

On 7 November 2026, a live browser visit to `gerhodd[.]com/` displayed a security check and then navigated to a result page under `/d-93af11e3758d/result/`. Its HTML contained a `meta http-equiv="refresh"` element with a zero-second delay pointing to `hxxps://github[.]com/Anas-Ak11/super-octo-sniffle/releases/download/best/Latest-Release.zip`. This causes the browser to navigate automatically to the ZIP download URL; the redirect was observed in the page HTML, but the resulting download and its contents were not independently verified in this live check. The previously preserved HTML version does not directly reference `gerhodd[.]com`; an earlier version did, as documented below. The full archive-to-executable chain remains unverified. See the [abuse report](reports/github-abuse-report-en.txt) for the earlier evidence and limitations.

### Historical redirect from the project page

An earlier `index.html`, present at source commit `caf90b41be6f9947b0beb91dd8da294e45fcfa6e` (5 November 2026) and preserved [here](evidence/index-redirect-2026-11-05.html), contains six Base64 chunks decoded with a repeating-key XOR. Its decoded HTML sets `TARGET` to `hxxps://gerhodd[.]com/` and schedules `window.location.href` to that target after 6,060 ms. Multiple navigation and download links also point there. This establishes the previously missing direct connection for that historical version.

The live GitHub Pages response checked on 7 November 2026 was the later blob `1c3b7af0ae8e21147af13c40ea4009eaa34c6aca` (commit `f15265be3aedb5647e34d7d223eb80e89e88a720`, SHA-256 `827ccde5d2d4676db41a4855a90444bdc89ff53f3f592f8b4754361b0b984923`). Its decoded page advertises a PowerShell command for `activation-full[.]cc`, rather than the earlier automatic `gerhodd[.]com` redirect. In the browser used for this check, the page remained blank and the script produced a `document.body` null error; no redirect to `gerhodd[.]com` was observed. Behavior may vary by delivery or environment, but the historical redirect is directly supported by the decoded code.
