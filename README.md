# /deep-audit

**A four-phase code auditor for Claude Code and OpenClaw.**

Built by [Speedrun AI Labs](https://speedrunlab.ai), Sydney, Australia.

---

AI writes code fast. This skill makes sure it does not ship broken.

`/deep-audit` chains planning mode, security review, extended thinking, and execution into a single disciplined pipeline. It catches what pattern-matching tools miss: business logic flaws, race conditions, silent failures, subtle auth bypasses, and the kind of bugs that survive years of human review.

Every finding comes with a file, a line number, and a fix.

## How it works

| Phase | Mode | What it does |
|-------|------|-------------|
| **1. Planning** | Wide exploration | Maps the repo, identifies architecture, ranks high-risk surfaces |
| **2. Security Review** | Structured scan | OWASP Top 10, injection attacks, auth flaws, data exposure, crypto issues, dependency CVEs |
| **3. Deep Analysis** | Extended thinking | Business logic flaws, race conditions, error handling gaps, dead code, config hygiene |
| **4. Execution** | Surgical fixes | Applies or proposes fixes for critical/high findings depending on host environment capabilities, verifies where possible, flags what needs manual review |

## Modes

| Mode | What runs | When to use |
|------|-----------|-------------|
| Full Audit | All four phases | No specific scope provided |
| Scoped Audit | All phases, limited scope | Auditing a directory or file set |
| PR Audit | Changed files + impacted files | Reviewing a pull request |
| Audit-Only | Phases 1-3, no fixes | Want findings without remediation |
| Fix Mode | Phase 4 only | Applying fixes for known findings |

Fix mode requires edit/verification tools and explicit user request.

## Install

### Claude Code (recommended)

Clone into your global skills directory:

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/Speedrunlab/deep-audit.git ~/.claude/skills/deep-audit
```

Or into a specific project:

```bash
mkdir -p .claude/skills
git clone https://github.com/Speedrunlab/deep-audit.git .claude/skills/deep-audit
```

Then run `/deep-audit` in any Claude Code session.

### OpenClaw

Copy the skill into your OpenClaw agent's skills directory:

```bash
cp -r .claude/skills/deep-audit /path/to/your/openclaw/agent/.claude/skills/
```

### Any Agent Skills-compatible tool

The skill follows the Agent Skills open standard. Drop the `.claude/skills/deep-audit/` directory into any compatible tool.

## Usage

```
/deep-audit                    # Audit the entire project
/deep-audit src/auth           # Audit a specific directory
/deep-audit --scope=PR         # Audit the current PR diff
```

## What it catches

**Phase 2 (Security Review):**
- SQL, command, NoSQL, XSS, XXE, LDAP injection
- Broken authentication and access control
- Hardcoded secrets, PII in logs, sensitive data in URLs
- Weak cryptography, insecure random generation
- Dependency risk review, unpinned versions, confirmed CVEs when audit tooling is available
- Debug mode, default credentials, missing security headers

**Phase 3 (Deep Analysis):**
- Race conditions (double-spend, TOCTOU)
- State machine violations
- Floating point precision in financial calculations
- Uncaught exceptions, silent failures, resource leaks
- Missing timeouts, unbounded operations
- Dead code, inconsistent validation, copy-paste divergence
- Environment variables referenced but never validated

## Verification limits

- Dependency CVEs are confirmed only when local audit tooling or
  vulnerability database access is available. Otherwise reported as
  dependency risk.
- Runtime behavior is not verified unless executable test tools are
  available in the environment.
- Findings are based on observable repository evidence. Inferred risks
  are labelled as such.
- Fix application depends on the host environment. When edit tools are
  unavailable, fixes are proposed as diffs for manual application.

## What it does NOT do

- Regulatory or compliance auditing (AFSL, SOC2, HIPAA, etc.)
- Runtime or dynamic analysis
- Penetration testing
- Replace your existing security team

This is a code auditor. It reads code, finds problems, and fixes the critical ones.

## Philosophy

> Evidence only. No opinions without proof. Every finding earns its place with a file path, a line number, and reproducible evidence.

If the code is clean, it says so. If the code is dangerous, it says so plainly. No padding. No false positives to look thorough.

## License

MIT License. Use it, fork it, improve it.

## About

Built by [Speedrun AI Labs](https://speedrunlab.ai) in Sydney, Australia. We build AI tools that make a meaningful difference. This is the first skill in our OpenClaw Skill Library -- more coming.

Part of our research into closing the last 10% gap in AI-assisted software development. Read our papers at [speedrunlab.ai](https://speedrunlab.ai).


---

## Part of the Speedrun AI Labs Skill Library

This skill is part of the open-source skill library at [speedrunlab.ai](https://www.speedrunlab.ai).

Other skills:
- [/deep-audit](https://github.com/Speedrunlab/deep-audit) — four-phase code auditor with evidence standards
- [/no-slop-writing](https://github.com/Speedrunlab/no-slop-writing) — eliminates AI-speak from generated text (American English)
- [/no-slop-writing-au](https://github.com/Speedrunlab/no-slop-writing-au) — same, Australian English edition
- [/read-before-write](https://github.com/Speedrunlab/read-before-write) — forces AI agents to read files before editing them
