# Contributing

Thank you for your interest in contributing to API Aberta! 🇵🇹

## Ways to contribute

### 🔌 Build a connector

The most impactful way to help. Each connector brings new data to the platform.

1. Check if the source already has a connector in [data-sources.md](docs/data-sources.md)
2. Open an issue with the `new-connector` label to discuss it
3. Fork the `connectors` repo
4. Implement following the [connector guide](docs/connectors.md)
5. Open a Pull Request

### 🐛 Report a bug

1. Check if the bug has already been reported in [issues](https://github.com/apiaberta/apiaberta/issues)
2. Use the "Bug Report" template
3. Include steps to reproduce, expected vs actual behaviour

### 💡 Suggest a feature

1. Open an issue with the `enhancement` label
2. Describe the use case and the value it brings

### 📝 Improve documentation

Errors, ambiguities or gaps in documentation are bugs. Docs PRs are always welcome.

---

## Development setup

```bash
# Clone
git clone https://github.com/apiaberta/api.git
cd api

# Install dependencies
npm install

# Environment variables
cp .env.example .env
# Edit .env with your settings

# Run locally
npm run dev
```

Requirements: Node.js 22+, MongoDB 7+

---

## Conventions

- **Commits:** English, imperative mood (`Add endpoint X`, `Fix bug Y`)
- **Branches:** `feat/name`, `fix/name`, `docs/name`
- **PRs:** clear description of what changes and why
- **Code:** ESLint + Prettier (config in repo)

---

## Code of Conduct

This project follows the [Code of Conduct](CODE_OF_CONDUCT.md). Respect and collaboration above all.
