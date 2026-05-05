# IPMG

<section class="ipmg-hero">
  <div class="ipmg-hero__copy">
    <div class="ipmg-brand">
      <span class="ipmg-brand__icon">📡</span>
      <span>IPMG Network Scanner</span>
    </div>
    <p class="ipmg-kicker">Enterprise IP management and ping monitoring</p>
    <h1>Find live hosts, resolve networks, and export clean reports in minutes.</h1>
    <p class="ipmg-lede">
      IPMG is a modern Python CLI for network administrators, cybersecurity teams,
      DevOps engineers, and SREs who need fast, repeatable visibility across IP
      ranges, spreadsheets, CIDR blocks, and local subnets.
    </p>
    <div class="ipmg-actions">
      <a class="ipmg-button ipmg-button--primary" href="#installation">Install IPMG</a>
      <a class="ipmg-button ipmg-button--ghost" href="https://github.com/sameeralam3127/ipmg">View on GitHub</a>
    </div>
  </div>
  <div class="ipmg-terminal" aria-label="IPMG terminal preview">
    <div class="ipmg-terminal__bar">
      <span></span><span></span><span></span>
      <strong>ipmg scan</strong>
    </div>
    <pre><code>$ ipmg --discover --resolve --formats csv xlsx
Discovering local subnet... 192.168.1.0/24
Scanning 254 hosts with parallel workers

Status        Count
Active        132
Inactive      12
Unreachable   4
Timeout       2

Active Rate: 88.00%
Scan Duration: 6.24s</code></pre>
  </div>
</section>

!!! warning "Authorized networks only"
    Do not use IPMG on networks without explicit authorization. Always obtain written approval from your organization's Cybersecurity or Network Security team before scanning.

<nav class="ipmg-page-toc" aria-label="Page contents">
  <strong>On this page</strong>
  <a href="#why-teams-use-ipmg">Why Teams Use IPMG</a>
  <a href="#product-highlights">Product Highlights</a>
  <a href="#installation">Installation</a>
  <a href="#common-commands">Common Commands</a>
  <a href="#sample-output">Sample Output</a>
  <a href="#input-formats">Input Formats</a>
  <a href="#output-format">Output Format</a>
  <a href="#troubleshooting">Troubleshooting</a>
  <a href="#macos-gui-beta">macOS GUI Beta</a>
</nav>

## Why Teams Use IPMG

<div class="ipmg-grid">
  <article>
    <strong>Fast subnet discovery</strong>
    <p>Automatically detect and scan local network ranges, or pass a single IP, text file, CSV, Excel file, or CIDR block.</p>
  </article>
  <article>
    <strong>Parallel monitoring</strong>
    <p>Thread-pool pinging keeps large scans moving while recording failures as structured results instead of stopping the run.</p>
  </article>
  <article>
    <strong>Report-ready exports</strong>
    <p>Generate XLSX, CSV, and JSON outputs with batch timestamps, duration, latency, hostname, and status metadata.</p>
  </article>
  <article>
    <strong>Built for automation</strong>
    <p>Use scheduled recurrent scans, CLI flags, and CI-friendly project structure for repeatable network checks.</p>
  </article>
</div>

!!! tip "Best fit"
    IPMG is ideal for network admins, systems engineers, cybersecurity teams, DevOps teams, and SREs who need clear status reporting without building a custom scanner.

## Product Highlights

- Subnet auto-discovery
- Parallel pinging with thread pools
- Hostname resolution through PTR lookups
- Multi-format reporting in XLSX, CSV, and JSON
- Flexible target input from `.xlsx`, `.csv`, `.txt`, `.list`, literal IPs, and CIDR blocks
- Scheduled recurrent scans with `--interval`
- Auto-generated sample input files
- Colorized CLI output with rich console panels, progress bars, and status summaries
- Batch-level scan timestamps and duration tracking
- Modular, testable Python package architecture

