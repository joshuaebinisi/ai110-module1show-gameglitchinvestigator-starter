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
When I asked for help validating user input inside `parse_guess`, the AI recommended using `raw.isnumeric()` to check whether the string was a valid number before converting it. It presented this as a clean, Pythonic approach that avoids the overhead of a `try`/`except` block.

Why it seemed correct at first:
`isnumeric()` is a real, built-in Python string method. The name sounds like exactly what we need, is this thing numeric? so it was easy to trust without questioning it.

How I figured out it was wrong:
I manually tested the game by typing `"3.14"` into the guess field. The code rejected it as invalid even though `3.14` is clearly a number. I traced back to `isnumeric()` and looked it up: it only returns `True` for strings made entirely of digit characters (like `"42"`), so the decimal point in `"3.14"` causes it to return `False`. It also fails on negative numbers like `"-5"` because of the minus sign. The AI's suggestion looked clean on the surface but broke on any real-world decimal input. I replaced it with a `try`/`except` block that calls `int()` (or `int(float())` for decimals), which handles all those cases correctly. This taught me that AI can suggest methods that exist and sound right but cover a narrower range of inputs than needed.

---

## 3. Debugging and testing your fixes

I decided a bug was really fixed when I could either run a pytest test that passed, or manually walk through the logic and confirm the output matched what the game should actually do.

For the type casting bug, I ran `pytest tests/test_game_logic.py` after adding `test_string_inputs_cast_to_int`. Before the fix, `check_guess("60", "50")` would have broken because Python can't compare a string and an int with `>` cleanly, and the old fallback code was doing string comparison which gives wrong ordering (e.g., `"9" > "10"` is True alphabetically). After casting both to int inside `check_guess`, the test passed and the comparison worked correctly.

Claude helped me design the string input test: it pointed out that since the bug was about type mismatches, the best test was one that deliberately passed strings in and confirmed the right outcome came back. That made it a direct test of the fix, not just a general check.

---

## 4. What did you learn about Streamlit and state?

Every time a user clicks a button or types anything in a Streamlit app, the entire Python script reruns from top to bottom, it is not like a normal website where only one function fires. This means any regular variable you declare, like `secret = random.randint(1, 100)`, gets reset to a brand-new value on every click. The fix is `st.session_state`: a dictionary that Streamlit keeps alive between reruns. You write `st.session_state.secret = random.randint(1, 100)` once (guarded by an `if "secret" not in st.session_state` check) and from that point on the same value survives every rerun until you deliberately change it. If I were explaining this to a friend, I would say: imagine your script is a function that gets called fresh every time you touch anything on the page: `session_state` is the one notepad that does not get erased between calls.

---

## 5. Looking ahead: your developer habits

One habit I want to keep: Writing a pytest test immediately after fixing a bug, not after the whole project is done. On this project, adding `test_string_inputs_cast_to_int` right after the type-casting fix made it easy to confirm the fix worked and would not regress later. That tight loop fix, write test, run test is something I want to do on every future project.

One thing I would do differently: I would test AI suggestions against case inputs before accepting them. The `isnumeric()` mistake happened because I read the suggestion, thought that it looked right, and moved on without actually trying a decimal or a negative number. Next time I will treat every AI suggested input validation method as unproven until I have typed a weird value into it myself.

How this project changed my thinking: 
  - I used to assume that if AI-generated code ran without crashing, it was probably correct. This project showed me that code can run, give feedback, and still be wrong in ways that only appear on specific inputs; the hints were completely backwards and the secret kept changing, and neither issue threw an error. AI is a fast starting point, but it requires the same skeptical testing I would give any other code.
