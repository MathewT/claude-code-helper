# CI/CD Skill

A reference guide for setting up, maintaining, and debugging continuous integration and continuous delivery pipelines.

## Core concepts

| Term | Meaning |
|------|---------|
| **CI (Continuous Integration)** | Automatically build and test every commit/PR |
| **CD (Continuous Delivery)** | Every passing commit is deployable to production at the click of a button |
| **CD (Continuous Deployment)** | Every passing commit is automatically deployed to production |
| **Pipeline** | An ordered sequence of stages (e.g., build → test → scan → deploy) |
| **Artifact** | A versioned, immutable build output (binary, container image, package) |

---

## Pipeline design principles

1. **Fail fast.** Put the cheapest checks (lint, unit tests) first so developers get feedback quickly.
2. **Run in parallel where possible.** Independent jobs (unit tests, lint, security scan) should run concurrently.
3. **Immutable artifacts.** Build once; promote the same artifact through environments. Never build again for staging/production.
4. **Pin dependency versions.** Unpinned dependencies lead to non-reproducible builds.
5. **Keep secrets out of source.** Use environment variables, secrets managers, or vault integrations — never hardcode credentials.
6. **Keep pipelines fast.** Target < 10 minutes for feedback on a PR. Cache aggressively.

---

## Typical pipeline stages

```
Commit pushed
    │
    ├─► Lint & static analysis  (fast: ~1–2 min)
    ├─► Unit tests               (fast: ~2–5 min)
    ├─► Build / compile          (varies)
    ├─► Integration tests        (slower: ~5–15 min)
    ├─► Security scan (SAST/SCA) (can be async)
    │
    ▼ (on merge to main)
    ├─► Build container image / package artifact
    ├─► Push to registry
    ├─► Deploy to staging
    ├─► Smoke / acceptance tests
    │
    ▼ (manual approval or auto on green)
    └─► Deploy to production
```

---

## GitHub Actions patterns

### Reusable workflow
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "20" }
      - run: npm ci
      - run: npm run lint

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "20" }
      - run: npm ci
      - run: npm test -- --coverage

  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: "20" }
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/
```

### Dependency caching
```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

### Environment-based deployment
```yaml
deploy-staging:
  needs: build
  environment: staging
  runs-on: ubuntu-latest
  steps:
    - uses: actions/download-artifact@v4
      with: { name: dist }
    - run: ./scripts/deploy.sh staging
      env:
        DEPLOY_KEY: ${{ secrets.STAGING_DEPLOY_KEY }}
```

---

## Debugging failing pipelines

1. **Read the full log** — The first red line is usually the root cause; errors below it are often cascading failures.
2. **Reproduce locally** — Run the same command the CI runs in your local environment. Use Docker to replicate the CI environment if needed.
3. **Check environment differences** — CI may use a different OS, Node/Python/Go version, or missing environment variables. Compare `.tool-versions`, `nvmrc`, or `pyproject.toml` against the CI setup.
4. **Isolate the failing step** — Comment out subsequent steps temporarily to confirm the exact step that fails.
5. **Check for flakiness** — Re-run the job. If it passes, the test is flaky. Use retry logic or fix the non-determinism.
6. **Inspect artifacts** — Download build artifacts, coverage reports, or test result XML for more detail.

---

## Security best practices

- **Use OIDC instead of long-lived credentials** — Modern cloud providers and GitHub Actions support short-lived tokens via OIDC.
- **Restrict secret scope** — Secrets should be scoped to the environment (staging, production) that needs them.
- **Pin action versions to a commit SHA** — `uses: actions/checkout@v4` can change; `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683` cannot.
- **Review third-party actions** — Third-party actions run in your pipeline and have access to secrets. Audit them carefully.
- **Enable dependency review** — Use Dependabot or similar tools to catch vulnerable dependencies automatically.

---

## Performance optimisation

| Technique | Saving |
|-----------|--------|
| Dependency caching | 1–5 min per job |
| Parallel jobs | Proportional to longest sequential chain |
| Sparse checkout | Useful for monorepos |
| Matrix builds | Test multiple versions concurrently |
| Skip unchanged paths | Use `paths:` filters in GitHub Actions |
| Incremental builds | Nx, Turborepo, Bazel remote cache |
