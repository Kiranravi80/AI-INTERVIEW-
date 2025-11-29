# AI Interview Agent (Full-stack Demo)

This repository demonstrates a local, full-stack Interviewer Agent that asks candidates questions, persists answers to SQLite, and validates responses with multiple LLMs (OpenAI + Google Generative models). This iteration added JWT authentication, admin-only endpoints, strict LLM evaluation parsing with retries, and a basic test + CI setup.

Highlights
- Express backend + SQLite persistence
- JWT-based login & role checks (admin + candidate)
- Admin-only CRUD for questions and session reporting
- LLM evaluation: OpenAI + Google Generative SDK (fallback to REST), JSON schema validation, retries and aggregation
- Frontend updates: admin pages for questions and sessions; candidate flow persists sessions and status
- Tests with Jest + Supertest and GitHub Actions CI

Getting started (Windows PowerShell)

1. Install dependencies

```powershell
npm install
```

2. Copy and configure environment variables

```powershell
copy .env.example .env
# Add OPENAI_API_KEY, GOOGLE_APPLICATION_CREDENTIALS or GOOGLE_API_KEY, and optionally ADMIN_PASSWORD
```

3. Initialize DB and seed admin user

```powershell
npm run migrate
```

4. Start server

```powershell
npm run dev
# Open http://localhost:3000
```

API overview
- POST /api/auth/login — { userId, password } -> { token }
- POST /api/auth/register — create candidate (admin registration is not allowed)
- POST /api/sessions — start a new session
- GET /api/sessions/:id/question — get next question
- POST /api/sessions/:id/answer — submit answer (triggers evaluation)
- POST /api/sessions/:id/complete — mark completed/disqualified (auth)
- Admin-only: /api/admin/questions and /api/admin/sessions

LLM / Gemini notes
- OpenAI: set OPENAI_API_KEY
- Google Generative: prefer SDK via GOOGLE_APPLICATION_CREDENTIALS; fallback to GOOGLE_API_KEY REST
- Model outputs are expected to return JSON: { score: 0-100, feedback: "string", disqualify?: true }

Tests & CI
- Run tests: `npm test`
- CI: GitHub Actions workflow included at `.github/workflows/ci.yml`

Next steps
- Add more secure auth & permissions (role checks and resource ownership)
- Harden production readiness (rate limits, monitoring, secrets management)
- Improve LLM prompting and full schema-based parsing

If you'd like, I can now add stronger admin reporting UI, integrate Google OAuth for service accounts, or build a full E2E test suite — which would you prefer next?
# AI Interview Agent Platform

A professional AI-powered video interview platform with adaptive questioning, proctoring, and intelligent evaluation.

## Project Structure

```
AI_Interview_Agent/
├── index.html          # Main HTML entry point
├── css/
│   └── style.css       # All styles and design system
├── js/
│   └── app.js          # Main application logic
└── README.md           # This file
```

## Features

### Admin Dashboard
- Create interviews with job details
- Assign candidates to interviews
- View active interviews
- Monitor candidate results
- See performance scores and match probability

### Candidate Portal
- Register and login
- View assigned interviews
- Upload and analyze resume
- Fullscreen video interview
- Adaptive questioning (7-15 questions)
- Real-time proctoring

### Interview Flow
1. **Join Interview** - Enter interview link or select from assigned
2. **Resume Upload** - Auto-analysis of skills match
3. **Instructions** - Read guidelines and accept consent
4. **Fullscreen** - Auto-activate fullscreen mode
5. **Questions** - 5-15 adaptive questions based on performance
6. **Completion** - Thank you screen (no scores shown to candidate)

### Proctoring Features
- Auto-fullscreen enforcement
- Tab switch detection (max 3 violations)
- Fullscreen exit = instant disqualification
- Violation tracking
- Timestamps and audit trail

### Results (Admin Only)
- Performance scores (0-100%)
- Job match probability
- Question-by-question breakdown
- Quality ratings for each answer
- Proctoring violation summary
- Hiring recommendations

## Setup & Usage

### Installation
1. Extract the project files
2. No server required - works locally
3. Open `index.html` in any modern web browser

### Demo Credentials

**Admin:**
- User ID: `admin`
- Password: `admin`

**Candidate (Pre-registered):**
- User ID: `demo_john`
- Password: `demo123`

**Or Register as New Candidate**

### How to Use

#### As Admin
1. Login with admin/admin
2. Go to "Create Interview"
3. Fill job details (title, description, skills, level)
4. Select candidates to assign
5. Click "Create Interview"
6. View active interviews and results

#### As Candidate
1. Register new account OR login as demo_john/demo123
2. Go to "My Interviews"
3. Click "Start Interview"
4. Upload resume and let system analyze
5. Read instructions and accept all consent checkboxes
6. Complete fullscreen interview
7. Answer 5+ questions based on performance
8. See completion message (results go to admin only)

## File Descriptions

### index.html
- Entry point of the application
- Links CSS and JavaScript files
- Provides single mount point for app

### css/style.css
- Design system with CSS variables
- All component styles
- Responsive layout
- Dark mode ready
- Professional color scheme

### js/app.js
- Core application logic
- State management
- User authentication
- Interview flow control
- Real-time scoring
- Proctoring system
- Admin and candidate functionality

## Technology Stack

- **Frontend:** HTML5, CSS3, Vanilla JavaScript
- **Storage:** In-memory (browser session)
- **Compatibility:** All modern browsers (Chrome, Firefox, Safari, Edge)
- **No dependencies:** 100% standalone

## Features in Detail

### Adaptive Questioning
- Questions 1-3: Easy (foundational assessment)
- Calculate average score after Q3
- If score ≥70: Ask 15 total questions (with advanced difficulty)
- If score <70: Ask only 7 questions (core competencies)

### Answer Evaluation
Each answer scored on:
- Relevance to question (25 points)
- Technical depth/accuracy (25 points)
- Communication clarity (25 points)
- Completeness (25 points)
- Total: 0-100 per answer

### Results Visibility
**Candidates see:** Only completion message + no scores
**Admin sees:** All metrics, detailed reports, recommendations

### Proctoring System
- Fullscreen exit = Auto-disqualify
- Tab switches: Warnings → Disqualify after 3
- Violations logged with timestamps
- Complete audit trail

## Browser Support

- ✓ Chrome (latest)
- ✓ Firefox (latest)
- ✓ Safari (latest)
- ✓ Edge (latest)
- ✓ Mobile browsers (responsive)

## Data Privacy

- All data stored in browser memory
- No server communication
- No external API calls
- Session data cleared on refresh
- Ideal for offline use

## Future Enhancements

- Real video recording and storage
- OpenAI API integration for question generation
- Database backend
- Email notifications
- Advanced reporting
- Mobile app version
- Multi-language support

## License

Professional recruitment platform - All rights reserved

## Support

For issues or questions, contact the development team.

---

**Version:** 1.0
**Last Updated:** November 29, 2025
