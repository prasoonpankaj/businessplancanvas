# Business Plan Canvas Constitution

## Core Principles

The two driving characteristics of this project are **Maintainability** and **Performance**.
When they conflict, choose maintainability unless a stated performance budget
(Principle IV) is at risk; document the tradeoff in the plan.

### I. Simplicity Over Cleverness

- Code MUST favor the simplest design that meets the current requirement. Speculative
  abstractions, configuration, and extension points for unrequested features are prohibited.
- Names MUST convey intent; a comment MUST explain *why*, never restate *what* the code does.
- Each function or component SHOULD have one responsibility. A unit that needs a paragraph to
  describe is a candidate for splitting.
- New dependencies MUST be justified in the plan (what it replaces, its size, its maintenance
  health). Prefer the platform and existing dependencies.

Rationale: code is read far more often than written; every unneeded abstraction is a permanent
maintenance cost.

### II. Clear Module Boundaries

- The codebase MUST be organized by feature/domain, with each module exposing a small,
  explicit public interface. Other modules MUST NOT reach into a module's internals.
- Dependencies between modules MUST flow in one direction; circular dependencies are prohibited.
- Domain logic (canvas model, validation, calculations) MUST be independent of UI and
  persistence so it can be tested and changed in isolation.

Rationale: well-bounded modules limit the blast radius of change and keep the system
understandable as it grows.

### III. Test-Backed Change

- Every behavior change MUST ship with automated tests that would fail if the behavior
  regressed. Tests MUST assert intent (business rules, user-visible outcomes), not
  implementation details.
- Domain logic MUST have unit tests; each user story MUST have at least one test covering its
  acceptance scenario end to end.
- Bug fixes MUST include a test that reproduces the bug before the fix.
- The test suite MUST stay fast enough to run on every commit; slow tests are a defect to fix.

Rationale: tests are what make refactoring safe, and safe refactoring is what keeps code
maintainable.

### IV. Explicit Performance Budgets

- Every feature plan MUST state its performance budget, either by adopting the defaults in
  *Performance Standards* or by declaring stricter/looser targets with justification.
- Budgets MUST be verified by an automated check (benchmark, Lighthouse/CI budget, load test)
  before a feature is considered done.
- A change that breaches a budget MUST NOT merge without an explicit, documented exception
  approved in review.

Rationale: performance that is not specified and measured silently degrades.

### V. Measure Before Optimizing

- Optimizations MUST be motivated by a measurement (profile, trace, benchmark) showing a real or
  projected budget breach, and MUST be re-measured after the change.
- Optimizations that reduce readability MUST be isolated behind a clear interface and carry a
  comment citing the measurement that justified them.
- Algorithmic complexity MUST be considered for any operation over user data that can grow
  (e.g., canvas items, plan sections); O(n²) or worse on such data requires justification.

Rationale: unmeasured optimization trades maintainability for imagined gains; measured
optimization keeps both characteristics in balance.

## Performance Standards

Default budgets, applied unless a feature plan overrides them with justification. These assume
a web application and MUST be revisited once the tech stack is fixed.

- **Initial load**: Largest Contentful Paint ≤ 2.5 s on a mid-tier device over a fast 4G profile.
- **Interaction**: user input to visual response ≤ 100 ms (Interaction to Next Paint ≤ 200 ms).
- **Server/API**: p95 latency ≤ 200 ms for read operations, ≤ 500 ms for writes, under expected
  load.
- **Payload**: initial JavaScript bundle ≤ 200 KB compressed; growth > 10% in a single change
  MUST be called out in review.
- **Data growth**: features MUST remain within budget for a canvas/plan 10× the expected typical
  size.

## Development Workflow & Quality Gates

- All changes go through a pull request with at least one reviewer other than the author.
- Before merge, CI MUST pass: build, lint/format, full test suite, and performance budget checks.
- Reviewers MUST check each change against Principles I–V; violations require either a fix or a
  documented entry in the plan's *Complexity Tracking* table.
- Changes MUST be surgical: no unrelated refactors or formatting churn in a feature PR.
- Existing codebase conventions take precedence over personal preference; a harmful convention
  is raised as a separate proposal, not silently diverged from.

## Governance

- This constitution supersedes other project practices. Where guidance conflicts, the
  constitution wins.
- Amendments are made via pull request that updates this file, states the version bump and its
  rationale, and describes any migration needed for in-flight work.
- Versioning follows semantic versioning: MAJOR for removing or redefining a principle, MINOR
  for adding a principle or materially expanding guidance, PATCH for clarifications.
- Compliance is reviewed at every `/speckit-plan` Constitution Check and in every PR review.
  Unjustified complexity or unbudgeted performance regressions are grounds to block a merge.

**Version**: 1.0.0 | **Ratified**: 2026-11-08 | **Last Amended**: 2026-11-08
