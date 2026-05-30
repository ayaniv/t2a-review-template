# t2a-review-template

> An AI code reviewer that thinks like your team — built from your team's actual PR comment history.

Forked from the system described in [We Accidentally Built an AI Code Reviewer That Thinks Like Us](https://medium.com/@ayaniv29/we-accidentally-built-an-ai-code-reviewer-that-thinks-like-us-5527486385bf).

## What it does

`/t2a-review` runs a multi-agent pre-PR review against your diff. It applies:

- Your team's reviewer profiles (mined from real PR comment history)
- Your codebase's conventions (`checklist.md`)
- A general engineering quality pass
- An Opus skeptic that filters false positives

Cost: ~$3 per review on a typical 400-500 line diff (Haiku + Sonnet + Opus).

## Prerequisites

- [Claude Code](https://claude.ai/code)
- `gh` CLI authenticated to your GitHub org

## Setup

### 1. Clone this repo into your project (or a shared team repo)

```bash
git clone https://github.com/ayaniv/t2a-review-template
```

### 2. Configure your repos

Edit `config.md` and set your GitHub repos — the ones your team reviews PRs in.

### 3. Generate reviewer profiles

For each team member who reviews PRs, run:

```
/t2a-review-add-team-member <github-username>
```

This mines their last 3 years of PR review comments, synthesizes recurring patterns, and writes a profile to `team-members/<username>.md`. Takes ~5-10 minutes for prolific reviewers.

Or write profiles manually — see `team-members/_template.md` for the format.

### 4. Update checklist.md

Replace the placeholder checklist with your team's actual conventions — framework preferences, naming rules, test requirements, etc.

### 5. Run it

```
/t2a-review
```

Run it on your local diff before pushing a PR for review. Pass a PR URL to review an existing PR:

```
/t2a-review https://github.com/your-org/your-repo/pull/123
```

## File structure

```
.claude/
  commands/
    t2a-review.md                  # main review skill
    t2a-review-add-team-member.md  # profile generator skill
team-members/
  _template.md                     # format reference
  <username>.md                    # one file per reviewer
checklist.md                       # your team's base conventions
config.md                          # repo configuration
```

## Adding a new team member

```
/t2a-review-add-team-member <github-username>
```

The skill fetches all PRs they reviewed in the last 3 years, extracts patterns, shows you a draft, and writes the file on confirmation.

## Updating a profile

Open `team-members/<username>.md` and edit it directly. Each profile is one file — no coordination needed with the rest of the team.

## License

MIT
