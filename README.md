# {{PROJECT_NAME}}

> A production-grade React Native application built on top of
> [react-native-boilerplate-rules](https://github.com/dennerparreiras/react-native-boilerplate-rules) —
> an opinionated, security-first engineering rulebook for modern React Native projects.

This repository ships with a complete, battle-tested engineering policy designed to be understood
**both by humans and by LLM coding agents**. Every rule is written so that an AI assistant working
on this codebase produces the same quality of code a disciplined senior engineer would.

---

## What this rulebook gives you

| Area | What is covered |
|---|---|
| **Development** | Core principles, cross-platform parity, state management, business-rule isolation, TypeScript strictness, JSDoc, accessibility, i18n, performance, file structure, navigation |
| **Design System** | Component-first UI development, token discipline, package conventions, Storybook coverage, quality gates |
| **Testing** | Jest layers (unit / store / integration / smoke), business-rule isolation, mocking standards, regression policy |
| **Security** | Data protection at rest, network security (TLS / pinning), authentication and session handling, deep-link and navigation hardening, platform hardening, supply-chain and secrets management |
| **Workflow** | Zero-tolerance quality gates, git and PR discipline, documentation-as-code |

---

## Getting started with a new project

### 1. Apply the boilerplate

Clone or copy this repository into your new project.

The `rules/` folder stays at the repository root. It is the source of truth for engineering policy
and the primary context source for AI coding agents.

### 2. Replace the project-name tag

Every file in this repository refers to your application through the placeholder tag
`{{PROJECT_NAME}}`. Replace it in a single pass with your real project name:

```bash
# macOS / Linux — replace MyAwesomeApp with your project name
grep -rl '{{PROJECT_NAME}}' . --exclude-dir=node_modules --exclude-dir=.git \
  | xargs sed -i '' 's/{{PROJECT_NAME}}/MyAwesomeApp/g'   # macOS (BSD sed)

grep -rl '{{PROJECT_NAME}}' . --exclude-dir=node_modules --exclude-dir=.git \
  | xargs sed -i 's/{{PROJECT_NAME}}/MyAwesomeApp/g'      # Linux (GNU sed)
```

Additional placeholder tags used across the documentation:

| Tag | Meaning | Example replacement |
|---|---|---|
| `{{PROJECT_NAME}}` | Human-readable application name | `MyAwesomeApp` |
| `{{APP_ID}}` | Bundle identifier / application ID | `com.company.myawesomeapp` |
| `{{REPO_URL}}` | Git repository URL of the applied project | `https://github.com/org/my-awesome-app` |

Verify no tag was left behind:

```bash
grep -rn '{{PROJECT_NAME}}\|{{APP_ID}}\|{{REPO_URL}}' . \
  --exclude-dir=node_modules --exclude-dir=.git
```

### 3. Wire the rules into your AI tooling

The `.mdc` files under `rules/` follow the [Cursor rules](https://docs.cursor.com/context/rules)
format (YAML front matter with `description`, `globs`, and `alwaysApply`). To activate them:

- **Cursor**: copy or symlink the `rules/` subfolders into `.cursor/rules/`.
- **Other agents (Claude Code, Codex, etc.)**: point the agent's instruction file
  (`CLAUDE.md`, `AGENTS.md`, …) at `rules/00-rules-index.mdc` as the entry point.

The rule chain is deliberately layered: the index file explains precedence, and each category file
is self-contained so agents can load only what is relevant to the files being edited.

---

## Rulebook structure

```
rules/
├── 00-rules-index.mdc              # Entry point: index, precedence, how the chain works
├── development/
│   ├── 01-core-principles.mdc      # Non-negotiable engineering principles
│   ├── 02-cross-platform.mdc       # iOS / Android / Web parity rules
│   ├── 03-architecture-state.mdc   # State management and business-rule isolation
│   ├── 04-typescript-documentation.mdc  # Strict TS + mandatory JSDoc
│   ├── 05-accessibility.mdc        # A11y requirements for every interactive surface
│   ├── 06-internationalization.mdc # i18n rules for user-visible copy
│   ├── 07-performance.mdc          # Complexity analysis and render-cost discipline
│   ├── 08-file-structure.mdc       # Folder layout, styled isolation, no monoliths
│   ├── 09-navigation.mdc           # Typed navigation and route contracts
│   └── 10-authoring-checklist.mdc  # Pre-review checklist for every feature
├── design-system/
│   ├── 01-foundation.mdc           # Design-system-first UI contract
│   ├── 02-package-conventions.mdc  # Tokens, layout ownership, lists, Storybook
│   └── 03-quality-gates.mdc        # Component quality bar and validation gates
├── testing/
│   ├── 01-tooling.mdc              # Jest setup, renderer policy, commands
│   ├── 02-test-layers.mdc          # Unit / store / integration / smoke layers
│   ├── 03-business-rule-isolation.mdc  # Where rules live and how they are tested
│   └── 04-practices.mdc            # AAA shape, mocks, naming, regression policy
├── security/
│   ├── 01-security-principles.mdc  # Threat model and security-by-default posture
│   ├── 02-data-protection.mdc      # Secure storage, encryption, PII handling
│   ├── 03-network-security.mdc     # TLS, certificate pinning, API hygiene
│   ├── 04-authentication.mdc       # Auth flows, tokens, biometrics, sessions
│   ├── 05-navigation-deeplinks.mdc # Deep-link validation and screen protection
│   ├── 06-platform-hardening.mdc   # OS-level hardening, root/jailbreak, obfuscation
│   └── 07-supply-chain-secrets.mdc # Dependency auditing and secrets management
└── workflow/
    ├── 01-quality-gates.mdc        # Zero-tolerance gates: lint, typecheck, tests
    ├── 02-git-and-pr.mdc           # Commit hygiene and PR discipline
    └── 03-documentation.mdc        # Documentation-as-code requirements
```

---

## Core philosophy

1. **Zero tolerance for red code.** Lint, typecheck, and the full Jest suite must be green before
   any change is considered complete. There is no "fix it in the next commit."
2. **Security by default.** Every feature is designed with data protection, transport security,
   and abuse resistance in mind — not patched afterwards.
3. **Design System first.** UI is composed from documented, tested, token-driven primitives.
   Ad-hoc view compositions do not enter product screens.
4. **Business rules are pure and tested.** Any decision expressible without React lives in a pure
   module with a colocated test.
5. **Readable by humans and machines.** Every rule states *what*, *why*, and *how to verify* —
   so that LLM agents and new team members converge on the same behavior.

---

## Runtime baseline

These rules target the modern React Native stack:

- **React Native 0.80+** with the **New Architecture** (Fabric + TurboModules) enabled
- **React 19+**
- **Hermes** as the JavaScript engine
- **TypeScript** in `strict` mode
- **react-native-web** for the web target
- **Storybook** for component development and visual reference

---

## Mandatory quality gates

Run before considering **any** change complete:

```bash
npm run lint                       # ESLint — zero errors, zero new warnings
npx tsc --noEmit --pretty false    # TypeScript — zero errors
npm test                           # Jest — full suite green
```

See [`rules/workflow/01-quality-gates.mdc`](rules/workflow/01-quality-gates.mdc) for the full
policy, including scoped gates and error-triage rules.

---

## License

This rulebook is provided as-is for use in any project. Attribution is appreciated:
[react-native-boilerplate-rules](https://github.com/dennerparreiras/react-native-boilerplate-rules).
