# Contributing

Thank you for your interest in contributing to API Aberta! 🇵🇹

## Ways to contribute

### 🔌 Build a connector

The most impactful way to help. Each connector brings new data to the platform.

1. Check whether the source already has a connector in [data-sources.md](docs/data-sources.md)
2. Open an issue with the `new-source` label to discuss it (use the "New Data Source" template)
3. Fork or copy [service-template](https://github.com/apiaberta/service-template) — each connector is its own repository, named `connector-<name>`. Where yours will live in the organisation is agreed in the issue
4. Implement it following the [service guidelines](docs/service-guidelines.md)
5. Once it is live and passes the checklist, open an issue with the `new-connector` label so the gateway team can register it (see [Registering your service](docs/service-guidelines.md#10-registering-your-service))

### 🐛 Report a bug

1. Check if the bug has already been reported in [issues](https://github.com/apiaberta/apiaberta/issues)
2. Use the "Bug Report" template
3. Include steps to reproduce, expected vs actual behaviour

### 💡 Suggest a feature

1. Open an issue with the `enhancement` label (use the "Feature Request" template)
2. Describe the use case and the value it brings

### 📝 Improve documentation

Errors, ambiguities or gaps in documentation are bugs. Docs PRs are always welcome. This repository holds the organisation-wide docs; connector-specific docs live in each connector's README.

---

## Development setup

This repository contains documentation only. The code lives in separate repositories (see the [repository list](README.md#project-structure)). Clone the one you want to change, for example the gateway:

```bash
# Clone
git clone https://github.com/apiaberta/gateway.git
cd gateway

# Install dependencies
npm install

# Environment variables
cp .env.example .env
# Edit .env with your settings

# Run locally
npm run dev

# Run the tests
npm test
```

Requirements: Node.js 22+, MongoDB 7+

---

## Submitting changes

1. **Fork** the repository you want to change (unless you have write access to it) and clone your fork
2. Create a branch from `main` (`feat/name`, `fix/name` or `docs/name`) — never commit to `main`
3. Commit, then **push the branch to your fork**
4. Open a **Pull Request** against `main` of the original repository, filling in the pull request template

---

## Conventions

- **Commits:** English, imperative mood (`Add endpoint X`, `Fix bug Y`). Several repositories also use Conventional Commit prefixes (`feat:`, `fix:`, `docs:`, `chore:`) — follow the style of the repository you are working in
- **Branches:** `feat/name`, `fix/name`, `docs/name`
- **PRs:** use the pull request template; describe what changes and why, and reference the issue (`Closes #42`)
- **Code:** follow the existing style of the repository. ESLint configuration currently exists only in the two frontends; there is no shared Prettier config yet. Add or update tests for what you change and run `npm test` before opening a PR

---

## Code of Conduct

This project follows the [Code of Conduct](CODE_OF_CONDUCT.md). Respect and collaboration above all.
