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
