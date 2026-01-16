# VS Code Project Template with Engineering Constitution

This repository is a **base template** for new software projects developed with AI assistance (e.g., GitHub Copilot), designed to prioritize:

- Durability over cleverness
- Explicit behavior over implicit magic
- Security and correctness by default
- Predictable, debuggable systems

It is intentionally **language- and framework-agnostic**.

---

## What This Is

This repository provides:

- A reusable **project skeleton**
- A documented **Engineering Constitution**
- Explicit **AI behavior constraints**
- Lightweight guardrails for human and AI contributors

It is meant to be used as a **starting point**, not as a framework or runtime dependency.

---

## Engineering Constitution

All projects created from this template adhere to the **Engineering Constitution**, located at:

[`docs/engineering-constitution.md`](docs/engineering-constitution.md)

The constitution defines non-negotiable principles covering:

- Modularity and structure
- Trust boundaries and validation
- Failure behavior and known-safe states
- Logging and error handling
- Side effects (network, filesystem, OS)
- Testing expectations
- Concurrency and async behavior
- Dependency and configuration hygiene

If a design or implementation choice conflicts with the constitution, **the constitution wins**.

---

## AI-Assisted Development

This repository is designed to be used with AI tools.

### GitHub Copilot

AI behavior is governed by:

[`.github/copilot-instructions.md`](.github/copilot-instructions.md)

All AI-generated or AI-modified code **must comply** with the Engineering Constitution.

The instructions explicitly constrain Copilot to avoid:

- Hidden side effects
- Implicit network behavior
- Ingesting unvalidated input
- Brittle or opaque designs

The goal is **predictable output**, not novelty.

---

## Pull Requests

A pull request template is included:

[`.github/PULL_REQUEST_TEMPLATE.md`](.github/PULL_REQUEST_TEMPLATE.md)

It exists to force an explicit compliance check against the Engineering Constitution.

Even if you rarely use pull requests, the checklist serves as:
- A review aid
- A design sanity check
- A way to catch regressions early

Unchecked boxes must be justified.

---

## What This Is Not

This repository is **not**:

- A framework
- A style guide
- A replacement for documentation
- An endorsement of any language or toolchain
- Optimized for enterprise process or velocity-at-all-costs development

It intentionally favors **boring, explicit, durable systems**.

---

## Using This Template

### Option 1: GitHub Template (Recommended)

- Click **Use this template**
- Create a new repository
- Start building

### Option 2: Local-First

- Clone or copy the repository locally
- Modify as needed
- Push to GitHub only when ready

---

## Philosophy

This template exists because:

- AI tools are powerful but undisciplined
- Implicit assumptions rot over time
- Debugging opaque systems is expensive
- “It works” is not the same as “It is safe”

The Engineering Constitution is a constraint system designed to make AI-assisted development **predictable, inspectable, and survivable**.

If you find it useful, feel free to use it, adapt it, or fork it.

---

## License

Use it however you like. Attribution appreciated, not required.
