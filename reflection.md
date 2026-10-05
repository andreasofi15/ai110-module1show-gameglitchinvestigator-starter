# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

### 1. What was broken when you started?

- When I first ran the game, it was unplayable: the hints pointed the wrong way and seemed to lie on every other guess. **[yours: add what you first saw, like how the page looked or what you tried]**
- Bug 1: The "Higher/Lower" hints were backwards. With secret 50 and guess 60, it said "Go HIGHER!" instead of "Go LOWER!".
- Bug 2: Every even-numbered attempt gave a wrong hint because the secret was converted to a string and compared as text.
- Bug 3: A wrong "Too High" guess on an even attempt raised my score by 5 instead of lowering it.
- Bug 4: After winning or losing, New Game left me stuck on "Game over" because `status` was never reset.

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| Secret 50, guess 60 | "Go LOWER!" | Said "Go HIGHER!" (messages swapped) | None, the game just showed the wrong hint |
| Any guess on an even-numbered attempt | Correct hint every time | Wrong hint, because the secret was converted to a string and compared as text | None shown, the `TypeError` was caught silently in `check_guess` |
| Wrong "Too High" guess on an even-numbered attempt | Score goes down | Score went up by 5 | None |
| Click New Game after winning or losing | A fresh game | Still showed "Game over" because `status` was never reset | None |

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion you did not accept as written (including what the AI suggested, why you rejected or changed it, and how you verified your version). It does not have to be a suggestion that was wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.
---- 

- **Tools used:** Claude in chat. **[yours: add any other tools you used, like ChatGPT or Copilot]**
- **Correct suggestion:** Claude pointed out that the secret was being converted to a string on even attempts inside `if submit:`, which broke the comparison. I removed that if/else so the real int is always passed to `check_guess`. I verified it by playing with Developer Debug Info open (the secret stayed the same and the hints were correct) and by running pytest.
- **Suggestion I did not accept as written:** Claude suggested making Hard a bigger range (like 1 to 200) because 1 to 50 is easier than Normal. I kept the original ranges since that wasn't one of the assigned bugs. **[yours: only keep this if it's true. Otherwise, describe a suggestion you changed or skipped, and how you checked your version]**
---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---
- I decided a bug was fixed when I could reproduce the wrong behavior before the change and no longer see it after.
- Example: with secret 50, guessing 30 now says go higher, and the secret stays the same across guesses.
- I ran pytest with tests for `check_guess` (win, too high, too low).
- The tests first failed because they compared the whole result to a string, but `check_guess` returns a tuple of (outcome, message). That showed me the function worked and the tests needed to unpack the tuple.
- Claude helped me read the pytest failure and update the tests.
-----

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
---- 
- Streamlit reruns the whole script from top to bottom every time you click a button or type something, so normal variables get reset.
- `st.session_state` works like a notebook that survives those reruns, so things like the secret number, score, and attempts get stored there.
- My secret was already stored in session state, so the real cause of the "changing secret" symptom was the string conversion on even attempts. The symptom can point to a different cause than you'd expect.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
--- 
- **Habit to reuse:** Running `python -m pytest` after each change, and checking the file on disk with `cat` when results didn't match my edits. **[yours: or name another habit, like committing after each phase]**
- **What I'd do differently:** Read AI-written code before running it, and paste only what's inside the code box. I once pasted a label into `app.py` and caused a syntax error.
- **How this changed my thinking:** AI-generated code can look complete and still hide bugs, like the swapped hints and the string conversion, so I need to test it instead of trusting it. **[yours: rewrite this in your own words]**