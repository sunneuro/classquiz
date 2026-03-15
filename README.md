# Self Assessment — Classroom Quiz Platform

A browser-based quiz and self-assessment tool for classrooms. Single HTML file, hosted on GitHub Pages, powered by Firebase.

---

## How It Works

1. Teacher adds questions tagged by Programme → Course → Unit/Topic
2. Teacher curates a quiz, launches it, shares QR code or link
3. Students join on any browser (phone or PC), enter name, roll number, email
4. Teacher starts the session from the lobby when all students have joined
5. Students tap Begin, take the quiz fullscreen, submit when done
6. Teacher views live results on the dashboard, optionally unlocks answer review

---

## Features

**Question Bank**
- MCQ and True/False question types
- Organised by: Programme → Course ID + Subject → Unit/Topic → Test ID/Comment
- Shared across all teachers, filterable by any tag
- Test ID auto-generated from Course ID + Unit/Topic

**Live Sessions**
- Teacher launches session → QR code + shareable link generated
- Live lobby shows students joining in real time (Waiting / Started / Submitted)
- Teacher clicks Go Live when ready
- Teacher can remove students from lobby before start
- Teacher can end session — auto-submits remaining students

**Student Experience**
- Fullscreen lock on mobile (iPhone and Android)
- Tab/window switching detected and penalised:
  - 1st offence: 10-second warning
  - 2nd offence: 7-second warning
  - 3rd offence: 4-second countdown then restart
  - 4th+ offence: any off-screen >2 seconds = immediate restart
- Violation count never resets across restarts
- Can navigate back to review — cannot change a selected answer
- Score shown immediately after submission
- Correct answers visible only after teacher unlocks them

**Results Dashboard**
- All results from all teachers and all tests in one place
- Filter by Programme, Subject, Unit/Topic, Test ID, Teacher
- Search bar searches across all columns simultaneously
- Session ID format: `BCD05_UpperLimb_15March2026_02:25AM`
- CSV download with session ID as filename


**Calculation of Performance**
PERFORMANCE SCORE — FORMULA, EXPLANATION & WORKED EXAMPLES
============================================================

PURPOSE
-------
The Performance Score ranks students on a single 0–100 scale that rewards
genuine knowledge, appropriate speed, and academic integrity. It ensures
that a well-prepared, honest student always ranks above one who scores
similarly but cheats or rushes carelessly.


THE FORMULA
-----------

  Performance Score = min(Score% + SpeedBonus, 100) × IntegrityMultiplier


COMPONENT 1: Score%
-------------------
  Score% = (correct answers / total questions) × 100

This is the foundation. A student who answers 7 out of 10 correctly = 70.
All other components only modify this — they cannot compensate for a low score.


COMPONENT 2: SpeedBonus
-----------------------
  SpeedBonus = (min(allottedTime / timeTaken, 2.0) − 1.0) × 10

Breaking it down:

  allottedTime / timeTaken
    → The ratio of time given to time used.
    → If given 15 minutes and took 15 minutes: ratio = 1.0
    → If given 15 minutes and took 7.5 minutes: ratio = 2.0
    → If given 15 minutes and took 5 minutes:   ratio = 3.0

  min(..., 2.0)
    → Caps the ratio at 2.0. Finishing in half the allotted time
      is the maximum reward. Finishing even faster gains nothing extra.
    → This prevents a student who clicks through randomly in 60 seconds
      from gaming the speed bonus.

  − 1.0
    → Shifts the scale so that using the full allotted time = 0 bonus.
    → After subtracting 1.0, the range becomes 0.0 to 1.0.

  × 10
    → Scales the bonus to a maximum of +10 points.
    → A student using 100% of the time gains 0 bonus.
    → A student using 75% of the time gains +3.3.
    → A student using 50% of the time (or less) gains +10.

  min(Score% + SpeedBonus, 100)
    → Caps the result at 100 before applying the integrity multiplier.
    → A student with 95% score and maximum speed bonus (10) would
      otherwise exceed 100.

NOTE ON SPEED: For MCQ and True/False assessments, a well-prepared student
should naturally finish faster. Speed is not penalised — it gives a modest
bonus of up to 10 points. Slow, careful students are not disadvantaged
beyond losing this small bonus.


