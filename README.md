# Self Assessment — Classroom Quiz Platform

A browser-based self-assessment and classroom testing platform. Teachers create and manage question banks, launch live quiz sessions, and review results — all from a single HTML file hosted on GitHub Pages.

---

## Features

### For Teachers
- Multiple teachers can log in with the same shared password, each with their own name and email
- Add MCQ and True/False questions tagged by **Programme → Course ID/Subject → Unit/Topic → Teacher\_Date**
- All questions are stored in a shared question bank, filterable and reusable by any tag
- Curate a custom quiz for each session — choose number of questions, type, and duration
- Save and reload quiz templates for repeated use
- Launch a test session and share the QR code or a direct link with students

### Live Session & Lobby
- Students open the link in any browser — PC, iPhone, or Android
- Students enter their **Full Name**, **Roll Number**, and **Email** to join the session
- Teacher sees a live lobby showing all students who have joined:

```
Waiting Lobby — Chapter 3 Quiz
● Live                  4 joined
────────────────────────────────
1. Ahmed Khan      Roll: 2024-01   ● Waiting
2. Sara Malik      Roll: 2024-02   ● Waiting
3. Usman Ali       Roll: 2024-03   ▶ Started
4. Fatima Raza     Roll: 2024-04   ✓ Submitted

        [ 🚀 Start Test for All ]
        [ ⏹ End Session         ]
```

- Teacher clicks **Start Test** when all students have joined — the quiz goes live
- Teacher can remove a student from the lobby before the test starts (e.g. if they entered wrong details)

### During the Quiz
- Quiz runs in **fullscreen mode** on the student's device
- If a student swipes away or exits fullscreen:
  - **First offence:** 10-second countdown warning — return or the session restarts
  - **Second offence:** session restarts immediately with remaining time
  - All violations and off-screen duration are recorded
- Students can navigate back to review previous questions but **cannot change a selected answer**
- A **Next** button appears directly below the options — no scrolling needed
- Test ends when the student submits, time runs out, or the teacher ends the session
- If the teacher ends the session, all active students are auto-submitted

### Results
- Score and stats shown immediately after submission
- Students can share their result via WhatsApp, email, or any app
- Teacher can **unlock correct answers** so students can review their responses
- All results from all teachers and all tests are accessible on the dashboard
- Filter results by Programme, Subject, Topic, Test ID, Teacher, Student Name, Roll Number, or Email
- Download results as a CSV file
- Email results to any address directly from the dashboard

---

## Question Bank Structure

Questions are organised in a five-level hierarchy:

```
Programme
  └── Course ID + Subject Name   (e.g. ANAT-101 / Anatomy)
        └── Unit / Topic         (e.g. Upper Limb)
              └── Test ID        (format: TeacherName_Date, e.g. DrAhmed_Oct2025)
                    └── Question (MCQ or True/False)
```

All fields support autocomplete — start typing to see existing options, or type a new value to create one.

---

## Setup

### Prerequisites
- A free [Firebase](https://firebase.google.com) account (Google account required)
- A [GitHub](https://github.com) account for hosting

### Step 1 — Firebase Setup

1. Go to [firebase.google.com](https://firebase.google.com) and sign in
2. Click **Add project** → name it `self-assessment` → click through → **Create project**
3. Click the **Web icon `</>`** → nickname: `self-assessment` → **Register app**
4. Copy the following three values from the config block shown:
   - `apiKey`
   - `projectId`
   - `appId`
5. In the Firebase left menu: **Build → Firestore Database → Create database**
6. Choose **Start in test mode** → Next → pick any location → **Enable**

### Step 2 — Create config.json

Create a file named `config.json` with your Firebase values:

```json
{
  "k": "YOUR_FIREBASE_API_KEY",
  "p": "YOUR_FIREBASE_PROJECT_ID",
  "a": "YOUR_FIREBASE_APP_ID"
}
```

### Step 3 — Upload to GitHub Pages

1. In your GitHub repository, upload:
   - `index.html` (renamed from `classquiz-v5.html`)
   - `config.json`
2. Go to **Settings → Pages → Source → main branch** → Save
3. Your URL will be: `https://yourusername.github.io/repository-name`

### Step 4 — First-Time Admin Setup

1. Open your GitHub Pages URL
2. The **Create Admin Account** screen appears (one time only)
3. Enter your institution name, your name, your email, and set:
   - **Admin password** — for you alone (full access)
   - **Teacher password** — shared with all other teachers
4. Click **Create Admin Account** — credentials are saved to Firebase

All subsequent visits go directly to the login screen.

---

## Logging In

| Role | How to log in |
|---|---|
| **Admin** | Your email + admin password — gives full access including settings |
| **Teacher** | Any name + any email + teacher password — gives access to questions, sessions, and results |

Teacher name and email are remembered on each device so they are pre-filled on return visits.

---

## Security Notes

- Passwords are hashed before storage — never stored in plain text
- Session URLs use 16-character random IDs (e.g. `?s=Kx7mR2pQwZ4nLv9T`) — not guessable
- Firebase config values are public keys — they are safe to include in the repository
- Set Firestore security rules to restrict direct database access once out of test mode

---

## Tech Stack

- **Frontend:** Single HTML file — no framework, no build step
- **Database:** Firebase Firestore (free Spark plan)
- **Hosting:** GitHub Pages (free)
- **QR codes:** [api.qrserver.com](https://api.qrserver.com) — no library required
- **Fonts:** Google Fonts (Syne + DM Sans)

No npm, no server, no monthly cost.

---

## Browser Compatibility

| Browser | Support |
|---|---|
| Chrome on Android | ✅ Full support |
| Safari on iPhone | ✅ Full support |
| Chrome on iPhone | ✅ Full support |
| Chrome on Mac/PC | ✅ Full support |
| Safari on Mac | ✅ Full support |
| Firefox | ✅ Full support |

---

## Licence

Free to use for educational purposes.
