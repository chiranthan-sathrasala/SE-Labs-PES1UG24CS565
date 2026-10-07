# Lab 4: VibeCoding — Complete Chat History

**Assignment:** Fix and extend the Hangman game (`16_hangman_SE_Lab`) using an LLM as a coding assistant.
**Repo:** https://github.com/chiranthan-sathrasala/16_hangman_SE_Lab
**LLM used:** Claude (Anthropic)

---

## Turn 1 — User

*(Attached: screenshot of the Lab 4 guidelines. Transcription of the key points: use Vibe Coding tools to fix the broken code and add features, in three to four prompt attempts; individual assignment; 60 minutes. Steps: go to the assigned repo, read the README, clone or fork, run the Python game, record 10 seconds of video before changes, write prompts to fix the broken code, add the README features one by one, record 10 seconds after, push the code, use separate commits for each task, do not raise a PR to the main repo. Deliverables in the Lab-4 folder: before/after videos, updated code, chat history as a doc/pdf.)*

This week Software engineering lab we have been given an assignment on vibe coding.
I've uploaded the screenshot of the guidelines that we have to follow.
This is the github repo link which is assigned to me: https://github.com/chiranthan-sathrasala/16_hangman_SE_Lab

can you help me in completing this assignment?
First as the guidelines tells we have to make a video of the current working, so tell me how do i run this.

## Turn 1 — Claude

*(Claude fetched the repo page and read the README: a terminal Hangman game with four tasks — guess-state correctness, session model, difficulty and scoring, robust input and feedback.)*

Yes, I can help. I read your repo's README. It's a terminal-based Hangman game written in Python (no GUI), so running it is simple.

### How to run it

**1. Clone your repo** (use your own fork, not the SETAPESU26 original):
```bash
git clone https://github.com/chiranthan-sathrasala/16_hangman_SE_Lab.git
cd 16_hangman_SE_Lab
```

**2. Check that Python is installed:**
```bash
python --version
```
You need Python 3. If `python` isn't recognised, try `python3`.

**3. Install dependencies.** There's a `requirements.txt`, but the README doesn't say the game needs anything. Run this to be safe:
```bash
pip install -r requirements.txt
```

**4. Start the game:**
```bash
python main.py
```

### What to do for the "before" video

The README says to reproduce the Task 1 bug before changing anything. Your 10-second video should show it:

- Start a round and pick a category if it asks.
- Guess a **wrong** letter, then guess the **same wrong letter again**. If the bug is present, you lose a second life for it.
- Guess a **correct** letter, then guess it **again**. It may be counted as a new guess.
- If you can, also try the hint command and an invalid input like `1` or `@`. This may show the other issues the later tasks fix.

Screen-record the terminal while you do this. On Windows use Win+G (Xbox Game Bar), on Mac use Cmd+Shift+5, and on Linux use OBS or SimpleScreenRecorder.

### Your four tasks

| Task | What to do |
|---|---|
| 1 | Fix repeated-guess handling so each distinct letter counts once |
| 2 | Track rounds played, rounds won and best streak with `stats.py`, resetting only round state between rounds |
| 3 | Add difficulty levels that change lives and scoring, with hints affecting score consistently |
| 4 | Handle invalid input safely and avoid duplicated feedback |

Before you start prompting, a few points from the README and the lab sheet matter for the submission:

- Make **one commit per task**, as the lab sheet requires.
- Don't add CSV, JSON or SQLite persistence. Keep all state in memory.
- Keep the **whole chat history**, because you submit it. Use a single chat for the whole lab.
- Don't raise a PR to the main repo.

Run the game and tell me what happens, or paste what you see in the terminal. Next I can help you read `game.py` and write the first prompt for Task 1, since the lab wants each task done in 3–4 prompts. If you paste the contents of `game.py`, `stats.py` and `main.py`, I'll find the bug with you.

---

## Turn 2 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: h
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: h
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Correct.

Word: _ a _ a _ a _ _
Wrong: h
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Correct.

Word: _ a _ a b a _ _
Wrong: h
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: c
Wrong.

Word: _ a _ a b a _ _
Wrong: c h
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Correct.

Word: d a _ a b a _ _
Wrong: c h
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: d a _ a b a _ e
Wrong: c h
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: f
Wrong.

Word: d a _ a b a _ e
Wrong: c f h
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: g
Wrong.

Word: d a _ a b a _ e
Wrong: c f g h
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: i
Wrong.

Word: d a _ a b a _ e
Wrong: c f g h i
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: h
Wrong.
Out of lives. The word was: database
Another round? [y/n]: n
Final score: 0  Streak: 0
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

this is what i did.

## Turn 2 — Claude

You reproduced the Task 1 bug. Near the end of your run:

```
Wrong: c f g h i
Lives: 1
Letter: h
Wrong.
Out of lives. The word was: database
```

You had already guessed `h` earlier and it had cost a life. Guessing it again cost your last life, which is exactly what Task 1 says must not happen.

Your run also shows something for Task 2. The ending only prints `Final score: 0  Streak: 0`. There are no rounds played, rounds won or best streak, so the session stats aren't connected yet.

### Recording the "before" video

Your run was about 10 guesses, which is too long for a 10-second clip. Record a shorter one that shows the bug quickly:

1. Run `python main.py` and type `technology`.
2. Type `h`, then `h` again. For this word `h` is wrong, and you should see the lives drop twice (6 → 5 → 4).
3. Type `a`, then `a` again to show the repeated correct letter.
4. Type `/quit`.

Practise once, then record so it fits in about 10–15 seconds. If you can't be sure of the timing, a slightly longer video is better than one that cuts off before the bug shows.

### Next step

To write good prompts for the fix, I need to see the code. Paste these here:

