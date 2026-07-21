# DROPGUARD AI

AI-Powered Student Dropout Prediction & Performance Monitoring — React + TypeScript + Tailwind frontend with an Excel-backed Node API.

## Run it

```bash
npm install
npm run dev
```

This starts **both** the API server (http://localhost:3001) and the web app (http://localhost:5173). Open http://localhost:5173.

## Data storage — Excel files (nothing is hardcoded)

Every value shown in the app lives in — or is computed live from — the Excel files in the project's `data/` folder (created and seeded automatically on first run):

| File | Contents | Seeded |
|---|---|---|
| `data/students.xlsx` | Name, Email, Password, Department, Year, Attendance, CGPA, QuizAverage, SemMarks, **StudyStreak, XP**, subject marks (Maths, DSA, DBMS, OS, Networks, AIML), 6-month attendance history (Att1–Att6), RiskScore, RiskLevel | 50 students |
| `data/teachers.xlsx` | TeacherID (T001–T020), Name, Password, Department | 20 teachers |
| `data/admins.xlsx` | Name, Email, Password | 1 admin |
| `data/events.xlsx` | Calendar / study-schedule events (exams, deadlines, drives) | 7 events |
| `data/internships.xlsx` | Internship listings with CGPA eligibility | 5 listings |
| `data/jobs.xlsx` | Job listings (match % computed per student) | 5 listings |
| `data/certificates.xlsx` | Per-student certificates, linked by Email | ~35 rows |
| `data/meta.xlsx` | Landing stats, testimonials, ML model version history | 3 sheets |

Every new signup is appended to the matching Excel file. Edit any cell and the app reflects it on the next request — streaks, XP, marks, leaderboards, charts, risk levels, everything. Deleting a file re-seeds it with defaults.

What's computed live from that data (not stored): risk scores, class averages, leaderboard ranks, career matches, skill gaps, placement readiness, job match %, AI suggestions, and notifications. The remaining in-code content is feature material only (interview question bank, coding problem set, email templates, chatbot phrasing) — all numbers in them come from the student's record.

## Login

- **Student** — login with College Email + password. Signup collects Name, College Email, Department, Year, Attendance %, CGPA. Seeded students use password `student123` (emails are inside `data/students.xlsx`).
- **Teacher** — login with **Teacher ID** + password only (no email, no password reset). Signup: Full Name, Teacher ID, Password, Confirm Password. Seeded teachers: `T001`–`T020`, password `teach123`.
- **Admin** — `admin@dropguard.ai` / `admin123`.

After login or signup you land directly on your dashboard.

## Dropout risk — one centralized calculation

`computeRisk(attendance, cgpa)` in `server/index.mjs` is the single source of truth:
`score = 100 − (0.55 × attendance + 4.5 × cgpa)` → **Low** < 20 ≤ **Medium** < 40 ≤ **High**

Examples: 92% / 8.8 → Low · 78% / 6.9 → Medium · 60% / 5.4 → High.
The same values appear on the Student Dashboard, Teacher Dashboard, and Students page.

## Teacher portal (simplified)

- **Dashboard** — four live stats computed from every registered student: Total Students, Low / Medium / High Risk.
- **Students** — search bar, risk filter, and a table (Name, Department/Year, Attendance %, CGPA, Dropout Risk) fed directly from `data/students.xlsx`. New registrations appear on refresh with no manual entry.

## Stack

React 18 + TypeScript + Vite · Tailwind CSS · Framer Motion · Recharts · Lucide React · Express + SheetJS (xlsx)

