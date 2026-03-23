---
name: claude-spinner-tips
description: "Turn Claude Code's thinking spinner into a personalized learning tool. Instead of watching 'Thinking...' while Claude works, see useful tips, vocab definitions, or power-user tricks tailored to your skill level. Interactive setup with basic (2 min) and advanced (10 min) tiers. Use when someone wants to customize their Claude Code spinner, make waiting time useful, or personalize their Claude Code experience. Also triggers on: spinner, thinking animation, claude tips, customize claude, waiting text, spinner customization."
---

# Claude Code Spinner Tips

Turn Claude Code's "Thinking..." spinner into something actually useful — personalized tips, vocab builders, and power-user tricks that show up while Claude works.

## What This Does

By default, Claude Code shows random verbs while it's thinking ("Analyzing...", "Researching...", etc.) and generic tips. This skill replaces both with content tailored to *you*:

- **Spinner verbs** → mini vocab lessons (e.g., "API — how apps talk to each other") or domain-specific terms
- **Spinner tips** → actionable things you can try in Claude Code, matched to your skill level

Every time Claude thinks (which is a lot), you're learning something instead of just waiting.

---

## How to Use This Skill

Walk the user through an interactive setup. Use AskUserQuestion for each decision point to keep it smooth. The whole thing should feel like a quick, fun customization — not a technical install.

**Start by saying:**
> "Let's turn your Claude Code spinner into something useful! Right now when Claude is thinking, you see generic text — we're going to replace it with tips and vocab that are actually relevant to you. I'll ask you a few questions to personalize it, then set everything up. Takes about 2 minutes for the basic version."

Then proceed through the phases below.

---

## Phase 1: Personalization

Ask these questions using AskUserQuestion to understand what content to generate.

### Question 1: Skill Level
> "How would you describe your comfort level with developer tools and technical concepts?"

Options:
- **Beginner** — "I'm new to terminal, git, and coding concepts. I'd love mini definitions of technical terms." → Generate beginner-friendly vocab verbs + discovery-oriented tips
- **Intermediate** — "I know the basics but I'm still building muscle memory. Show me what I'm missing." → Generate intermediate vocab + efficiency/workflow tips
- **Advanced** — "I'm technical and comfortable with dev tools. Surprise me with power-user stuff." → Generate advanced/niche vocab + deep-cut tips and shortcuts

### Question 2: What You Use Claude Code For
> "What do you mainly use Claude Code for? Pick all that apply."

Options (multi-select):
- **Writing & content** — blog posts, emails, docs, comms
- **Data & analysis** — spreadsheets, Python scripts, SQL, dashboards
- **Code & engineering** — building features, debugging, code review
- **Workflow automation** — connecting tools, building skills, process design

This shapes the *domain* of the tips (e.g., a data person gets tips about chaining Sheets → Python → Notion; a writer gets tips about voice matching and editing workflows).

### Question 3: Setup Level
> "How fancy do you want to get?"

Options:
- **Basic (2 min)** — "Just customize my spinner text. Simple and done." → Tier 1 only
- **Full system (10 min)** — "Give me the whole thing — auto-refresh, status bar, the works." → Tier 1 + Tier 2

---

## Phase 2: Generate Personalized Content

Based on their answers, generate a `spinner-content.json` file with:

### Verbs (20-25)
Generate vocabulary terms with short plain-language definitions, calibrated to their skill level:

**Beginner examples:**
- "API — how apps talk to each other"
- "git commit — saving a snapshot of your work"
- "JSON — the universal data format"

**Intermediate examples:**
- "middleware — code that runs between request and response"
- "race condition — when timing causes unexpected bugs"
- "idempotent — safe to run multiple times, same result"

**Advanced examples:**
- "backpressure — slowing input when output can't keep up"
- "AST — the tree structure a compiler sees in your code"
- "eventual consistency — distributed systems trade-off: fast writes, delayed reads"

