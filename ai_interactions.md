# AI Interactions Log

> **Stretch features only.** Only fill in the sections that apply to stretch features you attempted. If you did not attempt a stretch feature, leave its section blank or delete it. This file is not required for the core project.

---

## Agent Workflow (SF8)

> Document your experience using an AI agent (e.g., Cursor Agent, Claude, Copilot) to make multi-step changes autonomously.

**What task did you give the agent?**

I asked it to move the four functions from `app.py` into `logic_utils.py` to replace the stubs, update the import, and fix the bugs.

**What did the agent do?**

1. Read both `app.py` and `logic_utils.py` first to see what was going on.
2. Found all the bugs before changing anything:
   - `get_range_for_difficulty`: Hard was set to 1–50, which is actually easier than Normal (1–100).
   - `check_guess`: the hint messages were backwards — "Too High" said "Go HIGHER!" and "Too Low" said "Go LOWER!".
   - `check_guess`: it was trying to compare strings and ints instead of just always using ints.
   - `update_score`: it was giving +5 points on even attempts for a wrong guess, which makes no sense.
   - `app.py` line 174–177: every other attempt it was turning the secret into a string, which broke the comparisons.
   - `app.py` line 96: attempts started at 1 instead of 0, which messed up the score math.
3. Moved all four functions into `logic_utils.py` with the fixes applied.
4. Updated `app.py` to:
   - Add the import line at the top.
   - Delete the four old function definitions.
   - Remove the string/int switching code and just pass the secret directly.
   - Change attempts to start at 0.

**What did you have to verify or fix manually?**

- I had to make sure the bugs it found were actually wrong and not just how the game was supposed to work (like checking that Hard really should be harder, not easier).
- I double checked the import matched the actual function names in `logic_utils.py`.
- I looked over `app.py` after to make sure nothing was broken by removing the functions.

---

## Test Generation (SF7)

> Document how you used AI to help generate or improve tests.

| Edge Case | Prompt Used | AI-Suggested Test | Did It Pass? | Your Reasoning |
|-----------|-------------|-------------------|--------------|----------------|
| | | | | |
| | | | | |
| | | | | |

---

## Linting & Style (SF9)

> Document your use of AI for linting or code style improvements.

**Prompt used:**

```
<!-- Paste the prompt you gave the AI -->
```

**Linting output before:**

```
<!-- Paste relevant linter warnings/errors -->
```

**Changes applied:**

<!-- Describe what you changed based on the AI's suggestions -->

---

## Model Comparison (SF11)

> Compare two AI models on the same task.

**Task given to both models:**

<!-- Describe what you asked each model to do -->

| | Model A | Model B |
|-|---------|---------|
| **Model name** | | |
| **Response summary** | | |
| **More Pythonic?** | | |
| **Clearer explanation?** | | |

**Which did you prefer and why?**

<!-- Your conclusion -->