- `game.py`
- `stats.py`
- `main.py`

I'll walk you through where the bug is, and then we can do Task 1 as its own prompt and commit. Before you start fixing, run `git status` to confirm you're on a clean `main` branch.

---

## Turn 3 — User

*(Attached: `main.py`, `stats.py`, `game.py`)*

**main.py**
```python
from game import HangmanGame

if __name__ == "__main__":
    HangmanGame().run()
```

**stats.py**
```python
class SessionStats:
    def __init__(self):
        self.rounds = 0
        self.wins = 0
        self.best_streak = 0

    def record(self, won, streak):
        self.rounds += 1
        self.wins += int(won)
        self.best_streak = max(self.best_streak, streak)
```

**game.py** (original)
```python
import random
from words import WORDS, HINTS


class HangmanGame:
    def __init__(self):
        self.score = 0
        self.streak = 0
        self.category = "technology"
        self.secret = ""
        self.guessed = set()
        self.wrong = set()
        self.lives = 6
        self.hint_used = False

    def start_round(self):
        self.secret = random.choice(WORDS[self.category])
        self.guessed.clear()
        self.wrong.clear()
        self.lives = 6
        self.hint_used = False

    def masked(self):
        return " ".join(ch if ch in self.guessed else "_" for ch in self.secret)

    def won(self):
        return all(ch in self.guessed for ch in set(self.secret))

    def guess(self, letter):
        if len(letter) != 1 or not letter.isalpha():
            return "Enter one letter."
        if letter in self.guessed:
            return "Already guessed."
        if letter in self.secret:
            self.guessed.add(letter)
            return "Correct."
        self.wrong.add(letter)
        self.lives -= 1
        return "Wrong."

    def use_hint(self):
        if self.hint_used:
            return None
        self.hint_used = True
        self.score = max(0, self.score - 1)
        return HINTS.get(self.secret, "No hint available.")

    def play_round(self):
        self.start_round()
        while self.lives > 0 and not self.won():
            print("\nWord:", self.masked())
            print("Wrong:", " ".join(sorted(self.wrong)) or "-")
            print("Lives:", self.lives, "Score:", self.score, "Streak:", self.streak)
            raw = input("Letter, /hint, or /quit: ").strip().lower()
            if raw == "/quit":
                return False
            if raw == "/hint":
                hint = self.use_hint()
                print(hint if hint else "Hint already used.")
                continue
            print(self.guess(raw))

        if self.won():
            self.streak += 1
            self.score += 5 + self.streak
            print("Solved:", self.secret)
            return True

        self.streak = 0
        print("Out of lives. The word was:", self.secret)
        return True

    def run(self):
        print("Hangman Challenge")
        print("A session consists of multiple rounds.")
        while True:
            print("\nCategories:", ", ".join(WORDS))
            raw = input("Choose category or q: ").strip().lower()
            if raw == "q":
                return
            if raw not in WORDS:
                print("Unknown category.")
                continue
            self.category = raw
            if not self.play_round():
                return
            again = input("Another round? [y/n]: ").strip().lower()
            if again != "y":
                print("Final score:", self.score, " Streak:", self.streak)
                return
```

here are the files that you ask for.

## Turn 3 — Claude

I found the Task 1 bug. I've also spotted what the other three tasks need, but let's take them one at a time so each gets its own commit.

### Task 1: the bug

It's in `guess()`:

```python
if letter in self.guessed:
    return "Already guessed."
```

This only checks the **correct** letters. A wrong letter goes into `self.wrong`, which is never checked, so guessing it again costs another life. That's what happened with your second `h`. Correct letters are already protected, because they're in `self.guessed`.

### The fix

Check both sets. In `game.py`, change that line to:

```python
if letter in self.guessed or letter in self.wrong:
    return "Already guessed."
```

That's the only change needed. The full method becomes:

```python
def guess(self, letter):
    if len(letter) != 1 or not letter.isalpha():
        return "Enter one letter."
    if letter in self.guessed or letter in self.wrong:
        return "Already guessed."
    if letter in self.secret:
        self.guessed.add(letter)
        return "Correct."
    self.wrong.add(letter)
    self.lives -= 1
    return "Wrong."
```

### Test it

Run `python main.py`, choose `technology`, then type `h`, `h`, `a`, `a`. You should see:

- `h` the first time: `Wrong.`, lives 6 → 5
- `h` again: `Already guessed.`, lives stay at 5
- `a` the second time: `Already guessed.`

### Commit

```bash
git add game.py
git commit -m "Task 1: Fix repeated wrong-letter guesses consuming extra lives"
```

### What I noticed for the remaining tasks

I'll hold the details until we get there:

- **Task 2:** `SessionStats` in `stats.py` is never imported or used, which is why you saw only `Final score` at the end. Also, `/quit` and `q` exit without showing any summary.
- **Task 3:** lives are hardcoded to 6, and the hint penalty is inconsistent. It subtracts from the running score and `max(0, ...)` clamps it, so a hint is free if your score is 0.
- **Task 4:** the main issues are `y/n` handling (anything other than `y` ends the session, including typos), plus empty and unknown `/commands`.

Apply the Task 1 fix, run the test above and commit. Then tell me, and we'll do Task 2.

If you want to use your own LLM prompting for the lab (the sheet says to get each task done in 3–4 attempts), you can paste my diagnosis into your prompt. Either way, keep the full chat history for submission.

---

## Turn 4 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Wrong.

Word: _ _ _ _ _ _ _
Wrong: a
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Wrong.

Word: _ _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Already guessed.

Word: _ _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Already guessed.

Word: _ _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: c
Wrong.

Word: _ _ _ _ _ _ _
Wrong: a b c
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Wrong.

