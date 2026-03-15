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