Mix in domain-specific terms based on their use cases (Question 2). A data person gets "DataFrame", "pivot table", "ETL". A writer gets "frontmatter", "markdown", "slug".

### Tips (15-20)
Generate actionable Claude Code tips calibrated to skill level and use case:

**Beginner examples:**
- "Use @ to attach any file to your message — Claude reads it instantly"
- "Say 'explain this like I'm not technical' for any confusing output"
- "Ask Claude to explain a git error before you panic — it's usually simpler than it looks"

**Intermediate examples:**
- "Try creating a skill (/skill-creator) to package a workflow you repeat often"
- "You can chain tools: read data from Sheets, analyze in Python, post results to Notion"
- "Use /compact when your conversation gets long — it frees up context window space"

**Advanced examples:**
- "Create SessionStart hooks to auto-run setup tasks when Claude Code launches"
- "Use 'mode: replace' in spinnerVerbs to fully override defaults instead of appending"
- "Build MCP servers to give Claude access to your internal tools and APIs"

Format the JSON file like this:
```json
{
  "lastRefreshed": "YYYY-MM-DD",
  "verbs": [
    "term — definition",
    ...
  ],
  "tips": [
    "Tip text here",
    ...
  ]
}
```

**Save this file to `~/.claude/spinner-content.json`.**

---

## Phase 3: Apply the Settings (Tier 1 — Basic)

This is the core change. Update `~/.claude/settings.json` to add two keys:

### Read their current settings.json first
Read `~/.claude/settings.json` to see what's already there. We're *adding* keys, not replacing the whole file.

### Add spinner configuration
Add these two keys to their settings.json (merge with existing content):

```json
{
  "spinnerVerbs": {
    "mode": "replace",
    "verbs": [
      // paste the verbs array from spinner-content.json
    ]
  },
  "spinnerTipsOverride": {
    "excludeDefault": true,
    "tips": [
      // paste the tips array from spinner-content.json
    ]
  }
}
```

**Key details:**
- `"mode": "replace"` swaps out the default verbs entirely (otherwise they just get added to the defaults)
- `"excludeDefault": true` hides the built-in tips so only the custom ones show

**After saving, tell them:**
> "Done! Next time Claude is thinking, you'll see your custom tips and vocab instead of the defaults. Try asking Claude something and watch the spinner."

**If they chose Basic setup, stop here.** Congratulate them and remind them they can re-run this skill anytime to refresh their content.

---

## Phase 4: Full System Setup (Tier 2 — Advanced)

Only do this if they chose "Full system" in Question 3.

### 4A: Loader Script

Create `~/.claude/load-spinner.sh` — a script that reads spinner-content.json and syncs it into settings.json. This makes it easy to update content without hand-editing settings.

```bash
#!/bin/bash
# Reads spinner-content.json and updates settings.json with current verbs and tips

CONTENT_FILE="$HOME/.claude/spinner-content.json"
SETTINGS_FILE="$HOME/.claude/settings.json"

if [ ! -f "$CONTENT_FILE" ] || [ ! -f "$SETTINGS_FILE" ]; then
  echo "Missing required files"
  exit 1
fi

python3 -c "
import json

with open('$CONTENT_FILE') as f:
    content = json.load(f)

with open('$SETTINGS_FILE') as f:
    settings = json.load(f)

settings['spinnerVerbs'] = {
    'mode': 'replace',
    'verbs': content['verbs']
}

settings['spinnerTipsOverride'] = {
    'excludeDefault': True,
    'tips': content['tips']
}

with open('$SETTINGS_FILE', 'w') as f:
    json.dump(settings, f, indent=2)
    f.write('\n')

print('Spinner content loaded: {} verbs, {} tips'.format(len(content['verbs']), len(content['tips'])))
"
```

Tell them: "Now whenever you update spinner-content.json, just run `bash ~/.claude/load-spinner.sh` to sync the changes."

### 4B: Auto-Refresh Hook

Create `~/.claude/refresh-spinner.sh` — checks if spinner content is older than 7 days and nudges Claude to refresh it during a session.