Word: _ _ _ _ _ _ _
Wrong: a b c d
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Already guessed.

Word: _ _ _ _ _ _ _
Wrong: a b c d
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Already guessed.

Word: _ _ _ _ _ _ _
Wrong: a b c d
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: f
Wrong.

Word: _ _ _ _ _ _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: _ e _ _ _ _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: n
Correct.

Word: n e _ _ _ _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: t
Correct.

Word: n e t _ _ _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: w
Correct.

Word: n e t w _ _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: o
Correct.

Word: n e t w o _ _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: r
Correct.

Word: n e t w o r _
Wrong: a b c d f
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: k
Correct.
Solved: network
Another round? [y/n]: n
Final score: 6  Streak: 1
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

this is how the code ran after the task 1 fix.

now what are the next steps?

## Turn 4 — Claude

Task 1 is fixed. In your run, `a` and `b` were each guessed twice and every repeat said `Already guessed.` while lives stayed put (4 → 4, then 2 → 2). Commit it if you haven't:

```bash
git add game.py
git commit -m "Task 1: Fix repeated wrong-letter guesses consuming extra lives"
```

### Task 2: connect the session stats

`stats.py` already has a working `SessionStats` class, but `game.py` never imports or uses it. You need to do three things:

1. Create a stats object once per session.
2. Record each finished round.
3. Show a summary when the session ends, whether the player exits via `q`, `/quit` or `n`.

`start_round()` already resets only round state (secret, guesses, lives, hint). `score`, `streak` and the stats object live in `__init__`, so they carry over between rounds. You don't need to change `start_round()`.

#### Changes to `game.py`

**1. Add the import at the top:**
```python
from stats import SessionStats
```

**2. In `__init__`, add one line:**
```python
self.stats = SessionStats()
```

**3. Add this method (for example, above `run`):**
```python
def print_summary(self):
    print("\nSession summary")
    print("Rounds played:", self.stats.rounds)
    print("Rounds won:", self.stats.wins)
    print("Best streak:", self.stats.best_streak)
    print("Final score:", self.score)
```

**4. In `play_round`, record the result after a win and after a loss:**
```python
if self.won():
    self.streak += 1
    self.score += 5 + self.streak
    self.stats.record(True, self.streak)
    print("Solved:", self.secret)
    return True

self.streak = 0
self.stats.record(False, self.streak)
print("Out of lives. The word was:", self.secret)
return True
```

The win is recorded after the streak is incremented, so `best_streak` sees the new value. On a loss the streak resets to 0, but `best_streak` keeps its maximum because `record` uses `max()`.

**5. In `run`, call the summary on every exit path:**
```python
if raw == "q":
    self.print_summary()
    return
```
```python
if not self.play_round():
    self.print_summary()
    return
again = input("Another round? [y/n]: ").strip().lower()
if again != "y":
    self.print_summary()
    return
```

This replaces the old `print("Final score:", ...)` line, since the summary now prints the score.

A round abandoned with `/quit` isn't counted as played or won, because the round never finished.

#### How to test

Run a session of three rounds: **win, win, lose**, then answer `n`. You should see:

```
Rounds played: 3
Rounds won: 2
Best streak: 2
```

Also check that:
- The streak shown during the lost round's next round starts from 0.
- `/quit` mid-round still prints the summary.
- Typing `q` at the category prompt prints the summary, with rounds at 0 if you quit immediately.

To lose quickly, guess wrong letters like `q`, `z`, `x`, `j`, `v`, `k`. Some of these may be in the word, so adjust as needed.

#### Commit

```bash
git add game.py
git commit -m "Task 2: Integrate SessionStats and add session summary"
```

Run it, paste your output, and then we'll move on to Task 3 (difficulty and scoring).

---

## Turn 5 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Traceback (most recent call last):
  File "D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab\main.py", line 4, in <module>
    HangmanGame().run()
    ^^^^^^^^^^^^^^^^^
AttributeError: 'HangmanGame' object has no attribute 'run'
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

I got some error while running.

## Turn 5 — Claude

That error means `run` is no longer inside the `HangmanGame` class. Python only finds methods that are indented under the class, so a method pasted at the wrong indentation ends the class early.

The most likely cause is the new `print_summary` method. If it was pasted with no indentation (or less than the other methods), the class ends there and `run()` below it becomes a plain function outside the class.

Every method in the class should start with exactly 4 spaces:

```python
class HangmanGame:
    def __init__(self):      # 4 spaces
        ...                  # 8 spaces

    def print_summary(self): # 4 spaces, not 0
        ...
```

To avoid hunting for it, here is the complete `game.py` with Task 2 applied. Replace the whole file with this:

```python
import random
from words import WORDS, HINTS
from stats import SessionStats


class HangmanGame:
    def __init__(self):
        self.score = 0
        self.streak = 0
        self.category = "technology"
        self.secret = ""
        self.guessed = set()
        self.wrong = set()
        self.lives = 6
        self.hint_used = False
        self.stats = SessionStats()

    def start_round(self):
        self.secret = random.choice(WORDS[self.category])
        self.guessed.clear()
        self.wrong.clear()
        self.lives = 6
        self.hint_used = False

    def masked(self):
        return " ".join(ch if ch in self.guessed else "_" for ch in self.secret)

    def won(self):
        return all(ch in self.guessed for ch in set(self.secret))

    def guess(self, letter):
        if len(letter) != 1 or not letter.isalpha():
            return "Enter one letter."
        if letter in self.guessed or letter in self.wrong:
            return "Already guessed."
        if letter in self.secret:
            self.guessed.add(letter)
            return "Correct."
        self.wrong.add(letter)
        self.lives -= 1
        return "Wrong."

    def use_hint(self):
        if self.hint_used:
            return None
        self.hint_used = True
        self.score = max(0, self.score - 1)
        return HINTS.get(self.secret, "No hint available.")

    def print_summary(self):
        print("\nSession summary")
        print("Rounds played:", self.stats.rounds)
        print("Rounds won:", self.stats.wins)
        print("Best streak:", self.stats.best_streak)
        print("Final score:", self.score)

    def play_round(self):
        self.start_round()
        while self.lives > 0 and not self.won():
            print("\nWord:", self.masked())
            print("Wrong:", " ".join(sorted(self.wrong)) or "-")
            print("Lives:", self.lives, "Score:", self.score, "Streak:", self.streak)
            raw = input("Letter, /hint, or /quit: ").strip().lower()
            if raw == "/quit":
                return False
            if raw == "/hint":
                hint = self.use_hint()
                print(hint if hint else "Hint already used.")
                continue
            print(self.guess(raw))

        if self.won():
            self.streak += 1
            self.score += 5 + self.streak
            self.stats.record(True, self.streak)
            print("Solved:", self.secret)
            return True

        self.streak = 0
        self.stats.record(False, self.streak)
        print("Out of lives. The word was:", self.secret)
        return True

    def run(self):
        print("Hangman Challenge")
        print("A session consists of multiple rounds.")
        while True:
            print("\nCategories:", ", ".join(WORDS))
            raw = input("Choose category or q: ").strip().lower()
            if raw == "q":
                self.print_summary()
                return
            if raw not in WORDS:
                print("Unknown category.")
                continue
            self.category = raw
            if not self.play_round():
                self.print_summary()
                return
            again = input("Another round? [y/n]: ").strip().lower()
            if again != "y":
                self.print_summary()
                return
```

Then test again with `python main.py`: play a few rounds, answer `y` between them, and finish with `n`. You should see the session summary at the end. If another error appears, paste it here.

Once it works, commit Task 2:

```bash
git add game.py
git commit -m "Task 2: Integrate SessionStats and add session summary"
```

If you had already committed Task 1 separately, the diff for this commit will include only the Task 2 changes, which is what the lab wants.

---

## Turn 6 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: n
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: n
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: d n
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: a d n
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: f
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: a d f n
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: _ _ _ _ _ _ e _
Wrong: a d f n
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Wrong.

Word: _ _ _ _ _ _ e _
Wrong: a b d f n
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Already guessed.

Word: _ _ _ _ _ _ e _
Wrong: a b d f n
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: g
Wrong.
Out of lives. The word was: compiler
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: n
Wrong.

Word: _ _ _ _ _ _ _ _ _
Wrong: n
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Wrong.

Word: _ _ _ _ _ _ _ _ _
Wrong: d n
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: z
Wrong.

Word: _ _ _ _ _ _ _ _ _
Wrong: d n z
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit:
Enter one letter.

Word: _ _ _ _ _ _ _ _ _
Wrong: d n z
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Wrong.

Word: _ _ _ _ _ _ _ _ _
Wrong: d e n z
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Already guessed.

Word: _ _ _ _ _ _ _ _ _
Wrong: d e n z
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: f
Wrong.

Word: _ _ _ _ _ _ _ _ _
Wrong: d e f n z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Correct.

Word: a _ _ _ _ _ _ _ _
Wrong: d e f n z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Wrong.
Out of lives. The word was: algorithm
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: d
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: n
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: d n
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: a d n
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: m
Correct.

Word: _ _ m _ _ _ _ _
Wrong: a d n
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: o
Correct.

Word: _ o m _ _ _ _ _
Wrong: a d n
Lives: 3 Score: 0 Streak: 0
Letter, /hint, or /quit: z
Wrong.

Word: _ o m _ _ _ _ _
Wrong: a d n z
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: p
Correct.

Word: _ o m p _ _ _ _
Wrong: a d n z
Lives: 2 Score: 0 Streak: 0
Letter, /hint, or /quit: q
Wrong.

Word: _ o m p _ _ _ _
Wrong: a d n q z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: c
Correct.

Word: c o m p _ _ _ _
Wrong: a d n q z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: i
Correct.

Word: c o m p i _ _ _
Wrong: a d n q z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: l
Correct.

Word: c o m p i l _ _
Wrong: a d n q z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: c o m p i l e _
Wrong: a d n q z
Lives: 1 Score: 0 Streak: 0
Letter, /hint, or /quit: r
Correct.
Solved: compiler
Another round? [y/n]: n

Session summary
Rounds played: 3
Rounds won: 1
Best streak: 1
Final score: 6
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: a
Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Wrong.

Word: _ _ _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: c
Correct.

Word: c _ _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: o
Correct.

Word: c o _ _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: m
Correct.

Word: c o m _ _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: p
Correct.

Word: c o m p _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: o
Already guessed.

Word: c o m p _ _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: i
Correct.

Word: c o m p i _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit:
Enter one letter.

Word: c o m p i _ _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: l
Correct.

Word: c o m p i l _ _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: c o m p i l e _
Wrong: a b
Lives: 4 Score: 0 Streak: 0
Letter, /hint, or /quit: r
Correct.
Solved: compiler
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: technology

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 6 Streak: 1
Letter, /hint, or /quit: /hint
Stores structured information.

Word: _ _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: d
Correct.

Word: d _ _ _ _ _ _ _
Wrong: -
Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: t
Correct.

Word: d _ t _ _ _ _ _
Wrong: -
Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: a
Correct.

Word: d a t a _ a _ _
Wrong: -
Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: /quit

Session summary
Rounds played: 1
Rounds won: 1
Best streak: 1
Final score: 5
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

this is the output.

## Turn 6 — Claude

