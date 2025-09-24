\# Workflow Documentation



\## CI Pipeline (`.github/workflows/ci.yml`)

\*\*Purpose:\*\* Quality gate for every change. Ensures code style, tests (with coverage ≥80%), build, and daily security checks.



\*\*Triggers\*\*

\- `push`: branches `main`, `develop`

\- `pull\_request`: into `main`

\- `schedule`: daily (`cron`) for dependency/security audit

\- `workflow\_dispatch`: manual run



\*\*Jobs \& Dependencies\*\*

\- `lint` → `test` → `build` (sequential)

\- `audit` runs independently (on schedule and on push)



\*\*Job Details\*\*

\- \*\*Lint \& Prettier:\*\* runs ESLint and Prettier checks.

\- \*\*Testing + Coverage:\*\* runs Jest in CI mode, uploads a `coverage/` artifact. Coverage threshold enforced via `package.json`.

\- \*\*Build:\*\* builds the project (outputs to `dist/`).

\- \*\*Daily Dependency \& Security Audit:\*\* runs `npm audit --audit-level=high`, uploads `audit.txt`, and creates/updates a GitHub Issue if vulnerabilities are found.



\*\*Secrets / Permissions\*\*

\- No secrets required by default.

\- Workflow sets permissions: `contents: read`, `issues: write` (for audit Issue creation).

\- Optional (not required here): `VITE\_API\_BASE\_URL` for builds, `CODECOV\_TOKEN` if using Codecov.



\*\*Troubleshooting\*\*

\- Lint errors → `npx eslint . --fix` and `npx prettier --write .`

\- Coverage fail → add tests or review thresholds in `package.json`.

\- Audit findings → download `audit.txt` artifact or read the created Issue.

\- Windows/CRLF issues → we set Prettier `"endOfLine": "auto"`.



---



\## GitHub Pages Deploy (`.github/workflows/pages.yml`)

\*\*Purpose:\*\* Publishes `dist/` to GitHub Pages.



\*\*Triggers:\*\* `push` to `main`, manual `workflow\_dispatch`.



\*\*Flow:\*\*

1\. `build` job

&nbsp;  - Checkout, Node 20, `npm ci`

&nbsp;  - Build (or placeholder page)

&nbsp;  - `actions/configure-pages@v5`

&nbsp;  - Upload artifact with `actions/upload-pages-artifact@v3`

2\. `deploy` job

&nbsp;  - `actions/deploy-pages@v4` to publish

&nbsp;  - Environment `github-pages` exposes the Page URL



\*\*One-time Setup:\*\* Repo \*\*Settings → Pages → Build and deployment → GitHub Actions\*\*.



\*\*Troubleshooting:\*\* Ensure `configure-pages` step exists, correct permissions (`pages: write`, `id-token: write`), and artifact path is `dist/`.



---



\## Reusable Composite Action (`.github/actions/setup-node-env/action.yml`)

\*\*Purpose:\*\* DRY setup for Node + dependency install.



\*\*What it does:\*\* Configures Node 20 with npm cache and runs `npm ci`.



\*\*Usage Example:\*\*

```yaml

steps:

&nbsp; - uses: actions/checkout@v4

&nbsp; - uses: ./.github/actions/setup-node-env

&nbsp; - run: npm run lint



