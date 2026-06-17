# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable. 

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the broken app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [ ] Describe the game's purpose.
- [ ] Detail which bugs you found.
- [ ] Explain what fixes you applied.

## 📸 Demo Walkthrough

The following is a sample game played on **Normal** difficulty (range 1–100, 8 attempts allowed). The secret number is **42**.

1. **Launch the app.** Run `python -m streamlit run app.py`. The page title reads "🎮 Game Glitch Investigator". The sidebar shows *Difficulty: Normal*, *Range: 1 to 100*, *Attempts allowed: 8*.
2. **Start guessing — too high.** Type `70` in the guess box and click **Submit Guess 🚀**. The hint banner reads *"📉 Go LOWER!"* and the attempts counter drops to 7 remaining.
3. **Narrow it down — too low.** Type `30` and submit. The hint reads *"📈 Go HIGHER!"* — 6 attempts left.
4. **Close the gap — too high.** Type `55` and submit. Hint: *"📉 Go LOWER!"* — 5 attempts left.
5. **Binary-search further — too low.** Type `40` and submit. Hint: *"📈 Go HIGHER!"* — 4 attempts left.
6. **One away — too high.** Type `50` and submit. Hint: *"📉 Go LOWER!"* — 3 attempts left.
7. **Correct guess — win!** Type `42` and submit. Balloons fill the screen and the success banner reads *"You won! The secret was 42. Final score: 60"*. The Submit button is disabled for the rest of this round.
8. **Start a new round.** Click **New Game 🔁**. The input box clears, the attempt counter resets to 8, the debug panel re-locks, and a fresh secret number is generated.

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
# Paste your pytest terminal output below (Challenge 1).
# Run: pytest tests/test_game_logic.py -v
# Then copy and paste the full output here.
#
# Example format:
# ========================= test session starts =========================
# collected X items
#
# tests/test_game_logic.py::test_correct_guess PASSED           [ XX%]
# tests/test_game_logic.py::test_too_high PASSED                [ XX%]
# tests/test_game_logic.py::test_too_low PASSED                 [ XX%]
# tests/test_game_logic.py::test_string_inputs_cast_to_int PASSED [ XX%]
# ...
# ========================= X passed in 0.XXs =========================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
