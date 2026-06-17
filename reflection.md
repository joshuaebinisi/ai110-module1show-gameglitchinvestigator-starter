# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the hints were backwards").
When I first launched the application, the game exposed the developer debug information to all users instead of requiring a password. Second, the "New Game" button failed to clear previous guesses from the text box. It also failed to re-lock the debug panel after a new round started. Last, the number entry field had validation issues regarding the 1 to 100 range, affecting the loop.

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
|Launching the app|Debug info is hidden and requires a password(1234)|Debug info is fully visible to the regular user by default.|none|
|Clicking "New Game" after guessing a number|The guess input text box clears completely for the new round.|The previous number remains stuck in the text box.|none|
|Clicking "New Game" while debug info is unlocked|The developer debug info automatically locks again.|The developer debug info remains unlocked and visible.|none|
|| | | |

---

## 2. How did you use AI as a teammate?

- I used Claude (Claude Code) as my main AI tool throughout this project. I gave it tasks directly in the editor and it read the files, spotted the bugs, and made the changes.

Casting fix in `check_guess`:
  - The original `app.py` was secretly turning the secret number into a string every other attempt (line 174–177), so when `check_guess` tried to compare an int guess to a string secret, it either crashed or gave wrong results. Claude caught this and suggested fixing it in two places: cast both `guess` and `secret` to `int` inside `check_guess` so the function always works no matter what type gets passed in, and remove the alternating string conversion from `app.py` entirely. I verified it by running the new `test_string_inputs_cast_to_int` pytest test, which passes `"60"` and `"50"` as strings and confirms the outcome still comes back as `"Too High"`.

Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
---
What the AI suggested:
- The AI told me to use raw.isnumeric() in parse_guess to check if the input is a number.

Why it seemed correct at first:
- It is a real Python method. It seems like a clean way to check a string without using a try block.

How I figured out it was wrong:
- I tested the game by typing a decimal like "3.14". The code rejected it. The isnumeric() method only accepts whole numbers and breaks if there is a period. I had to reject the code and use a try block instead.
---

## 3. Debugging and testing your fixes

I decided a bug was really fixed when I could either run a pytest test that passed, or manually walk through the logic and confirm the output matched what the game should actually do.

For the type casting bug, I ran `pytest tests/test_game_logic.py` after adding `test_string_inputs_cast_to_int`. Before the fix, `check_guess("60", "50")` would have broken because Python can't compare a string and an int with `>` cleanly, and the old fallback code was doing string comparison which gives wrong ordering (e.g., `"9" > "10"` is True alphabetically). After casting both to int inside `check_guess`, the test passed and the comparison worked correctly.

Claude helped me design the string input test — it pointed out that since the bug was about type mismatches, the best test was one that deliberately passed strings in and confirmed the right outcome came back. That made it a direct test of the fix, not just a general check.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