Task 2 works. Checking your output against the code:

- **Run 1:** you played 3 rounds, lost 2 and won 1. The summary says played 3, won 1, best streak 1, and the score of 6 matches one win at streak 1 (5 + 1).
- **Run 2:** you won round 1, then used a hint in round 2 (score 6 → 5, streak carried over) and quit. The summary shows only 1 round played, since the abandoned round isn't counted.
- **Round resets:** lives went back to 6 and wrong letters cleared on each new round, while score and streak carried over.

Commit it if you haven't:
```bash
git add game.py
git commit -m "Task 2: Integrate SessionStats and add session summary"
```

### Task 3: difficulty and scoring

The current code has two problems for this task: lives are hardcoded to 6, and the hint penalty is subtracted from the running score and clamped at 0. That makes a hint free when your score is 0.

My design:

| Difficulty | Lives | Score multiplier | Hint cost |
|---|---|---|---|
| easy | 8 | x1 | 1 |
| medium | 6 | x2 | 2 |
| hard | 4 | x3 | 3 |

- Round points on a win are `(5 + streak) * multiplier`, minus the hint cost if a hint was used (never below 0).
- The hint penalty is applied the same way every time, when the round ends, so it no longer depends on your current total.
- Difficulty is chosen each round after the category, so category selection still works as before.
- Lives, hint state and guesses are reset in `start_round()`, so nothing leaks between rounds.

Here is the full `game.py` with Tasks 1–3 applied. Replace the whole file again so there are no indentation problems:

```python
import random
from words import WORDS, HINTS
from stats import SessionStats

DIFFICULTIES = {
    "easy": {"lives": 8, "multiplier": 1, "hint_cost": 1},
    "medium": {"lives": 6, "multiplier": 2, "hint_cost": 2},
    "hard": {"lives": 4, "multiplier": 3, "hint_cost": 3},
}


class HangmanGame:
    def __init__(self):
        self.score = 0
        self.streak = 0
        self.category = "technology"
        self.difficulty = "medium"
        self.secret = ""
        self.guessed = set()
        self.wrong = set()
        self.lives = DIFFICULTIES[self.difficulty]["lives"]
        self.hint_used = False
        self.stats = SessionStats()

    def start_round(self):
        self.secret = random.choice(WORDS[self.category])
        self.guessed.clear()
        self.wrong.clear()
        self.lives = DIFFICULTIES[self.difficulty]["lives"]
        self.hint_used = False

    def masked(self):
        return " ".join(ch if ch in self.guessed else "_" for ch in self.secret)

    def won(self):
        return all(ch in self.guessed for ch in set(self.secret))

    def guess(self, letter):
        if len(letter) != 1 or not letter.isalpha():
            return "Enter one letter."
        if letter in self.guessed or letter in self.wrong:
            return "Already guessed."
        if letter in self.secret:
            self.guessed.add(letter)
            return "Correct."
        self.wrong.add(letter)
        self.lives -= 1
        return "Wrong."

    def use_hint(self):
        if self.hint_used:
            return None
        self.hint_used = True
        return HINTS.get(self.secret, "No hint available.")

    def round_points(self):
        rules = DIFFICULTIES[self.difficulty]
        points = (5 + self.streak) * rules["multiplier"]
        if self.hint_used:
            points -= rules["hint_cost"]
        return max(0, points)

    def print_summary(self):
        print("\nSession summary")
        print("Rounds played:", self.stats.rounds)
        print("Rounds won:", self.stats.wins)
        print("Best streak:", self.stats.best_streak)
        print("Final score:", self.score)

    def play_round(self):
        self.start_round()
        while self.lives > 0 and not self.won():
            print("\nWord:", self.masked())
            print("Wrong:", " ".join(sorted(self.wrong)) or "-")
            print("Difficulty:", self.difficulty, "Lives:", self.lives,
                  "Score:", self.score, "Streak:", self.streak)
            raw = input("Letter, /hint, or /quit: ").strip().lower()
            if raw == "/quit":
                return False
            if raw == "/hint":
                hint = self.use_hint()
                print(hint if hint else "Hint already used.")
                continue
            print(self.guess(raw))

        if self.won():
            self.streak += 1
            points = self.round_points()
            self.score += points
            self.stats.record(True, self.streak)
            print("Solved:", self.secret, "(+" + str(points), "points)")
            return True

        self.streak = 0
        self.stats.record(False, self.streak)
        print("Out of lives. The word was:", self.secret)
        return True

    def choose_difficulty(self):
        while True:
            print("Difficulties:", ", ".join(DIFFICULTIES))
            raw = input("Choose difficulty: ").strip().lower()
            if raw in DIFFICULTIES:
                self.difficulty = raw
                return
            print("Unknown difficulty.")

    def run(self):
        print("Hangman Challenge")
        print("A session consists of multiple rounds.")
        while True:
            print("\nCategories:", ", ".join(WORDS))
            raw = input("Choose category or q: ").strip().lower()
            if raw == "q":
                self.print_summary()
                return
            if raw not in WORDS:
                print("Unknown category.")
                continue
            self.category = raw
            self.choose_difficulty()
            if not self.play_round():
                self.print_summary()
                return
            again = input("Another round? [y/n]: ").strip().lower()
            if again != "y":
                self.print_summary()
                return
```

There's one subtle change. Previously `self.streak += 1` happened first and then `5 + self.streak` was added. I kept that order, so `round_points()` sees the updated streak and a first win scores `(5 + 1) * multiplier`.

#### Tests to run

