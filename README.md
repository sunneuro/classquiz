# ClassQuiz

**A browser-based quiz and assessment platform for teachers and students — no installation required.**

ClassQuiz runs entirely from a single HTML file, backed by Google Firebase (Firestore). Open it in any browser on any device. No server, no app store, no setup beyond the first-run wizard.

---

## What It Does

ClassQuiz lets teachers build a question bank, assemble quiz papers, launch live sessions, and view ranked results — all from one screen. Students join via a URL on their own device and take the quiz in real time.

**Core features:**

- **Question Bank** — Build a searchable master bank of MCQ and True/False questions, tagged by programme, subject, unit/topic, difficulty, and test ID
- **Question Paper Wizard** — Write new questions into a draft or pull from the bank; auto-generates a test ID; supports saving drafts mid-session
- **Live Quiz Sessions** — Teacher launches a session, students join by URL with name, roll number, and email; teacher controls start, monitors lobby, ends session
- **Per-student question randomisation** — Question order is shuffled independently for each student; MCQ option order is also shuffled (so "option B" is different for every student)
- **Random n-from-x delivery** — Set a pool of 40 questions and serve only 20 to each student; each student gets a different random 20
- **One attempt per session** — Roll number checked against both the lobby and completed results; duplicate entry blocked
- **Anti-cheating** — Tab/window-switch detection with violation counter; performance score penalised per violation; fullscreen enforcement; violation restart reshuffles all questions
- **Performance ranking** — Score%, speed bonus, and integrity multiplier combine into a single performance score; leaderboard ranks all students with medals
- **Results dashboard** — Filterable table (programme, subject, topic, test ID, teacher, session, free text search); CSV download
- **Leaderboard** — Fully filter-aware; respects all active filters including teacher and programme
- **Admin Office** — Password-protected admin panel with three tabs: delete teacher profiles/sessions, delete students by roll number with bulk selection, change passwords (admin, shared teacher, individual teachers)
- **Manage Teachers** — Register, reset passwords, remove teachers; pending password reset requests from teachers shown as notifications
- **Password reset requests** — Teacher clicks "Request Password Reset" on login screen; request written to Firestore and notified to admin in Manage Teachers; admin resets directly from the notification
- **Home screen filtering** — Teachers see only their own subjects and sessions; admin has a My Work / Teachers tab to switch between own content and any teacher's content
- **Student-facing** — Clean quiz UI with question dots, navigation, answer locking on move, review mode after submit, score reveal with answer review (if unlocked by teacher)

---

## File Structure

```
ClassQuiz_Final_v2.html    ← The entire application (single file)
ClassQuiz_README.md        ← This file
```

That's it. One HTML file is the complete application.

---

## First-Time Setup

1. Open `ClassQuiz_Final_v2.html` in Chrome or Safari
2. The app connects to Firebase automatically
3. On first run, a setup wizard appears — fill in:
   - **Institution / App Name** (shown on login screen)
   - **Admin full name and email**
   - **Admin password** (your personal login)
   - **Teacher password** (shared with all teachers who don't have individual accounts)
4. Click **Create Admin Account** — setup is complete

> Setup only runs once. After that, the login screen is shown on every open.

---

## Teacher Accounts

There are two types of teacher login:

| Type | How it works |
|---|---|
| **Shared password** | Any teacher enters their name, email, and the shared teacher password. No pre-registration needed. |
| **Individual account** | Teacher registers on the login screen with their own email and password. Admin can reset their password from Manage Teachers or Admin Office. |

The **admin account** uses the admin email + admin password and has access to all management features.

---

## Running a Quiz — Quick Flow

1. **Create Question Paper** → Write or select questions → Save draft or go to Launch
2. **Curate Quiz** → Review paper → Set session name, time limit, questions per student → **Launch Quiz →**
3. Share the session URL with students (shown on the Launch screen, also as QR)
4. Watch the lobby fill → Click **Go Live** when ready
5. Monitor live results → End session when done
6. Go to **All Results** to view, filter, and download

---

## Student Flow

1. Open the session URL on any device
2. Enter name, roll number, email → Join
3. Wait for teacher to go live
4. Take the quiz — answers lock when moving to the next question; can navigate back to unlocked questions
5. Submit → See score and (if teacher unlocks) answer review

---

## Firebase Project

The app is connected to Firebase project **`classquiz-bc2a2`**. Credentials are embedded in the HTML file. The Firestore database uses the following collections:

| Collection | Contents |
|---|---|
| `settings` | App config (name, admin credentials, passwords) |
| `questions` | Master question bank |
| `teachers` | Individual teacher accounts |
| `sessions` | Live and past quiz sessions |
| `results` | Student quiz results |
| `drafts` | Saved question paper drafts |
| `lobby_{sessionId}` | Per-session student lobby (one per live session) |
| `pw_requests` | Teacher password reset requests |

---

## Passwords & Security

- Passwords are stored as a djb2 hash in Firestore alongside the plaintext (for password reminder use)
- The forgot-password flow is removed; teachers request a reset through the app → admin resets from Manage Teachers
- Admin Office is protected by a second password prompt even when already logged in as admin
- No external authentication service is used

---

## Known Constraints

- **Single file, file:// compatible** — works when opened directly from disk in Chrome or Safari (no local server needed)
- **No video/audio recording** — not implemented; anti-cheating relies on violation detection and question/option randomisation
- **Firebase Storage not used** — all data is text/JSON in Firestore; no image or file uploads
- **Not designed for simultaneous large cohorts** — suitable for classroom-scale sessions (tested up to ~60 students)

---

## Version History

| Version | File | Notes |
|---|---|---|
| v1 | `ClassQuiz_Final_v1.html` | Stable baseline with Admin Office, leaderboard filters, password reset flow, Chrome fix |
| v2 | `ClassQuiz_Final_v2.html` | MCQ option shuffling, one-attempt enforcement, random n-from-x questions, home screen teacher filtering |

---

*Built with Firebase Firestore · Syne & DM Sans fonts · No frameworks · No build step*
