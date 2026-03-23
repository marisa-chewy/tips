# tips

Turn Claude Code's "Thinking..." spinner into a personalized learning tool.

## What is this?

When Claude Code is working, you stare at a spinner. By default it says things like "Analyzing..." or "Researching..." — not very useful.

This skill replaces that dead time with **micro-learning moments**: technical vocab definitions, workflow tips, and power-user tricks tailored to your skill level and what you actually use Claude Code for.

A People leader who's new to dev tools? You'll learn what "API" and "webhook" mean while you wait. A senior engineer? You'll get deep-cut shortcuts and advanced patterns. Everyone gets something useful.

## How to use it

1. Open Claude Code
2. Point it at the skill file:

```
@claude-spinner-tips.md set up my spinner tips
```

3. Answer 3 quick questions:
   - **Your skill level** (beginner / intermediate / advanced)
   - **What you use Claude Code for** (writing, data, code, automation)
   - **How fancy you want to get** (basic 2-min setup vs. full auto-refresh system)

4. Claude generates personalized content and installs it. Done.

## What it looks like

Instead of this:
```
⠋ Thinking...
```

You see things like:
```
⠋ webhook — auto-notification between apps
```

And tips like:
```
💡 You can chain tools: read data from Sheets, analyze in Python, post results to Notion
```

## Two tiers

| | Basic | Full System |
|---|---|---|
| **Time** | ~2 min | ~10 min |
| **What it does** | Swaps spinner text | Adds auto-refresh, loader script, status bar |
| **Best for** | Just want custom tips | Want tips that evolve with your work |

## The ethos

This repo is about making tools work harder for the humans using them. The best customizations are the ones that turn passive moments into active ones — even something as small as a loading spinner.

I'm not an engineer. I'm a People leader at [Valon](https://valon.com) who started using Claude Code and realized that the best way to learn technical concepts is to see them *constantly*, in context, without having to go looking. This was my first real "hack" and I wanted to share it.

If you build something cool with this, I'd love to hear about it.

---

## We're hiring

This skill was built at [Valon](https://valon.com), where we're using AI to transform mortgage servicing. We're building **Valon.ai** — and we're looking for people who are excited about what happens when you put powerful tools in the hands of every team, not just engineering.

If you're curious about working somewhere that treats AI adoption as a company-wide sport, check out our [careers page](https://valon.com/careers).

---

## License

MIT — do whatever you want with it.
