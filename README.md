# Claude Skills

> Custom slash commands that change how Claude thinks — on demand.

![Version](https://img.shields.io/badge/version-1.0.0-blue) ![Skills](https://img.shields.io/badge/skills-9-brightgreen) ![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## Install

```bash
# 1. Add marketplace
claude /plugin marketplace add https://github.com/rajat2911/claude-skills

# 2. Install plugin
claude /plugin install claude-skills@rajat2911-claude-skills
```

**Or test locally (no install needed):**

```bash
claude --plugin-dir /path/to/claude-skills/plugins/claude-skills
```

---

## Skills

| Skill | What it does | Invoke |
|---|---|---|
| [`/caveman-mode`](#caveman-mode) | Brutal brevity — strip all filler | `/caveman-mode start` |
| [`/rubber-duck`](#rubber-duck) | Claude asks YOU questions | `/rubber-duck` |
| [`/review-mode`](#review-mode) | Bugs, security, perf only. No praise. | `/review-mode` |
| [`/commit-msg`](#commit-msg) | Conventional commits from diff/desc | `/commit-msg` |
| [`/explain-eli5`](#explain-eli5) | Plain English, no jargon | `/explain-eli5` |
| [`/debug-mode`](#debug-mode) | Hypothesis → Test → Verify loop | `/debug-mode` |
| [`/devops-mode`](#devops-mode) | Reliability-first, blast radius first | `/devops-mode` |
| [`/tdd-mode`](#tdd-mode) | Tests before code. Always. | `/tdd-mode` |
| [`/dry-run`](#dry-run) | Confirm before any destructive command | `/dry-run` |

---

## caveman-mode

**~85% token reduction on explanatory answers.**

```
You:    /caveman-mode start

You:    explain how kubernetes works

Claude: Pods = containers.
        Nodes = machines running pods.
        Cluster = group of nodes.
        kubectl = you talk to cluster.
        Scheduler puts pods on nodes.
        Done.
```

vs the default (without caveman-mode):

```
Claude: Kubernetes is an open-source container orchestration platform that
        automates the deployment, scaling, and management of containerized
        applications. At its core, Kubernetes organizes containers into
        logical units called Pods, which are the smallest deployable units...
        [200 more words]
```

---

## rubber-duck

**Forces structured thinking. Claude asks questions, doesn't give answers.**

```
You:    /rubber-duck

You:    my API keeps returning 500 errors randomly

Claude: Walk me through what you expect to happen when the request comes in.

You:    it should hit the handler, query the DB, return JSON

Claude: What does "randomly" mean — same endpoint, same payload, different results?

You:    yes, same request sometimes works sometimes fails

Claude: When it fails, does it fail immediately or after a delay?

You:    after about 30 seconds

Claude: What do you think is causing it?

You:    ...oh. DB connection pool timeout. We have 10 connections and high traffic.

Claude: You got it.
```

---

## review-mode

**Bugs, security vulnerabilities, and performance issues only. Zero noise.**

````
You:    /review-mode

You:    review this

        ```python
        def get_user(user_id):
            query = f"SELECT * FROM users WHERE id = {user_id}"
            return db.execute(query)
        ```

Claude: [SEVERITY: critical] SQL injection vulnerability — line 2.
        user_id is interpolated directly into the query string.
        Fix: use parameterised queries.
        ```python
        query = "SELECT * FROM users WHERE id = %s"
        return db.execute(query, (user_id,))
        ```

        [SEVERITY: high] SELECT * fetches all columns including sensitive fields
        (passwords, tokens). Specify only needed columns.
````

---

## commit-msg

**Paste a diff or describe a change. Get back one commit message. Nothing else.**

```
You:    /commit-msg

You:    added retry logic to the payment processor with exponential backoff,
        max 3 retries, logs each attempt

Claude: feat(payments): add exponential backoff retry to payment processor

        Retries up to 3 times with exponential backoff on transient failures.
        Each attempt is logged for observability.
```

```
You:    /commit-msg

You:    fixed a typo in the README

Claude: docs: fix typo in README
```

---

## explain-eli5

**No jargon. Analogies only. One concept at a time.**

```
You:    /explain-eli5

You:    what is a database index?

Claude: Imagine a really thick book — like a dictionary.

        If you want to find the word "mango", you don't read every page from
        the start. You go to the index at the back, find "M", and it tells
        you exactly which page.

        A database index does the same thing. Instead of checking every row
        in a table, the database jumps straight to the right one.

        So basically, an index is a shortcut that makes finding data fast.
```

---

## debug-mode

**One hypothesis at a time. No scatter-gun suggestions.**

```
You:    /debug-mode

You:    my React component re-renders on every keystroke even though I didn't change state

Claude: What is the exact symptom — infinite loop or just excessive renders?

You:    just excessive, but it's making the app slow

Claude: What did you already try to fix it?

You:    added React.memo around the component

Claude: Hypothesis 1: a new object or function is being created on every parent render
        and passed as a prop, breaking memo's shallow comparison.

        Test: log the prop values before and after a keystroke with JSON.stringify.
        Are they equal?

You:    no, they're equal strings but console shows different object references

Claude: Hypothesis 1 confirmed. Root cause: prop is recreated each render.
        Fix: wrap the prop in useMemo or useCallback in the parent.
```

---

## devops-mode

**Reliability first. Blast radius before every answer.**

```
You:    /devops-mode

You:    we need to deploy a cron job that deletes old records from our DB every night

Claude: Blast radius: if the job runs wild (bug in WHERE clause), it could delete
        all records. If it hangs, it holds locks, blocking your app.

        Reliability concerns:
        - No retry on failure? Silent data loss.
        - No alerting? You won't know it stopped running.
        - Single instance? Fine for cron, but add a lock (pg_advisory_lock or Redis)
          to prevent overlap if the job ever takes longer than 24 hours.

        Recommended approach:
        1. Use a Kubernetes CronJob with concurrencyPolicy: Forbid
        2. Set activeDeadlineSeconds to kill hung jobs
        3. Add a Prometheus heartbeat metric — alert if job doesn't emit it
        4. Dry-run the DELETE as a SELECT first in staging

        Failure mode: job silently stops running → stale data accumulates →
        detect via alerting on last-success timestamp.
```

---

## tdd-mode

**Tests before code. No exceptions.**

```
You:    /tdd-mode

You:    write a function that validates an email address

Claude: Tests first:

        ```python
        def test_valid_email():
            assert validate_email("user@example.com") == True

        def test_missing_at_sign():
            assert validate_email("userexample.com") == False

        def test_missing_domain():
            assert validate_email("user@") == False

        def test_empty_string():
            assert validate_email("") == False

        def test_multiple_at_signs():
            assert validate_email("a@b@c.com") == False
        ```

        Do these tests cover your requirements? Anything missing?

You:    looks good

Claude: ```python
        import re

        def validate_email(email: str) -> bool:
            pattern = r'^[^@]+@[^@]+\.[^@]+$'
            return bool(re.match(pattern, email))
        ```
```

---

## dry-run

**Shows exactly what will happen before any destructive command runs.**

```
You:    /dry-run

You:    git push --force origin main

Claude: ┌─────────────────────────────────────────┐
        │  DRY RUN — not executed yet             │
        │                                         │
        │  Action:     force push to main         │
        │  Affected:   origin/main (remote)       │
        │  Overwrites: any commits not in local   │
        │  Reversible: NO — remote history lost   │
        │  Risk:       CRITICAL                   │
        └─────────────────────────────────────────┘

        Confirm? (yes/no)

You:    no

Claude: Aborted. Consider --force-with-lease instead — it only pushes if
        no one else has pushed since you last fetched.
```

---

## Contributing

Have a useful skill? Open a PR.

```
plugins/claude-skills/skills/
└── your-skill-name/
    └── SKILL.md    # frontmatter (name, description) + instructions
```

1. Fork the repo
2. Create `plugins/claude-skills/skills/<your-skill>/SKILL.md`
3. Add an example to `README.md`
4. Open a PR to `master`

---

## How it works

Each skill is a markdown file. When you invoke `/skill-name`, Claude Code injects those instructions into its context — changing how it thinks and responds for the rest of the session.

No external API calls. No data sent anywhere. Just prompt engineering, packaged cleanly.
