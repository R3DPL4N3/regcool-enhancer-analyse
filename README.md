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

The acquisition flow was observed reaching `gerhodd[.]com` as a download destination. The preserved HTML does not directly reference that domain, and the intermediate redirect or script stage has not been captured independently. The exact automatic download mechanism on that site and the complete chain from the original downloaded archive to the analyzed executable therefore remain unverified. See the [abuse report](reports/github-abuse-report-en.txt) for the evidence and limitations.