COMPONENT 3: IntegrityMultiplier
---------------------------------
Violations occur when a student leaves the quiz screen (switches tabs,
minimises window, or exits fullscreen on mobile).

  Violations    Multiplier    Penalty
  ----------    ----------    -------
  0             1.00          None
  1             0.95          −5%
  2             0.90          −10%
  3             0.80          −20%
  4             0.75          −25%
  5 or more     0.55          −45% (capped here)

The multiplier applies to the entire score including the speed bonus.
A student cannot compensate for violations by scoring high or finishing fast.
The steep drop at 5+ violations (0.75 → 0.55) reflects a persistent pattern
of leaving the screen.


FULL SCALE
----------
  Maximum possible: 100.0 (perfect score + fastest + zero violations)
  Displayed to one decimal place.
  Colour coding in dashboard:
    ≥ 90  green
    ≥ 75  blue/accent
    ≥ 60  amber
    < 60  red


WORKED EXAMPLES — All students score 90%, 15-minute test
---------------------------------------------------------

Student   Score%  Time Used       Violations  SpeedBonus  Base (capped)  Multiplier  PERFORMANCE
--------  ------  --------------  ----------  ----------  -------------  ----------  -----------
Ahmed     90%     7.5 min (50%)   0           +10.0       100.0          × 1.00      100.0
Sara      90%     11.25 min (75%) 0           +3.3        93.3           × 1.00       93.3
Ali       90%     7.5 min (50%)   2           +10.0       100.0          × 0.90       90.0
Fatima    90%     15 min (100%)   0           0.0         90.0           × 1.00       90.0
Usman     90%     7.5 min (50%)   3           +10.0       100.0          × 0.80       80.0

RANKING:
  1st  Ahmed   — 100.0  Honest and fast. Maximum reward.
  2nd  Sara    —  93.3  Honest and careful. Speed bonus is modest but counts.
  3rd  Ali     —  90.0  Fast but 2 violations. Speed bonus cancelled by penalty.
  3rd  Fatima  —  90.0  Used full time, zero violations. Tied with Ali.
  5th  Usman   —  80.0  Same score and speed as Ahmed, but 3 violations cost 20%.

KEY OBSERVATIONS:
  → Ahmed and Usman both scored 90% and finished in the same time.
    Ahmed scores 100, Usman scores 80. Integrity costs 20 points.

  → Ali (fast, 2 violations) and Fatima (slow, honest) tie at 90.0.
    Speed bonus and integrity penalty cancel out exactly.

  → Sara scores higher than Ali despite being slower, because she has
    zero violations versus Ali's two.

  → Usman ranks last despite the highest raw score and fastest time,
    because 3 violations apply a 20% multiplier reduction.

  → Honesty and integrity prevail. A perfect score cannot overcome
    repeated violations.


FORMULA SUMMARY
------------------------------

  Performance Score = min(Score% + SpeedBonus, 100) × IntegrityMultiplier

  SpeedBonus       = (min(allottedTime / timeTaken, 2.0) − 1.0) × 10
                     Range: 0 to +10 points
                     Max bonus reached when time taken ≤ 50% of allotted time

  IntegrityMultiplier:
    0 violations = 1.00  |  1 = 0.95  |  2 = 0.90
    3 = 0.80  |  4 = 0.75  |  5+ = 0.55


---

## Access

| Role | Login |
|---|---|
| **Admin** | Admin email + admin password — full access |
| **Teacher** | Any name + any email + shared teacher password |

Passwords set once on first launch. Forgot password opens your email app with a reminder pre-filled.

---

## Setup

### 1. Firebase
1. Go to [firebase.google.com](https://firebase.google.com) → Create project
2. Add a web app → copy `apiKey`, `projectId`, `appId`
3. Build → Firestore Database → Create database → Start in test mode

### 2. config.json
Create `config.json` in the same folder as `index.html`:
```json
{
  "k": "YOUR_API_KEY",
  "p": "YOUR_PROJECT_ID",
  "a": "YOUR_APP_ID"
}
```

### 3. GitHub Pages
Upload `index.html` and `config.json` → Settings → Pages → main branch → Save

### 4. First Launch
Open your GitHub Pages URL → Create Admin Account → set app name, passwords → done

---

## Tech Stack
- Single HTML file — no build step, no npm, no server
- Firebase Firestore for all data
- GitHub Pages for hosting (free)
- QR codes via api.qrserver.com

Supports Chrome, Safari, Firefox on desktop and mobile.
