# Claude Skills

A collection of Claude Code skills to supercharge your workflow.

## How Skills Work

Claude Code skills are custom slash commands that change Claude's behaviour on demand. When you install this plugin, Claude Code loads the skill definitions and makes them available as `/commands` in your session.

Each skill is a markdown file with instructions that get injected into Claude's context when invoked — telling it how to behave differently.

## Install

**Prerequisites:** [Claude Code](https://claude.ai/code) installed.

```bash
claude /plugin install rajatverma/claude-skills
```

After install, skills are available immediately in any Claude Code session.

---

## Skills

### `/caveman-mode` — Extreme Brevity Mode

Switches Claude from verbose AI assistant to bare-minimum communicator. Every response is stripped to the essential information only.

#### Why use it?

Claude by default adds:
- Introductions ("Great question! Let me help you with...")
- Explanations of what it's about to do
- Summaries after doing it
- Politeness filler

All of that consumes tokens and your reading time. Caveman mode cuts all of it.

#### Token savings

Normal Claude response to "what does git stash do?":
> "Great question! `git stash` is a very useful Git command that temporarily saves your uncommitted changes so you can switch branches or work on something else without committing incomplete work. Here's how it works: it takes your modified tracked files and staged changes and saves them on a stack of unfinished changes that you can reapply at any time..."
> — ~80 tokens

Caveman mode response:
> "Saves uncommitted changes to a stack. Cleans working tree."
> — ~10 tokens

**~85% token reduction** on explanatory responses.

#### Usage

```
/caveman-mode start    # enable caveman mode
/caveman-mode stop     # return to normal
```

#### Example

```
You:     /caveman-mode start
Claude:  Caveman mode on.

You:     how do i reverse a list in python
Claude:  my_list[::-1]

You:     /caveman-mode stop
Claude:  Normal mode restored.
```

#### When to use it

- Quickly looking up syntax
- Getting code snippets without explanation
- Already know the context, just need the answer
- Burning through a long session and want to preserve context

---

## Contributing

Have a useful skill? Open a PR.

Skill file format:
```
skills/
└── your-skill-name/
    └── SKILL.md        # frontmatter (name, description) + instructions
```