| Test | Expected result |
|---|---|
| Round on `easy` | Lives start at 8 |
| Next round on `hard` | Lives start at 4, with no leftover guesses |
| Win a first round on `medium` (no hint) | +12 points |
| Win a first round on `hard` (no hint) | +18 points |
| Win a first round on `easy` (no hint) | +6 points |
| Use `/hint` and win on `medium` | +10 points (12 − 2) |
| Use `/hint` at score 0 | Score doesn't change until the round ends, and the hint cost shows in the points for that round |
| Type a bad difficulty like `extreme` | `Unknown difficulty.`, then it asks again |
| Use a hint in one round, then start the next | `/hint` works again in the new round |

The points also appear in the message after a win (for example `Solved: compiler (+10 points)`), which makes the scoring easy to check on screen.

#### Commit

```bash
git add game.py
git commit -m "Task 3: Add difficulty levels and consistent hint scoring"
```

Run it, then paste your output and I'll check it. After that, Task 4 covers robust input.

---

## Turn 7 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: easy

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: /hint
Stores structured information.

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Correct.

Word: d _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Correct.

Word: d a _ a _ a _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: t
Correct.

Word: d a t a _ a _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Already guessed.

Word: d a t a _ a _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: z
Wrong.

Word: d a t a _ a _ _
Wrong: z
Difficulty: easy Lives: 7 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct.

Word: d a t a _ a _ e
Wrong: z
Difficulty: easy Lives: 7 Score: 0 Streak: 0
Letter, /hint, or /quit: s
Correct.

Word: d a t a _ a s e
Wrong: z
Difficulty: easy Lives: 7 Score: 0 Streak: 0
Letter, /hint, or /quit: x
Wrong.

Word: d a t a _ a s e
Wrong: x z
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: c
Wrong.

Word: d a t a _ a s e
Wrong: c x z
Difficulty: easy Lives: 5 Score: 0 Streak: 0
Letter, /hint, or /quit: b
Correct.
Solved: database (+5 points)
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: medium

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: /hint
Turns source code into another form.

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: c
Correct.

Word: c _ _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: o
Correct.

Word: c o _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: m
Correct.

Word: c o m _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: p
Correct.

Word: c o m p _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: l
Correct.

Word: c o m p _ l _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: o
Already guessed.

Word: c o m p _ l _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: i
Correct.

Word: c o m p i l _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: e
Correct.

Word: c o m p i l e _
Wrong: -
Difficulty: medium Lives: 6 Score: 5 Streak: 1
Letter, /hint, or /quit: x
Wrong.

Word: c o m p i l e _
Wrong: x
Difficulty: medium Lives: 5 Score: 5 Streak: 1
Letter, /hint, or /quit: a
Wrong.

Word: c o m p i l e _
Wrong: a x
Difficulty: medium Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: r
Correct.
Solved: compiler (+12 points)
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: hard

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: /hint
Turns source code into another form.

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: c
Correct.

Word: c o _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: o
Correct.

Word: c o m _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: m
Correct.

Word: c o m p _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: p
Correct.

Word: c o m p _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: o
Already guessed.

Word: c o m p _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: i
Correct.

Word: c o m p i _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: l
Correct.

Word: c o m p i l _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: e
Correct.

Word: c o m p i l e _
Wrong: -
Difficulty: hard Lives: 4 Score: 17 Streak: 2
Letter, /hint, or /quit: r
Correct.
Solved: compiler (+21 points)
Another round? [y/n]: n

Session summary
Rounds played: 3
Rounds won: 3
Best streak: 3
Final score: 38
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

this is the output.

## Turn 7 — Claude

Task 3 works. I checked your numbers against the formula `(5 + streak) * multiplier - hint cost`:

| Round | Difficulty | Calculation | Shown | Score after |
|---|---|---|---|---|
| 1 | easy, hint | (5+1)×1 − 1 | +5 ✓ | 5 |
| 2 | medium, hint | (5+2)×2 − 2 | +12 ✓ | 17 ✓ |
| 3 | hard, hint | (5+3)×3 − 3 | +21 ✓ | 38 ✓ |

Lives started at 8, 6 and 4 for the three difficulties, and the hint was available again in every new round. Repeated letters like `a` and `o` didn't cost lives. Commit it:

```bash
git add game.py
git commit -m "Task 3: Add difficulty levels and consistent hint scoring"
```

You didn't test a **loss on hard** or an **invalid difficulty** (like `extreme`). Try both quickly before the final video, since the README asks for edge-case testing.

### Task 4: robust input and feedback

These are the remaining problems in the code:

- Typing anything other than `y` at "Another round?" ends the session, so a typo like `yes` or `Y ` quits the game. It should accept `y`/`yes` and `n`/`no` and ask again otherwise.
- An unknown command like `/foo` gets "Enter one letter.", which is misleading. It needs its own message.
- Empty input, multiple letters and symbols all get the same generic message. Each should say what was wrong.
- `isalpha()` also accepts non-ASCII letters such as `é`, which can never be in the word and would cost a life. Restrict guesses to a–z.
- If a word had no hint, `/hint` would still mark the hint as used and charge the player. Failed hints shouldn't be charged.

Invalid input already returns before touching any state, which is what "never changes game state" needs. The changes below only make the messages clearer and cover the gaps above.

Replace `game.py` with the full file below:

