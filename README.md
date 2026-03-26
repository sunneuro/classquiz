# Agni Pariksha — Self Assessment Platform

A browser-based quiz platform for classrooms. Single HTML file, hosted on GitHub Pages, powered by Firebase. No installation, no server, no app download required.

---

## Quick Start

1. Upload `index.html` to a GitHub repository
2. Enable GitHub Pages (Settings → Pages → main branch)
3. Open the URL → first-run setup screen appears → set app name and passwords
4. Share the URL with teachers and students

---

## How a Test Works

```
Teacher creates questions  →  Curate & launch  →  Share QR code
Students scan QR           →  Join lobby       →  Teacher clicks Go Live
Students take quiz         →  Submit           →  Teacher views results
```

---

## For Teachers

### Creating a Question Paper
1. **Create Question Paper** — enter Programme, Course ID, Subject, Unit/Topic
2. Write questions (MCQ or True/False), set difficulty, save
3. Add more from the **Master Question Bank** if needed
4. **Curate Quiz** to review, edit, or remove questions
5. Set time limit → **Launch Quiz** → share QR code or link

### During a Live Session
- **Lobby** shows students joining in real time with their status
- Click **Go Live** when all students are ready
- **Answers tab** lets you unlock correct answers after submission
- Auto-unlock triggers when all students submit
- Green **🟢 Return to Session** button stays visible on all pages

### Results
- **Results Table** — all submissions, sortable by any column
- **Leaderboard** — ranked by Performance Score
- **Student Profiles** — aggregate average per student across all tests
- Filter by Programme, Subject, Topic, Session ID, Teacher
- Download CSV exports only what is currently filtered/visible

---

## For Students

1. Scan QR code or open the link on any browser
2. Enter name, roll number, and email → Join
3. Wait in lobby until teacher starts the session
4. Read the instructions → tap **Start Exam**
5. Answer questions — selected answers lock when you move on
6. Submit when done — score appears immediately
7. Correct answers shown when teacher unlocks them

### Important Rules
- Exam runs in **full screen** on mobile
- Leaving the screen is detected as a **violation**
- V1 = 10s warning · V2 = 7s warning · V3 = 4s then restart · V4+ = 2s restart
- Violations reduce your **Performance Score** (not raw score)
- If you lose connection, rejoin within **2 minutes** to resume from where you left off

---

## Performance Score

```
Performance = min(Score% + SpeedBonus, 100) × IntegrityMultiplier

SpeedBonus = (min(allottedTime ÷ timeTaken, 2.0) − 1.0) × 10   →  max +10 pts

IntegrityMultiplier:
  0 violations = 1.00   1 = 0.95   2 = 0.90
  3 = 0.80   4 = 0.75   5+ = 0.55
```

Speed gives a small bonus (max +10). Violations apply a multiplier penalty. Raw score is the foundation — speed and integrity only modify it.

---

## Access Levels

| Role | Login |
|---|---|
| **Admin** | Admin email + admin password |
| **Teacher** | Any name + any email + shared teacher password |

Admin can: add/edit/delete students, manage all results, configure settings.  
Teachers can: create questions, run sessions, view results.

---

## Firebase Setup

1. Go to [firebase.google.com](https://firebase.google.com) → Create project
2. Add a web app → copy `apiKey`, `projectId`, `appId`
3. Firestore Database → Create → Start in test mode
4. Paste credentials into `index.html` (search for `classquiz-bc2a2` to find the location)

---

## Features at a Glance

- MCQ and True/False questions
- Question hierarchy: Programme → Course → Subject → Unit/Topic → Test ID
- Master Question Bank shared across all teachers, filterable at all levels
- Draft system — question papers save automatically, resume across sessions
- QR code + shareable link for students
- Fullscreen lock on mobile with escalating violation detection
- Answer lock — selected answers cannot be changed after navigating away
- Post-submission rejoin — students can return to view their results
- Network disconnect rejoin — resume within 2 minutes from last question
- Live lobby with student status (Waiting / Started / Submitted)
- Auto-unlock answers when all students submit
- Performance Score combining accuracy, speed, and integrity
- Admin Office — manage teachers, students, and all data
- Email typo detection on login (gmail.com, yahoo.com, curaj.ac.in, etc.)
- Roll number normalised to uppercase — `2025msn05` = `2025MSN05`

---

## Browser Support

Chrome, Safari, Firefox — desktop and mobile.  
Fullscreen lock works on iPhone and Android.
