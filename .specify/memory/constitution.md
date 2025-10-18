<!-- 
=============================================================================
SYNC IMPACT REPORT
=============================================================================
Version Change: 0.1.0 (template) → 1.0.0 (initial)
Rationale: Established foundational governance with four core principles
          addressing code quality, testing standards, UX consistency, and
          performance requirements.

Principles Established:
- Code Quality (Principle I)
- Testing Standards (Principle II)  
- User Experience Consistency (Principle III)
- Performance Requirements (Principle IV)

Added Sections:
- Quality Assurance Standards
- Development Workflow & Review Process

Templates Updated:
- ✅ .specify/templates/plan-template.md (Constitution Check now references all 4 principles)
- ✅ .specify/templates/spec-template.md (added UX-001 to UX-007, PERF-001 to PERF-004)
- ✅ .specify/templates/tasks-template.md (integrated Quality, Testing, UX review tasks in each Phase)

Follow-up TODOs: None - all placeholders resolved.
=============================================================================
-->

# Duotify Membership Constitution

## Core Principles

### I. Code Quality
All specifications, plans, and user-facing documentation MUST be written in Traditional Chinese (zh-TW)

Every line of code must be maintainable, understandable, and aligned with
industry best practices. Code quality is non-negotiable and directly impacts
the project's long-term sustainability and team velocity.

**Non-Negotiable Rules:**
- All code MUST adhere to established linting and formatting standards
- Code reviews MUST verify readability, architecture, and maintainability
- No complex logic without clear documentation and comments
- Functions and modules MUST have a single, well-defined responsibility
- Dead code and unused dependencies MUST be removed immediately
- Naming conventions MUST be consistent and self-documenting

**Rationale**: High-quality code reduces bugs, accelerates onboarding,
enables safer refactoring, and reduces technical debt accumulation.

---

### II. Testing Standards

Testing is a primary engineering activity, not an afterthought. Test-driven
development (TDD) ensures correctness, maintainability, and confidence in
deployments.

**Non-Negotiable Rules:**
- Tests MUST be written before implementation (TDD mandatory)
- Test coverage MUST be ≥ 80% for critical paths
- All public APIs MUST have contract tests
- Integration tests MUST verify inter-component communication
- Unit tests MUST be fast (<100ms per test)
- Tests MUST be independent and reproducible (no shared state)
- Failing tests MUST block merges to main branch

**Rationale**: Comprehensive testing prevents regressions, documents
expected behavior, enables confident refactoring, and reduces production
incidents.

---

### III. User Experience Consistency

Every user-facing feature MUST deliver a consistent, predictable, and
delightful experience. Consistency builds trust and reduces cognitive load.

**Non-Negotiable Rules:**
- All UI components MUST follow established design system
- User workflows MUST align with existing interaction patterns
- Error messages MUST be clear, actionable, and user-friendly
- Loading states and feedback MUST be visible and informative
- Accessibility standards MUST be met (WCAG 2.1 AA minimum)
- API responses MUST return consistent data formats and error structures
- Localization/internationalization MUST be considered in design

**Rationale**: Consistency improves user satisfaction, reduces support
burden, and increases adoption and retention.

---

### IV. Performance Requirements

The application MUST perform efficiently under expected load to deliver
responsive user experiences and minimize infrastructure costs.

**Non-Negotiable Rules:**
- API response times MUST be ≤ 200ms for p95 under normal load
- UI interactions MUST be ≤ 100ms to feel instant
- Database queries MUST be optimized with proper indexing
- Bundle sizes MUST not exceed 500KB (gzipped) for frontend assets
- Memory usage MUST be monitored and profiled regularly
- Performance regressions MUST be caught in automated testing
- Load testing MUST be performed before major releases

**Rationale**: Performance directly impacts user experience, conversion
rates, and operational costs. Early detection prevents costly production
incidents.

---

## Quality Assurance Standards

All features MUST pass through defined quality gates before deployment:

1. **Code Review Gate**: Peer review verifying code quality principles
2. **Test Gate**: Automated tests pass (unit, integration, contract)
3. **Performance Gate**: Performance benchmarks met or explained
4. **UX Consistency Gate**: Feature reviewed against design system
5. **Accessibility Gate**: WCAG 2.1 AA compliance verified

---

## Development Workflow & Review Process

**Pre-Commit**:
- All code MUST pass linting and formatting checks
- All tests MUST pass locally before committing

**Pull Request**:
- At least one peer review MUST approve before merge
- All automated checks (linting, tests, performance) MUST pass
- Constitution compliance MUST be verified by reviewers
- Commit messages MUST clearly describe changes and link to tracking

**Merge to Main**:
- Feature branch MUST be up-to-date with main
- All review comments MUST be addressed or closed
- Deployment readiness verification

---

## Governance

**Constitution Authority**: This constitution supersedes all informal
practices and conventions. When conflicts arise, the constitution provides
the definitive standard.

**Amendment Procedure**:
1. Proposed changes MUST be documented with clear rationale
2. Amendments MUST be reviewed by team leads for alignment
3. Rationale and migration plan MUST be provided for breaking changes
4. Version number MUST follow semantic versioning (MAJOR.MINOR.PATCH)

**Compliance Review**: Constitution compliance MUST be verified in all
code reviews. Exceptions MUST be explicitly documented with business
justification.

**Runtime Guidance**: Development guidance, examples, and tool
configuration are documented in `.github/prompts/` and project README.
These are living documents that reflect the constitution's intent.

---

**Version**: 1.0.0 | **Ratified**: 2025-10-18 | **Last Amended**: 2025-10-18

```
