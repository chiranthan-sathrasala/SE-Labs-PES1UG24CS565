# Lab 5: Static Code Analysis - Animal Shelter Adoption Register

**Name:** Chiranthan Sathrasala

**SRN:** PES1UG24CS565

**Assigned repository:** https://github.com/SETAPESU26/L5_16_pet_shelter

**File analysed:** `pet_shelter.py`

**Tools used:** Pylint, Bandit, Flake8 (Python 3.14.7, virtual environment `myenv`)

---

## 1. Objective

Use static analysis tools to find and fix quality, style and security problems in `pet_shelter.py`, a small SQLite-backed register for an animal shelter (registers pets, processes adoptions, finds the oldest available pet, issues adoption codes, clears unadopted records).

## 2. Setup

```bash
python -m venv myenv
myenv\Scripts\activate
pip install pylint bandit flake8
python pet_shelter.py        # baseline output: Kiwi PET-XXXX active
```

## 3. Results summary

| Tool | Before | After |
|------|--------|-------|
| Pylint | 3.95/10 | **10.00/10** |
| Bandit | 3 issues (1 Medium, 2 Low) | **No issues identified** |
| Flake8 | 5 issues | **No output (clean)** |

The program output is unchanged after the fixes (`Kiwi PET-XXXX active`, the code is random).
Before and after reports are in the repository (`before/` folder for the originals; `pylint_report.txt`, `bandit_report.txt`, `flake8_report.txt` for the final ones).

## 4. Issues found and how they were fixed

Line numbers refer to the original file. The full table is in `issues.md`.

### Bandit (security)

| # | Issue | Severity | Line | Fix |
|---|-------|----------|------|-----|
| 1 | B608 SQL injection via `%` string formatting in the INSERT | Medium | 10 | Parameterized query: `VALUES (?, ?, ?, 0)` with a tuple |
| 2 | B311 `random.randint` used for adoption codes | Low | 22 | `secrets.randbelow(9000) + 1000` |
| 3 | B110 try/except/pass swallowing database errors | Low | 36-37 | Catch `sqlite3.Error` and print a message |

### Pylint (quality)

| # | Issue | Line | Fix |
|---|-------|------|-----|
| 4 | W0102 mutable default argument `tags=[]` | 9 | Default to `None`, create the list inside the function |
| 5 | W0702 bare `except` hiding the "no rows" case | 29 | Explicit `if row is None: return None` |
| 6 | W0603 / C0103 global counter `adoption_count` | 6, 16 | Removed the global; `shelter_status()` uses `SELECT COUNT(*)` |
| 7 | C0103 camelCase function names | 9, 21, 25, 33, 40 | Renamed to snake_case and updated callers |
| 8 | C0114 / C0116 missing docstrings | several | Added module and function docstrings |
| 9 | C0209 `%` formatting | 10 | Resolved by the parameterized query |
| 10 | Missing `if __name__ == "__main__":` guard (no tool flagged it) | 54 | Added the guard |

### Flake8 (style)

| # | Issue | Line | Fix |
|---|-------|------|-----|
| 11 | E501 / C0301 lines too long | 5, 10, 27 | Wrapped across multiple lines |
| 12 | E701 / C0321 multiple statements on one line | 41 | `return` moved to its own line |
| 13 | E302 / E305 blank-line spacing | appeared after other fixes | Added the missing blank lines |

## 5. Commit history

Below is the real, unedited git history of this repository (oldest first).

| Commit | Date (IST) | Message | What it contains |
|--------|-----------|---------|------------------|
| `71c62ce` | 16 Oct 2026, 10:08 | Baseline: original pet_shelter.py | Original code, README, empty `issues.md` and `reflection.md` |
| `c5c45fa` | 16 Oct 2026, 10:11 | Added before static analysis reports | `before/` folder with the original three reports |
| `4e8d1ef` | 16 Oct 2026, 10:14 | Document static analysis issues | Issue table in `issues.md` |
| `f6bfb63` | 16 Oct 2026, 10:28 | Fix Bandit issues: parameterized SQL, secrets for codes, handle sqlite errors | Fixes for issues 1-3, Bandit report clean |
| `b4f9e5e` | 16 Oct 2026, 10:36 | Fix Pylint issues: naming, docstrings, mutable default, bare except, global | Fixes for issues 4-10, Pylint 10.00/10 |
| `fd758f3` | 16 Oct 2026, 10:42 | Fix Flake8 style issues | Fixes for issues 11-13, Flake8 clean |
| `1a39fd0` | 16 Oct 2026, 10:48 | updated issues.md | Issue table updated to match the final code |

The lab handout asks for three fix commits (one per tool), which matches the Bandit, Pylint and Flake8 commits above. Each of those commits groups the fixes for that tool's findings.

## 6. What I learned

- **Bandit found the only real vulnerability.** The string-built INSERT was exploitable by SQL injection and also crashed on names containing an apostrophe.
- **Some fixes were logic fixes, not style fixes.** The bare `except` in `oldest_available` hid the case where no pet is available; the correct fix was an explicit `None` check.
- **Tools don't catch everything.** None of them flagged the missing `__main__` guard.
- **Fixes interact.** Wrapping long lines during the Pylint pass left only blank-line spacing for Flake8.
- **Re-running all three tools at the end matters,** since a fix for one tool can introduce warnings in another.

## 7. Deliverables checklist

- `pet_shelter.py` cleaned (13 issues fixed, well beyond the minimum of 4)
- `issues.md` completed
- Before and after reports for all three tools
- Three fix commits (Bandit, Pylint, Flake8)