# appgithubsecurity

Security baseline for this repository:

- `SECURITY.md`: how to report vulnerabilities privately
- `.github/dependabot.yml`: weekly updates for pinned GitHub Actions
- `.github/CODEOWNERS`: requires owner review for every change, including `.github/`
- `.github/workflows/dependency-review.yml`: blocks pull requests that add dependencies with known moderate-or-worse vulnerabilities
- `.github/workflows/scorecard.yml`: weekly OpenSSF Scorecard security score, reported under Security → Code scanning
- `.github/workflows/codeql.yml`: CodeQL code scanning on pushes, pull requests, and a weekly schedule. Actions are pinned to commit SHAs and the token is least-privilege.

## Recommended repository settings

These are set in the GitHub UI, not in files:

- **Settings → Code security**: enable Dependabot alerts, secret scanning, push protection, and private vulnerability reporting
- **Settings → Branches**: protect `main` by requiring pull requests, requiring the `CodeQL` check, and blocking force-pushes
