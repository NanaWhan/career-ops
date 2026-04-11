# Setup Guide

## Prerequisites

- [Claude Code](https://claude.ai/code) installed and configured
- Node.js 18+ (for PDF generation and utility scripts)
- (Optional) Go 1.21+ (for the dashboard TUI)

## Quick Start (5 steps)

### 1. Clone and install

```bash
git clone https://github.com/NanaWhan/career-ops.git
cd career-ops
npm install
npx playwright install chromium   # Required for PDF generation
```

### 2. Open Claude Code — it guides the rest

```bash
claude
```

Career-ops detects whether you've set up your profile, CV, and portals. If any are missing, it automatically enters **onboarding mode** and walks you through each step:

1. **CV** — Paste your CV, share your LinkedIn URL, or describe your experience. Claude converts it to `cv.md`.
2. **Profile** — Claude asks for your target roles, salary expectations, location preferences, and deal-breakers. Saved to `config/profile.yml` and `modes/_profile.md`.
3. **Portals** — Claude configures the scanner for your target roles and market. Saved to `portals.yml`.

> **You don't need to edit YAML files manually.** Just answer Claude's questions in plain language. The more context you give it (your career story, your stack, what you want to avoid, your best achievements), the better every evaluation will be.

If you prefer to set things up manually:

```bash
cp config/profile.example.yml config/profile.yml
cp templates/portals.example.yml portals.yml
# then create cv.md with your CV in markdown format
```

### 3. Start evaluating

Once setup is done, paste any job URL directly into Claude Code. Career-ops evaluates it, generates a report, creates a tailored PDF, and tracks it — all automatically.

Or run `/career-ops scan` to pull fresh listings from your configured portals.

## Available Commands

| Action | How |
|--------|-----|
| Evaluate an offer | Paste a URL or JD text |
| Search for offers | `/career-ops scan` |
| Process pending URLs | `/career-ops pipeline` |
| Generate a PDF | `/career-ops pdf` |
| Batch evaluate | `/career-ops batch` |
| Check tracker status | `/career-ops tracker` |
| Fill application form | `/career-ops apply` |

## Verify Setup

```bash
node cv-sync-check.mjs      # Check configuration
node verify-pipeline.mjs     # Check pipeline integrity
```

## Build Dashboard (Optional)

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard --path ..  # Opens TUI pipeline viewer
```
