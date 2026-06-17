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

1. Play the game. Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. Find the State Bug. Why does the secret number change every time you click "Submit"? Ask ChatGPT: "How do I keep a variable from resetting in Streamlit when I click a button?"
3. Fix the Logic. The hints ("Higher/Lower") are wrong. Fix them.
4. Refactor and Test. Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [x] Describe the game's purpose. The app is a number-guessing game built with Streamlit where the player tries to guess a secret number within a set number of attempts. The sidebar lets the player pick a difficulty (Easy, Normal, or Hard), which changes the number range and attempt limit. After each guess the game shows a hint telling the player to go higher or lower, and the score updates based on how many attempts it took to win.

- [x] Detail which bugs you found.
  - The secret number regenerated on every button click because it was declared as a plain variable instead of being stored in `st.session_state`, so Streamlit reset it on every rerun.
  - The Higher/Lower hints were reversed. Guessing too high showed "Go HIGHER!" and too low showed "Go LOWER!".
  - On even-numbered attempts, `app.py` silently converted the secret number to a string, which caused `check_guess` to do string comparison instead of integer comparison and produce wrong results.
  - The Developer Debug Info panel was open by default with no password, exposing the secret number to all users.
  - Clicking "New Game" did not clear the guess input box or re-lock the debug panel.
  - The scoring logic rewarded points for wrong guesses on even attempts instead of always penalizing wrong guesses.

- [x] Explain what fixes you applied.
  - Wrapped the secret number, attempts, score, status, history, and game ID in `st.session_state` so they survive reruns without resetting.
  - Corrected the comparison in `check_guess` so `guess > secret` returns "Too High" and `guess < secret` returns "Too Low".
  - Added `int()` casts for both `guess` and `secret` inside `check_guess` and removed the alternating string conversion from `app.py`.
  - Put the debug panel behind a password input that requires `"1234"` and stores the authenticated state in `st.session_state.dev_authenticated`.
  - Incremented `st.session_state.game_id` on new game so the input field key changes, which forces Streamlit to render a fresh empty box; also reset `dev_authenticated` to `False`.
  - Changed `update_score` to always subtract 5 points for any wrong guess instead of rewarding points on even attempts.

## 📸 Demo Walkthrough

The following is a sample game played on Normal difficulty (range 1–100, 8 attempts allowed). The secret number is 42.

1. Launch the app. Run `python -m streamlit run app.py`. The page title reads "🎮 Game Glitch Investigator". The sidebar shows Difficulty: Normal, Range: 1 to 100, Attempts allowed: 8.
2. Start guessing, too high. Type `70` in the guess box and click Submit Guess 🚀. The hint banner reads "📉 Go LOWER!" and the attempts counter drops to 7 remaining.
3. Narrow it down, too low. Type `30` and submit. The hint reads "📈 Go HIGHER!" with 6 attempts left.
4. Close the gap, too high. Type `55` and submit. Hint: "📉 Go LOWER!" with 5 attempts left.
5. Binary-search further, too low. Type `40` and submit. Hint: "📈 Go HIGHER!" with 4 attempts left.
6. One step away, too high. Type `50` and submit. Hint: "📉 Go LOWER!" with 3 attempts left.
7. Correct guess, win! Type `42` and submit. Balloons fill the screen and the success banner reads "You won! The secret was 42. Final score: 60". The Submit button is disabled for the rest of this round.
8. Start a new round. Click New Game 🔁. The input box clears, the attempt counter resets to 8, the debug panel re-locks, and a fresh secret number is generated.

Screenshot (optional): <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
============================= test session starts =============================
platform win32 -- Python 3.14.3, pytest-9.0.3, pluggy-1.6.0
rootdir: C:\Users\joshua\Desktop\ai110-module1show-gameglitchinvestigator-starter-main
plugins: anyio-4.13.0
collected 4 items

tests/test_game_logic.py::test_winning_guess PASSED                      [ 25%]
tests/test_game_logic.py::test_guess_too_high PASSED                     [ 50%]
tests/test_game_logic.py::test_guess_too_low PASSED                      [ 75%]
tests/test_game_logic.py::test_string_inputs_cast_to_int PASSED          [100%]

============================== 4 passed in 0.07s ==============================
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here. A screenshot is optional.]
