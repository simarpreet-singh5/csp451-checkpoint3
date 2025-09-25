\# 🛠️ Workflow Documentation – CSP451 Checkpoint 3



\## 📌 CI Pipeline (`.github/workflows/ci.yml`)



\*\*Purpose:\*\*  

This workflow automatically runs on every push or pull request to `main` or `develop`.  

It ensures the code is clean, tested, and build-ready before merging.



\*\*Triggers:\*\*  

\- `on: push` (branches: main, develop)  

\- `on: pull\_request` (branch: main)



\*\*Jobs \& Order:\*\*



1\. \*\*Lint \& Prettier\*\* – Checks code style with ESLint and Prettier.

2\. \*\*Testing + Coverage\*\* – Runs Jest unit tests and generates a coverage report.

3\. \*\*Build\*\* – Builds the application after successful linting and tests.

4\. \*\*Audit\*\* – (Optional) Checks for vulnerabilities with `npm audit`.



\*\*Artifacts Produced:\*\*  

\- Test coverage report (`coverage/`)  

\- Audit report (if vulnerabilities are found)



\*\*Secrets / Permissions:\*\*  

\- None required for CI.



\*\*Common Issues \& Fixes:\*\*  

\- `exit code 1` during lint: Run `npm run lint` and fix errors.  

\- `exit code 127` in tests: Ensure Jest is installed and tests are valid.  

\- Coverage < 80%: Add or improve unit tests.



---



\## 📤 Deployment Workflow (`.github/workflows/pages.yml`)



\*\*Purpose:\*\*  

Automatically builds and deploys the project to GitHub Pages.



\*\*Triggers:\*\*  

\- `on: push` (branch: `main`)  

\- `on: workflow\_dispatch` (manual run)



\*\*Jobs:\*\*



1\. \*\*Build\*\* – Installs dependencies, runs the build command, and prepares the `dist/` folder.

2\. \*\*Deploy\*\* – Publishes the content of `dist/` to GitHub Pages.



\*\*Permissions Needed:\*\*  

```yaml

permissions:

&nbsp; contents: read

&nbsp; pages: write

&nbsp; id-token: write



