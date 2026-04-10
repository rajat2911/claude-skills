# Claude Skills

A collection of Claude Code skills to supercharge your developer workflow.

## How Skills Work

Claude Code skills are custom slash commands that change Claude's behaviour on demand. When you install this plugin, Claude Code loads the skill definitions and makes them available as `/commands` in your session.

Each skill is a markdown file with instructions injected into Claude's context when invoked — telling it how to behave differently.

## Install

```bash
# Step 1: Add marketplace
claude /plugin marketplace add https://github.com/rajat2911/claude-skills

# Step 2: Install plugin
claude /plugin install claude-skills@rajat2911-claude-skills
```

**Test locally (no install needed):**
```bash
claude --plugin-dir /path/to/claude-skills
```

---

## Skills

### `/caveman-mode` — Extreme Brevity
Strips all filler. Bare minimum responses only. ~85% token reduction on explanatory answers.
```
/caveman-mode start    # enable
/caveman-mode stop     # disable
```

---

### `/rubber-duck` — Rubber Duck Debugging
Claude asks YOU questions instead of giving answers. Forces structured thinking to find your own solution.
```
/rubber-duck
```

---

### `/review-mode` — Strict Code Review
Flags only bugs, security issues, and performance problems. No praise. No style suggestions. Every issue gets a severity: `critical / high / medium / low`.
```
/review-mode
```

---

### `/commit-msg` — Conventional Commit Generator
Give it a diff or description. Get back a conventional commit message. Nothing else.
```
/commit-msg
```

---

### `/explain-eli5` — Explain Like I'm 5
Analogies only. No jargon. Every concept explained in plain language a 5-year-old would understand.
```
/explain-eli5
```

---

### `/debug-mode` — Systematic Debugging
Hypothesis → Test → Verify loop. One hypothesis at a time. No random suggestions. Methodical root cause analysis.
```
/debug-mode
```

---

### `/devops-mode` — DevOps / SRE Lens
Every answer considers reliability, security, cost, scalability — in that order. Flags single points of failure. States blast radius before any solution.
```
/devops-mode
```

---

### `/tdd-mode` — Test-Driven Development
Refuses to write implementation before tests. Enforces Red → Green → Refactor cycle.
```
/tdd-mode
```

---

### `/dry-run` — Safety Mode for Destructive Operations
Before any destructive command (git force push, terraform apply, kubectl delete, DROP TABLE), shows exactly what will happen and asks for confirmation.
```
/dry-run
```

---

## Contributing

Have a useful skill? Open a PR.

```
skills/
└── your-skill-name/
    └── SKILL.md    # frontmatter (name, description) + instructions
```
