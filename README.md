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


- [x] **Game's purpose:** Game Glitch Investigator is a number guessing game built with Streamlit. The player picks a difficulty, guesses the secret number within a limited number of attempts, and gets "Higher" or "Lower" hints and a score. The AI-generated starter code was full of bugs, and the project is about finding and fixing them.

- [x] **Bugs I found:**
  - The Higher/Lower hint messages were backwards.
  - On every even-numbered attempt, the secret was converted to a string, so the comparison was done as text and the hint was wrong.
  - A wrong "Too High" guess on an even attempt raised the score by 5 instead of lowering it, and the win formula used `attempt_number + 1`.
  - New Game didn't reset `status`, `score`, or `history`, so I stayed stuck on "Game over."
  - `attempts` started at 1 instead of 0, so "Attempts left" was off by one.
  - The info box always said "between 1 and 100," no matter the difficulty.

- [x] **Fixes I applied:**
  - Swapped the hint messages and removed the unneeded `try/except TypeError` fallback in `check_guess`.
  - Removed the string conversion so `check_guess` always receives the secret as an int.
  - Made wrong guesses always lose 5 points and fixed the win formula to `100 - 10 * attempt_number`.
  - Updated New Game to reset the secret, attempts, score, status, and history, using the difficulty's `low` and `high` range.
  - Started `attempts` at 0 and changed the info box to use `{low}` and `{high}`.
  - Refactored the game logic into `logic_utils.py` and added pytest tests for `check_guess` (all passing).

## 📸 Demo Walkthrough

Describe your fixed game in numbered steps so a reader can follow along without watching a video:

Sample game on Normal difficulty (range 1 to 100, 8 attempts). The secret number is 55, which I could see in Developer Debug Info.

1. The game starts with score 0 and "Attempts left: 8".
2. I enter a guess of 40. The game shows "📈 Go HIGHER!" (outcome: Too Low), the score drops to -5, and attempts left goes to 7.
3. I enter a guess of 70. The game shows "📉 Go LOWER!" (outcome: Too High), the score drops to -10, and attempts left goes to 6.
4. The secret number in Developer Debug Info stays at 55 after both guesses, so the state bug is fixed.
5. I enter a guess of 55. The game shows "🎉 Correct!", balloons appear, and the win adds 70 points (100 - 10 × 3), for a final score of 60.
6. I try to keep guessing, and the game says "You already won. Start a new game to play again."
7. I click New Game, and the score, attempts, history, and status all reset, so I can play again.

**Screenshot** *(optional)*: <!-- Insert a screenshot of your fixed, winning game here -->

## 🧪 Test Results

```
python -m pytest -v

collected 3 items

tests/test_game_logic.py::test_winning_guess PASSED
tests/test_game_logic.py::test_guess_too_high PASSED
tests/test_game_logic.py::test_guess_too_low PASSED

=================================== 3 passed in 0.03s ===================================
```

All 3 tests pass. They check that `check_guess` returns "Win", "Too High", and "Too Low" for the right inputs.
```

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, describe the Enhanced UI changes here — a screenshot is optional]
