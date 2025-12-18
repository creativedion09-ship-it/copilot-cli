# AI Agent Instructions for copilot-cli Repository

## Project Overview
This is the **distribution and public-facing repository** for GitHub Copilot CLI, a terminal-native AI coding assistant. The repository primarily contains:
- Installation scripts and distribution manifests
- GitHub automation workflows for releases and issue triage
- Official documentation and changelog
- **Source code is not here** — this is a release/installation channel only

## Key Context for Development

### Not a Typical Source Repository
- **No source code** in this repo — it's a distribution channel
- **Distribution formats** managed: npm packages, Homebrew, WinGet, direct binary releases
- **Target audience**: End users downloading/installing Copilot CLI
- **Contributing**: Bug reports, installation issues, documentation, not code patches

### Installation Mechanisms
1. **Homebrew** (macOS/Linux): `brew install copilot-cli`
2. **npm** (cross-platform): `npm install -g @github/copilot`
3. **WinGet** (Windows): `winget install GitHub.Copilot`
4. **Direct bash script** (macOS/Linux): `curl -fsSL https://gh.io/copilot-install | bash`
5. **Prerelease versions** available alongside stable versions

**Key file**: [install.sh](install.sh) — handles multi-platform binary distribution with version pinning via SHA256 checksums

### Release Workflow
- Releases trigger WinGet submission automatically ([.github/workflows/winget.yml](.github/workflows/winget.yml))
- Binary assets attached: `copilot-{platform}-{arch}.tar.gz` format
- Versioning: Semantic versioning with `v` prefix (e.g., `v0.0.369`)
- Prerelease vs stable package IDs handled separately for WinGet

### Issue Management Automation
Multiple GitHub Actions workflows enforce quality:
- Auto-close single-word issues (likely spam)
- Triage new issues with labels
- Close stale issues after 30 days inactivity
- Remove triage label when maintainers engage
- Generate templated responses for non-reproducible/invalid issues

## Architecture & Dependencies

### Core Features (from README)
- **Terminal-native**: Direct command-line access without GUI switching
- **GitHub integration**: Native repo, issue, PR access with OAuth authentication
- **Agentic capabilities**: Multi-step planning, code editing, debugging, refactoring
- **MCP extensibility**: Model Context Protocol servers for custom tool integration
- **Model selection**: Claude Sonnet 4.5 (default), Claude Sonnet 4, GPT-5.x variants available

### Authentication
- **GitHub OAuth**: Primary (via `/login` command)
- **Personal Access Token (PAT)**: Alternative with "Copilot Requests" permission
- **Environment variables**: `GH_TOKEN` or `GITHUB_TOKEN` for auth via ENV

### Platform Support
- **Linux** (x64, arm64)
- **macOS** (x64, arm64)  
- **Windows** (via WinGet or npm; requires PowerShell v6+)

### Requirements
- Node.js v22+
- npm v10+
- Active Copilot subscription

## Working Patterns & Conventions

### When Making Changes

**Documentation**:
- Update [README.md](README.md) for installation/usage changes
- Keep [changelog.md](changelog.md) as running log of version changes
- Include feature/fix descriptions, date, version number

**Issue Templates**:
- Located in [.github/ISSUE_TEMPLATE/](.github/ISSUE_TEMPLATE/) 
- Use to guide users toward reproducible bug reports
- Auto-tagged by workflow (needs triage, needs info, etc.)

**Release Publishing**:
- WinGet integration auto-triggers on GitHub release publication
- Ensure binary assets are named correctly: `copilot-{darwin|linux|win32}-{x64|arm64}`
- SHA256 checksums required for install script integrity verification

### Common Issues Pattern
Repository focuses on:
- Installation problems (multi-platform)
- Authentication/token issues
- Compatibility reports
- Feature requests for CLI behavior
- **Not**: Source code bugs (route to GitHub internal teams)

## Quick Reference

| Task | File/Command |
|------|------|
| Change installation instructions | [README.md](README.md) line ~40-70 |
| Document version changes | [changelog.md](changelog.md) |
| Update WinGet release logic | [.github/workflows/winget.yml](.github/workflows/winget.yml) |
| Modify issue automation | [.github/workflows/](workflows/) |
| Check install script logic | [install.sh](install.sh) |

## Related Resources
- [Official Copilot CLI Docs](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)
- [Premium Requests Info](https://docs.github.com/copilot/managing-copilot/monitoring-usage-and-entitlements/about-premium-requests)
- [Model availability](README.md) — includes all supported models and current defaults
