---
title: "Week 6 Worklog"
date: 2026-05-25
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---




### Week 6 Objectives:

* Build core platform features: authentication, exam engine, and learner dashboard.
* Integrate Cognito authentication flow with the frontend.

### Tasks carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | - Implement Cognito authentication handlers in backend: <br>&emsp; + User registration and login <br>&emsp; + JWT token verification middleware <br>&emsp; + Role-based access control (learner/admin) | 25/05/2026 | 25/05/2026 | |
| Tue | - Build exam engine backend API: <br>&emsp; + Create exam sessions <br>&emsp; + Submit answers with persistence <br>&emsp; + Calculate scores with Cambridge band mapping | 26/05/2026 | 26/05/2026 | |
| Wed | - Develop learner dashboard API: <br>&emsp; + Daily recommendations endpoint <br>&emsp; + Exam history and progress tracking <br>&emsp; + Mistake pattern analysis | 27/05/2026 | 27/05/2026 | |
| Thu | - Build frontend: <br>&emsp; + Login/Register pages with Cognito integration <br>&emsp; + Dashboard layout with React Query data fetching <br>&emsp; + Responsive navigation with TailwindCSS | 28/05/2026 | 28/05/2026 | |
| Fri | - Integrate frontend → backend → Cognito <br> - Test full authentication flow end-to-end <br> - Fix CORS issues between Amplify frontend and API Gateway | 29/05/2026 | 29/05/2026 | |

### Week 6 Achievements:

* Implemented complete authentication system:
    * User registration with email verification (Cognito)
    * Login with JWT token issuance
    * Role-based middleware: learner vs admin routes
    * Token refresh and session management

* Built the exam engine with:
    * Session creation and persistence (resume capability)
    * Answer submission with auto-save
    * Cambridge band scoring algorithm for IELTS
    * TOEIC score calculation

* Created learner dashboard APIs:
    * Daily exam recommendation engine
    * Historical performance analytics
    * Mistake pattern categorization

* Developed responsive frontend pages:
    * Login/Register with Cognito integration
    * Dashboard with data visualization (React Query + charts)
    * TailwindCSS responsive layout

* Resolved cross-origin issues between Amplify-hosted frontend and API Gateway backend.

* ...
