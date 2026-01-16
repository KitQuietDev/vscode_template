# Engineering Constitution v1.1

## 0. Purpose

This document defines non-negotiable engineering principles intended to prevent:
- Fragility
- Hidden behavior
- Security rot
- Debugging dead ends
- AI-generated nonsense that “works” but cannot be trusted

This is a **constraint system**, not a best-practices manifesto.

If a design choice conflicts with this constitution, the constitution wins.

---

## 1. Structure and Modularity

### 1.1 Module

A **module** is the least complex unit capable of performing meaningful work or transformation.

A module must:
- Accept input
- Perform work or transform that input
- Produce stable, known output

Examples of meaningful work or transformation:
- Validation (untrusted → trusted)
- Computation (data → result)
- Formatting (structure A → structure B)
- Parsing or decoding
- State transitions

A module must **not** exist solely to:
- Pass messages unchanged
- Act as plumbing between other modules
- Hide complexity without owning behavior

### 1.2 Infrastructure vs Modules

**Infrastructure components** (routers, event buses, schedulers, loaders):
- Coordinate execution
- Route data
- Manage lifecycle

Infrastructure **must not**:
- Perform validation
- Perform business logic
- Transform domain data

Modules do work. Infrastructure coordinates.

---

## 2. Trust Boundaries and Validation

### 2.1 Trust Boundaries

All data is considered **untrusted** when it crosses any of the following:
- Process boundaries
- Network boundaries
- File or device I/O
- External system interfaces
- Serialization or deserialization (JSON, YAML, binary, etc.)

Trust is **lost** at these boundaries and must be re-established.

### 2.2 Validation Model

**Validate at boundaries. Assert internally.**

- **External boundaries** require full validation:
  - Type
  - Structure
  - Range
  - Format
  - Required fields
- **Internal module boundaries** may use lightweight assertions if:
  - The data originates from a validated source
  - The data has not crossed a trust boundary
  - The modules are within the same trust domain

When in doubt, validate.

Invalid input must never be ingested.

---

## 3. Known-Safe State and Failure Behavior

### 3.1 Known-Safe State

A system must define a **known-safe state**.

A known-safe state means:
- No secrets are leaked
- No unintended network access occurs
- No partial or corrupt state persists
- The system can be restarted or exited without side effects

### 3.2 Failure

Failures must:
- Fail fast
- Fail loudly
- Fail safely

Silent failure is forbidden.

---

## 4. Errors and Logging

Errors must be **human-readable**.

Every error must include:
1. What failed
2. Why it failed
3. What input or condition caused it
4. Suggested remediation (when applicable)

Stack traces are supplemental, not sufficient.

Logging must:
- Be readable by humans
- Be explicit about state changes
- Never expose secrets
- Never be the only place correctness is enforced

---

## 5. Side Effects and System Interaction

### 5.1 Network

- No component may phone home.
- All network activity must be:
  - Documented
  - Explicit
  - Disable-able

Background network activity without user awareness is forbidden.

### 5.2 Filesystem and OS

- Writes must be confined to declared directories
- Service spawning is allowed only if:
  - Documented
  - Traceable
  - Required for core function

Modifying system state must be intentional and reversible.

---

## 6. Warnings

Warnings are:
- Non-blocking
- Lightweight
- Advisory

Warnings must:
- Be logged
- Indicate potential issues

Warnings must never:
- Prevent valid input from processing
- Escalate into failure conditions
- Be used as soft validation

---

## 7. Testing

Every module that accepts **external input** must have tests covering:
1. Valid input (happy path)
2. Invalid or malformed input (must reject clearly)
3. Edge cases (empty, null, zero, boundary conditions)
4. Hostile input (oversized, adversarial, malicious) when applicable

Modules that only accept validated internal input may omit (2) and (4) **only if**:
- Upstream validation is tested
- Internal assertions exist

Allowing invalid input to pass tests is a critical defect.

---

## 8. Concurrency and Async

When async or concurrent behavior is introduced:

- State mutations must be explicit and traceable
- Shared state must be protected (locks, queues, immutability)
- Failures in async paths must propagate or log clearly
- Race conditions must be prevented or documented

Operations that may be retried must be idempotent.

If concurrency makes correctness unverifiable, the design must be justified.

---

## 9. Dependencies

Dependencies must be:
- Explicit
- Recognizable
- Isolated (virtualenv, container, module boundary)

“Reasonably mature” means one or more of:
- Stable releases
- Active maintenance
- Widespread production use

Hidden, vendored, or undeclared dependencies are forbidden.

---

## 10. Configuration

Configuration defines:
- Interfaces
- Constraints
- Limits
- Integration points

Configuration must not:
- Contain hidden logic
- Replace code-level validation

Derived configuration values are allowed if:
- Deterministic
- Documented

---

## 11. Hot Reloading and Dynamic Behavior

Hot reloading is permitted only if safe.

If a reload fails, the system must:
- Roll back to the last known-safe state, **or**
- Halt explicitly

Partial reloads that leave the system in an unknown state are forbidden.

---

## 12. Enforcement

This constitution is not advisory.

Code that violates these principles must be rewritten, not justified.

“It works” is not a defense.