```python
import random
import string
from words import WORDS, HINTS
from stats import SessionStats

DIFFICULTIES = {
    "easy": {"lives": 8, "multiplier": 1, "hint_cost": 1},
    "medium": {"lives": 6, "multiplier": 2, "hint_cost": 2},
    "hard": {"lives": 4, "multiplier": 3, "hint_cost": 3},
}


class HangmanGame:
    def __init__(self):
        self.score = 0
        self.streak = 0
        self.category = "technology"
        self.difficulty = "medium"
        self.secret = ""
        self.guessed = set()
        self.wrong = set()
        self.lives = DIFFICULTIES[self.difficulty]["lives"]
        self.hint_used = False
        self.stats = SessionStats()

    def start_round(self):
        self.secret = random.choice(WORDS[self.category])
        self.guessed.clear()
        self.wrong.clear()
        self.lives = DIFFICULTIES[self.difficulty]["lives"]
        self.hint_used = False

    def masked(self):
        return " ".join(ch if ch in self.guessed else "_" for ch in self.secret)

    def won(self):
        return all(ch in self.guessed for ch in set(self.secret))

    def guess(self, letter):
        letter = letter.strip().lower()
        if len(letter) != 1:
            return "Enter one letter at a time."
        if letter not in string.ascii_lowercase:
            return "Only letters a-z are allowed."
        if letter in self.guessed or letter in self.wrong:
            return "Already guessed '" + letter + "'."
        if letter in self.secret:
            self.guessed.add(letter)
            return "Correct: '" + letter + "' is in the word."
        self.wrong.add(letter)
        self.lives -= 1
        return "Wrong: '" + letter + "' is not in the word."

    def use_hint(self):
        if self.hint_used:
            return "Hint already used this round."
        hint = HINTS.get(self.secret)
        if hint is None:
            return "No hint available for this word."
        self.hint_used = True
        return "Hint: " + hint

    def round_points(self):
        rules = DIFFICULTIES[self.difficulty]
        points = (5 + self.streak) * rules["multiplier"]
        if self.hint_used:
            points -= rules["hint_cost"]
        return max(0, points)

    def print_summary(self):
        print("\nSession summary")
        print("Rounds played:", self.stats.rounds)
        print("Rounds won:", self.stats.wins)
        print("Best streak:", self.stats.best_streak)
        print("Final score:", self.score)

    def play_round(self):
        self.start_round()
        while self.lives > 0 and not self.won():
            print("\nWord:", self.masked())
            print("Wrong:", " ".join(sorted(self.wrong)) or "-")
            print("Difficulty:", self.difficulty, "Lives:", self.lives,
                  "Score:", self.score, "Streak:", self.streak)
            raw = input("Letter, /hint, or /quit: ").strip().lower()
            if raw == "/quit":
                return False
            if raw == "/hint":
                print(self.use_hint())
                continue
            if raw == "":
                print("Please enter a letter, /hint, or /quit.")
                continue
            if raw.startswith("/"):
                print("Unknown command. Use /hint or /quit.")
                continue
            print(self.guess(raw))

        if self.won():
            self.streak += 1
            points = self.round_points()
            self.score += points
            self.stats.record(True, self.streak)
            print("Solved:", self.secret, "(+" + str(points), "points)")
            return True

        self.streak = 0
        self.stats.record(False, self.streak)
        print("Out of lives. The word was:", self.secret)
        return True

    def choose_difficulty(self):
        while True:
            print("Difficulties:", ", ".join(DIFFICULTIES))
            raw = input("Choose difficulty: ").strip().lower()
            if raw in DIFFICULTIES:
                self.difficulty = raw
                return
            print("Unknown difficulty.")

    def ask_yes_no(self, prompt):
        while True:
            raw = input(prompt).strip().lower()
            if raw in ("y", "yes"):
                return True
            if raw in ("n", "no"):
                return False
            print("Please answer y or n.")

    def run(self):
        print("Hangman Challenge")
        print("A session consists of multiple rounds.")
        while True:
            print("\nCategories:", ", ".join(WORDS))
            raw = input("Choose category or q: ").strip().lower()
            if raw == "q":
                self.print_summary()
                return
            if raw not in WORDS:
                print("Unknown category.")
                continue
            self.category = raw
            self.choose_difficulty()
            if not self.play_round():
                self.print_summary()
                return
            if not self.ask_yes_no("Another round? [y/n]: "):
                self.print_summary()
                return
```

Each action now produces exactly one message that names the letter, for example `Wrong: 'z' is not in the word.` In `use_hint`, the hint is only marked used (and charged) when one is actually shown.

#### Tests to run

| Input | Expected result |
|---|---|
| `/hint`, then `/hint` again | Hint once, then "Hint already used this round."; hint cost applied only once |
| empty Enter | "Please enter a letter, /hint, or /quit." |
| `ab` | "Enter one letter at a time." |
| `5` or `@` | "Only letters a-z are allowed." |
| `é` | "Only letters a-z are allowed." and lives unchanged |
| `/foo` | "Unknown command." and nothing else changes |
| repeated wrong letter | "Already guessed", lives unchanged |
| `maybe` at "Another round?" | "Please answer y or n." and asks again |
| `YES` at "Another round?" | starts a new round |
| `extreme` at difficulty | "Unknown difficulty." and asks again |
| `/quit` mid-round | summary prints and exits |

For each invalid input, check that **lives, wrong letters and score are the same as before**.

#### Commit

```bash
git add game.py
git commit -m "Task 4: Add robust input validation and clearer feedback"
```

### After Task 4

Once your test output looks right, the remaining steps from the lab sheet are:

1. Record the **after** video (about 10 seconds). Show the repeated-letter fix, a difficulty choice, a hint, and one invalid input.
2. Create a `Lab-4` folder in your repo and put in the two videos, the updated code, and your chat history exported as PDF or doc.
3. Push to **your own** repo with `git push origin main`, and don't open a PR to SETAPESU26.

Paste your Task 4 test output and I'll check it with you.

---

## Turn 8 — User