```bash
#!/bin/bash
# Checks if spinner content is >7 days old and signals a refresh

CONTENT_FILE="$HOME/.claude/spinner-content.json"

if [ ! -f "$CONTENT_FILE" ]; then
  echo '{"systemMessage": "Spinner content file missing — regenerate it."}'
  exit 0
fi

LAST_REFRESHED=$(python3 -c "
import json
with open('$CONTENT_FILE') as f:
    data = json.load(f)
print(data.get('lastRefreshed', '2000-01-01'))
" 2>/dev/null)

DAYS_OLD=$(python3 -c "
from datetime import datetime
last = datetime.strptime('$LAST_REFRESHED', '%Y-%m-%d')
now = datetime.now()
print((now - last).days)
" 2>/dev/null)

if [ -z "$DAYS_OLD" ]; then exit 0; fi

if [ "$DAYS_OLD" -ge 7 ]; then
  echo "{\"hookSpecificOutput\": {\"hookEventName\": \"SessionStart\", \"additionalContext\": \"The spinner tips and verbs in ~/.claude/spinner-content.json are $DAYS_OLD days old. Refresh them: swap out ~5 verbs and ~5 tips based on what the user has been working on recently. Update lastRefreshed date. Then run: bash ~/.claude/load-spinner.sh\"}}"
fi
```

Then add the hook to their settings.json under `hooks.SessionStart`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash ~/.claude/refresh-spinner.sh"
          }
        ]
      }
    ]
  }
}
```

Tell them: "Now Claude will automatically check if your tips are getting stale and refresh them with new content based on what you've been working on. It's like a self-updating learning feed."

### 4C: Status Line (Bonus)

Ask: "Want a custom status bar too? It shows your model name and how much of Claude's memory you've used as a progress bar."

If yes, create `~/.claude/statusline.sh`:

```bash
#!/bin/bash
input=$(cat)
model=$(echo "$input" | jq -r '.model.display_name // "Claude"')
used_pct=$(echo "$input" | jq -r '.context_window.used_percentage // empty')

if [ -z "$used_pct" ]; then
  echo "$model"
  exit 0
fi

used_pct=$(printf "%.0f" "$used_pct")
bar_width=20
filled=$(( used_pct * bar_width / 100 ))
empty=$(( bar_width - filled ))

bar=""
for ((i=0; i<filled; i++)); do bar="${bar}█"; done
for ((i=0; i<empty; i++)); do bar="${bar}░"; done

echo "$model | [$bar] ${used_pct}%"
```

And add to settings.json:
```json
{
  "statusLine": {
    "type": "command",
    "command": "bash ~/.claude/statusline.sh"
  }
}
```

---

## Phase 5: Wrap Up

Summarize what was set up:
- **Basic:** "Your spinner now shows [X] custom vocab terms and [Y] tips tailored to your level. Watch for them next time Claude is thinking!"
- **Full system:** "You've got the full setup — custom spinner content, auto-refresh every 7 days, and a status bar. Your Claude Code experience is now fully personalized."

Remind them:
- To refresh manually anytime: edit `~/.claude/spinner-content.json` and run `bash ~/.claude/load-spinner.sh`
- To re-run this skill with different preferences: just invoke it again
- The content evolves with them — as they level up, they can re-run with a higher skill level

---

## Content Guidelines

When generating verbs and tips, follow these principles:

1. **Verbs should teach, not just label.** "API — how apps talk to each other" is better than just "API"
2. **Tips should be actionable.** Start with a verb: "Use...", "Try...", "Ask Claude to..."
3. **Match the user's world.** A People/HR person gets different examples than a backend engineer
4. **Keep it conversational.** These show up in a spinner — they should be quick reads, not documentation
5. **Mix learning + doing.** Some tips teach a concept, others prompt action
6. **No duplicates of built-in tips** when using excludeDefault: false (only matters if someone wants to keep defaults)
