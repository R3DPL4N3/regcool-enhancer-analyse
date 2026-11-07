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

On 7 November 2026, a live browser visit to `gerhodd[.]com/` displayed a security check and then navigated to a result page under `/d-93af11e3758d/result/`. Its HTML contained a `meta http-equiv="refresh"` element with a zero-second delay pointing to `hxxps://github[.]com/Anas-Ak11/super-octo-sniffle/releases/download/best/Latest-Release.zip`. This causes the browser to navigate automatically to the ZIP download URL; the redirect was observed in the page HTML, but the resulting download and its contents were not independently verified in this live check. The original encoded HTML does not directly reference `gerhodd[.]com`, so the intermediate link from that page to this site and the full archive-to-executable chain remain unverified. See the [abuse report](reports/github-abuse-report-en.txt) for the earlier evidence and limitations.