## Installation

### Option 1 — Install from PyPI

```bash
pip install ipmg
ipmg --help
```

### Option 2 — Install with uv

```bash
uv tool install git+https://github.com/sameeralam3127/ipmg.git
ipmg --help
```

### Option 3 — Development install

```bash
git clone https://github.com/sameeralam3127/ipmg.git
cd ipmg
pip install -e .
ipmg --help
```

### Option 4 — Install with curl

```bash
curl -sSL https://raw.githubusercontent.com/sameeralam3127/ipmg/main/install.sh | bash
ipmg --help
```

!!! note "Installer behavior"
    The installer checks for `uv`, installs it when needed, and installs IPMG globally through `uv`.

## Common Commands

| Use Case | Command | Description |
| --- | --- | --- |
| Basic scan | `ipmg` | Runs with default config and creates a sample input file if missing. |
| Excel scan | `ipmg --input network_devices.xlsx` | Scans IPs from an Excel file with an `IP Address` column. |
| CSV scan | `ipmg --input network_devices.csv` | Scans IPs from a CSV file with an `IP Address` column. |
| Text scan | `ipmg --input targets.txt` | Scans one target per line. Blank lines and comments are ignored. |
| Single host | `ipmg --input 8.8.8.8` | Scans a literal IP address. |
| CIDR range | `ipmg --input 192.168.1.0/24` | Expands the CIDR block into host IPs and scans them. |
| LAN discovery | `ipmg --discover` | Automatically detects and scans devices in the local subnet. |
| CSV + XLSX export | `ipmg --formats csv xlsx` | Exports scan results in CSV and Excel formats. |
| Hostname lookup | `ipmg --resolve` | Performs reverse DNS lookups for hostnames. |
| Scheduled scan | `ipmg --interval 10` | Repeats the scan every 10 minutes. |

## Sample Output

```text
IPMG Summary

Status        Count
Active        132
Inactive      12
Unreachable   4
Timeout       2

Batch Timestamp: 2026-04-09 11:42:13
Total Hosts: 150
Active Rate: 88.00%
Completion Rate: 90.67%
Scan Duration: 6.24s
```

## Input Formats

IPMG accepts targets from Excel files, CSV files, plain text files, literal IPs, and CIDR blocks.

For Excel and CSV inputs, include an `IP Address` column:

| IP Address |
| --- |
| 192.168.1.1 |
| 10.0.0.1 |
| 10.0.1.0/30 |

Text files can contain one target per line:

```text
# Production DNS
8.8.8.8
1.1.1.1
192.168.1.0/30
```

## Output Format

Each exported row includes batch-level metadata so a single scan can be grouped reliably in downstream tools.

| IP Address | Status | Latency | Hostname | Batch Timestamp | Scan Duration (s) |
| --- | --- | --- | --- | --- | --- |
| 8.8.8.8 | Active | 12.5 | dns.google | 2026-04-09 11:42:13 | 6.24 |

Possible status values include `Active`, `Inactive`, `Timeout`, `Unreachable`, `Invalid IP`, and `Error`.

## Troubleshooting

!!! failure "Command not found: ipmg"
    Install IPMG with `pip install ipmg`, or use `pip install -e .` inside a cloned development checkout.

!!! warning "Permission denied output folder"
    Run IPMG inside a writable directory, choose a writable output path, or use elevated permissions only when your organization allows it.

!!! info "Hostname unresolvable"
    The target may not have a DNS PTR record. The scan can still complete without hostname data.

!!! bug "One host crashes the scan"
    IPMG records unexpected per-host failures as `Error` and continues scanning the remaining targets.

## macOS GUI Beta

A native macOS interface for IPMG is under active development.

[Download the PingMonitorApp beta](https://github.com/sameeralam3127/IP_Management/releases/tag/macOS)

## License

MIT License. Free for commercial and personal use.

Made with Python and uv.