```text
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab> python main.py
Hangman Challenge
A session consists of multiple rounds.

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: easy

Word: _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: /hint
Hint: A connected collection of devices or systems.

Word: _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: /hint
Hint already used this round.

Word: _ _ _ _ _ _ _
Wrong: -
Difficulty: easy Lives: 8 Score: 0 Streak: 0
Letter, /hint, or /quit: d
Wrong: 'd' is not in the word.

Word: _ _ _ _ _ _ _
Wrong: d
Difficulty: easy Lives: 7 Score: 0 Streak: 0
Letter, /hint, or /quit: a
Wrong: 'a' is not in the word.

Word: _ _ _ _ _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: t
Correct: 't' is in the word.

Word: _ _ t _ _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: n
Correct: 'n' is in the word.

Word: n _ t _ _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: e
Correct: 'e' is in the word.

Word: n e t _ _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: w
Correct: 'w' is in the word.

Word: n e t w _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: 1
Only letters a-z are allowed.

Word: n e t w _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: 2
Only letters a-z are allowed.

Word: n e t w _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit:
Please enter a letter, /hint, or /quit.

Word: n e t w _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: /hello
Unknown command. Use /hint or /quit.

Word: n e t w _ _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: o
Correct: 'o' is in the word.

Word: n e t w o _ _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: r
Correct: 'r' is in the word.

Word: n e t w o r _
Wrong: a d
Difficulty: easy Lives: 6 Score: 0 Streak: 0
Letter, /hint, or /quit: k
Correct: 'k' is in the word.
Solved: network (+5 points)
Another round? [y/n]: yes

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: har
Unknown difficulty.
Difficulties: easy, medium, hard
Choose difficulty: head
Unknown difficulty.
Difficulties: easy, medium, hard
Choose difficulty: hard

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: /hint
Hint: Turns source code into another form.

Word: _ _ _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: c
Correct: 'c' is in the word.

Word: c _ _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: o
Correct: 'o' is in the word.

Word: c o _ _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: m
Correct: 'm' is in the word.

Word: c o m _ _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: p
Correct: 'p' is in the word.

Word: c o m p _ _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: i
Correct: 'i' is in the word.

Word: c o m p i _ _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: l
Correct: 'l' is in the word.

Word: c o m p i l _ _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: e
Correct: 'e' is in the word.

Word: c o m p i l e _
Wrong: -
Difficulty: hard Lives: 4 Score: 5 Streak: 1
Letter, /hint, or /quit: r
Correct: 'r' is in the word.
Solved: compiler (+18 points)
Another round? [y/n]: y

Categories: technology, science, culture
Choose category or q: techonology
Unknown category.

Categories: technology, science, culture
Choose category or q: technology
Difficulties: easy, medium, hard
Choose difficulty: medium

Word: _ _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 23 Streak: 2
Letter, /hint, or /quit: /hint
Hint: A connected collection of devices or systems.

Word: _ _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 23 Streak: 2
Letter, /hint, or /quit: n
Correct: 'n' is in the word.

Word: n _ _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 23 Streak: 2
Letter, /hint, or /quit: e
Correct: 'e' is in the word.

Word: n e _ _ _ _ _
Wrong: -
Difficulty: medium Lives: 6 Score: 23 Streak: 2
Letter, /hint, or /quit: /quit

Session summary
Rounds played: 2
Rounds won: 2
Best streak: 2
Final score: 23
(myenv) PS D:\chiranthan\Semester_5\Software Engineering\16_hangman_SE_Lab>
```

this is the output.

## Turn 8 — Claude

Task 4 works. Checking your output:

- **Hints:** the first `/hint` showed the hint and the second said "Hint already used this round." In the hard round, the points were (5+2)×3 − 3 = **18**, so the cost was charged once. Score 5 + 18 = 23, then 23 carried into the next round.
- **Invalid input:** `1`, `2`, an empty Enter and `/hello` each gave a specific message, and lives stayed at 6 and wrong letters stayed `a d` throughout.
- **Feedback:** each action printed one message naming the letter, with no duplicates.
- **Prompts:** `yes` was accepted at "Another round?", `har` and `head` re-asked for difficulty, `techonology` re-asked for category, and `/quit` printed the summary with 2 rounds played and won.

Commit Task 4:

```bash
git add game.py
git commit -m "Task 4: Add robust input validation and clearer feedback"
```

### Optional extra checks

A few README cases weren't in your run: a **lost round followed by a new round** (streak should reset to 0), a **multi-letter input** like `ab`, a non-ASCII letter like `é`, and an invalid answer like `maybe` at "Another round?". You can try these once and fold them into the after video if you want.

### Remaining steps

**1. Record the "after" video (about 10 seconds).** Show these in order:
- pick a difficulty
- type a wrong letter twice (second one says "Already guessed", lives unchanged)
- use `/hint`
- type one invalid input (like `5`)
- finish with `/quit` so the session summary shows

Practise it once first so it fits in the time.

**2. Check your repo state.**
```bash
git status
git log --oneline
```
You should see four commits, one per task, on top of the original ones. Make sure the venv folder (`myenv`) isn't being added if it sits inside the repo.

**3. Add the Lab-4 folder.** Create `Lab-4` in the repo and put in:
- the before and after videos
- the updated code (`game.py` and the other files, or a copy of the project)
- your chat history as a PDF or doc

```bash
git add Lab-4
git commit -m "Add Lab-4 deliverables"
git push origin main
```

**4. Export the chat history.** In the Claude chat, use the share option to get a link for the README's "chat link" requirement. You can also print the conversation to PDF from your browser (Ctrl+P → Save as PDF). Do this after you've finished chatting, so it contains the complete history.

Don't open a PR to SETAPESU26, as the lab sheet says. Pushing to your own fork's `main` is all you need.

---

## Turn 9 — User

Can you put this whole conversation in a markdown file?

## Turn 9 — Claude

*(This file, `chat_history.md`, was created in response to this request.)*
