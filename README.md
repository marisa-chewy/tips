# tips

Turn Claude Code's "Thinking..." spinner into a personalized learning tool.

## What is this?

When Claude Code is working, you stare at a spinner. By default it says things like "Analyzing..." or "Researching..." which is... not very useful.

This skill replaces that dead time with **micro-learning moments**: technical vocab definitions, workflow tips, and power-user tricks. All tailored to your skill level and what you actually use Claude Code for.

New to dev tools? You'll learn what "API" and "webhook" mean while you wait. Senior engineer? You'll get deep-cut shortcuts and advanced patterns. Everyone gets something useful.

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

4. Claude generates personalized content and installs it. Done!

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

## Why I built this

I'm not an engineer. I'm a People leader who started using Claude Code about two months ago and immediately thought "there has to be a way to make this waiting time useful."

Turns out there is! You can customize the spinner text in your settings. So I filled mine with beginner-friendly technical vocab and tips for things I didn't know Claude Code could do. Now every time Claude thinks, I'm picking up a new concept without even trying.

It's a small thing, but it genuinely changed how I learn. Two months in and I've absorbed dozens of technical terms just from watching the spinner. No flashcards, no tutorials. Just little moments of "oh, that's what that means" while I wait.

If you build something cool with this, I'd love to hear about it!

---

## We're hiring!

I built this at [Valon](https://valon.ai), where we're using AI to transform mortgage servicing. We're building Valon.ai and we're looking for people who are excited about what happens when you put powerful tools in the hands of every team, not just engineering.

If that sounds interesting to you, check out our [careers page](https://valon.ai/careers) :)

---

## License

MIT. Do whatever you want with it!
