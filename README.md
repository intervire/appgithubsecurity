# appgithubsecurity

Security baseline for this repository:

- `SECURITY.md`: how to report vulnerabilities privately, response times, disclosure and safe harbour
- `LICENSE`: MIT
- `.github/dependabot.yml`: weekly updates for pinned GitHub Actions
- `.github/CODEOWNERS`: requires owner review for every change, including `.github/`
- `.github/rulesets/main.json`: branch ruleset for `main`, to import in the GitHub UI (see below)
- `.github/workflows/codeql.yml`: CodeQL code scanning on pushes, pull requests, and a weekly schedule
- `.github/workflows/dependency-review.yml`: blocks pull requests that add dependencies with known moderate-or-worse vulnerabilities
- `.github/workflows/secret-scan.yml`: TruffleHog secret scanning on pushes, pull requests, and a weekly full-history scan
- `.github/workflows/zizmor.yml`: zizmor audit of the workflows themselves (risky triggers, script injection, excess permissions)
- `.github/workflows/scorecard.yml`: weekly OpenSSF Scorecard security score

Every workflow pins actions to commit SHAs, uses a least-privilege token,
and runs [Harden-Runner](https://github.com/step-security/harden-runner) in
`audit` mode to record outbound network calls. Results from CodeQL, zizmor and
Scorecard appear under **Security → Code scanning**.

## Repository settings

These are set in the GitHub UI, not in files:

- **Settings → General → Default branch**: set to `main`. The workflows and
  ruleset assume `main` is the default branch.
- **Settings → Code security**: enable the dependency graph, Dependabot alerts,
  secret scanning, push protection, and private vulnerability reporting.
- **Settings → Rules → Rulesets → New ruleset → Import a ruleset**: import
  `.github/rulesets/main.json`. It blocks force-pushes and deletion, and requires
  signed commits, a reviewed pull request, resolved review threads, and passing
  CodeQL, Dependency Review, TruffleHog and zizmor checks.
- **Settings → Actions → General**: allow only actions pinned to a full commit
  SHA, and set the default workflow permissions to read-only.

### Single-maintainer bypass

The ruleset lets repository admins merge their own pull requests without a
second approval, because there is only one maintainer today. Changes still
have to go through a pull request with passing checks. Once a second reviewer
is added to `CODEOWNERS`, remove the bypass from the ruleset.

## Adding code

When you add application code, in the same pull request:

- add its language to the CodeQL `language` matrix in `codeql.yml`;
- add its package ecosystem (for example `npm` or `pip`) to `dependabot.yml`.

After a few weeks of Harden-Runner audit logs, consider switching
`egress-policy` to `block` with an allow-list of the endpoints you saw.
