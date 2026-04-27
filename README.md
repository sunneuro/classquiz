# ClassQuiz

A browser-based quiz and assessment platform built for classroom use — no app installs required for students. Teachers manage everything from a single `index.html` file backed by Firebase Firestore. Students join via a shared link on any device.

---

## Purpose

ClassQuiz is designed for educators who need a fast, no-friction way to conduct assessed quizzes with real psychometric feedback. The entire app is a self-contained HTML file. There is nothing to install, no server to maintain, and no per-student account setup.

---

## How It Works

The app operates in two modes determined by the URL:

- **Teacher mode** — the plain URL (e.g. `https://yoursite/`). Requires a password login. All quiz management, analytics, and administration happens here.
- **Student mode** — a URL with a session parameter (e.g. `?s=SESSION_ID`). Students open this link, enter their name, roll number, and email, and are placed in a lobby until the teacher starts the quiz.

All data — questions, sessions, results, lobby state — is stored in Firebase Firestore and synced in real time across devices.

---

## Features

### For Teachers

**Question Bank**
- Add MCQ (multiple choice) and True/False questions
- Tag difficulty (Easy / Medium / Hard), Programme, Course ID, Subject, Topic, and Test ID
- Tag questions by Cognitive Level: Recall, Comprehension, Application, Analysis, Synthesis, Evaluation (Bloom's Taxonomy)
- **AI Auto-Tag** — bulk-tag untagged questions using heuristic keyword analysis of question stems
- **AI-assisted tagging** via the Anthropic Claude API for richer cognitive classification with rationale
- Edit or delete individual questions; bulk-commit drafts in one action
- Inline cognitive level editing directly from the question bank list

**Curate Quiz**
- Hand-pick questions from the Master Bank to assemble a paper
- Save and reload papers as reusable templates
- Set a per-student time limit and an optional open window — a countdown after which new students cannot join and the session auto-closes
- Set a questions-per-student count — each student gets a different random selection from the pool
- Download the question paper (with or without answer key) as a portable HTML file for offline use

**Run Session**
- Launch a live session and share the student join link or QR code
- Monitor a real-time lobby showing who has joined (name, roll, status)
- Kick individual students from the lobby
- Lock or unlock answer review for students after the quiz ends
- End the session manually or let it auto-close when the time window expires
- **Session recovery** — if the page refreshes or the internet drops mid-session, the active session is automatically detected and restored on next login
- A **Back to Active Session** button is always visible from any screen

**Results & Analytics**
- View all results filterable by Programme, Subject, Topic, Session ID, Teacher, and student name/roll
- Sortable columns including score, performance score, violations, time taken
- **Performance Score** — composite metric combining percentage correct, time efficiency, and integrity violations
- **Leaderboard** ranked by performance score
- **Student Profiles** — aggregated view per student across all sessions: average score, average performance, sessions count

**Session Analytics — three views:**

*Class Overview*
- Mean score, well-answered questions count, weak discriminators count
- Class performance by Cognitive Level with strongest/weakest level identification
- Class confidence vs accuracy matrix (Sure / Not So Sure / Wild Guess vs correctness)

*By Question*
- Per-question difficulty (% correct) and discrimination index (D-index, top/bottom 27% method)
- Distractor distribution — visual bar showing how students distributed across answer options
- Average response time overall and for wrong answers only
- Cognitive level and difficulty tags per question
- D-index classification: Excellent (≥0.4) / Acceptable (≥0.2) / Weak / Review

*By Student*
- Per-student cognitive profile — score per cognitive level for every student in the session
- Confidence calibration (Well-calibrated / Overconfident)
- Cognitive snapshot descriptor per student (e.g. "Strong on Application · Weak on Analysis")
- One-tap **Strategy** button with personalised revision guidance per student

- **Cross-session cognitive speed chart** — timing norms per cognitive level across all sessions for a programme/subject
- Clean Duplicates tool — keeps only the best result per student per session
- Edit student details (name, roll, email) across all their results in one action

**My Students**
- Consolidated view of all students who have sat any quiz
- Edit or delete individual students; bulk-delete selected students

**Admin Office** *(admin account only)*
- Manage registered teacher accounts — view, delete, reset passwords
- View and manage all students across all teachers
- Handle password reset requests

---

### For Students

- Join via link — no account or app needed
- Autocomplete suggestions on the login form (name, roll, email) sourced from existing lobby entries
- Roll number automatically normalised to uppercase throughout
- Quiz runs fullscreen; leaving the screen is detected and logged as a violation
- **Confidence tagging** — mark each answer as Sure / Not So Sure / Wild Guess
- Per-question countdown timer with a progress bar
- Navigate between questions freely before submitting
- Reconnect automatically if connection drops mid-quiz (within a 2-minute window)
- Review correct answers after submission once the teacher unlocks them
- **Download Answer Sheet** — portable HTML file with question, student's answer, correct answer, and score summary

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Single-file HTML/CSS/JS — no build step |
| Database | Firebase Firestore (compat SDK v10) |
| Auth | Firebase Anonymous Authentication |
| AI tagging | Anthropic Claude API (optional) |
| Hosting | GitHub Pages / Firebase Hosting |
| Fonts | Google Fonts (Syne, DM Sans) |

---

## First-Time Setup

1. Create a Firebase project and enable Firestore and Anonymous Authentication
2. Add your Firebase config (API key, Project ID, App ID) via the first-run wizard on first open
3. Set Firestore security rules — `results` and `teachers` require `request.auth != null` for writes
4. Deploy `index.html` to GitHub Pages or Firebase Hosting
5. Open the URL — the app detects no existing setup and walks you through creating the master admin account

---

## Security Notes

- All write operations require Firebase Anonymous Authentication (`request.auth != null`)
- The `settings` and `teachers` collections are write-locked in Firestore rules
- Quiz screens enforce fullscreen; exit events are recorded as integrity violations and factored into performance scores
- Roll numbers are normalised to uppercase to prevent duplicate student records
