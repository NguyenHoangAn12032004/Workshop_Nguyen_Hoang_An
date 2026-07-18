---
title: "Week 5 Worklog"
date: 2026-05-18
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---





### Week 5 Objectives:

* Kick off the LingoRise project: write proposal, design architecture, and set up the repository.
* Finalize technology decisions and define the AWS service stack.

### Tasks carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | ---- | ---------- | --------------- | ------------------ |
| Mon | - Research existing IELTS/TOEIC platforms to identify gaps <br> - Draft project proposal with problem statement and target users | 18/05/2026 | 18/05/2026 | |
| Tue | - Design the system architecture: <br>&emsp; + Define AWS services: Lambda, API Gateway, S3, CloudFront, Cognito, RDS, Polly <br>&emsp; + Draw the architecture diagram | 19/05/2026 | 19/05/2026 | |
| Wed | - Initialize the monorepo: <br>&emsp; + `lingorise-app` (Next.js 15 frontend) <br>&emsp; + `lingorise-backend` (Express + TypeScript) <br>&emsp; + Configure ESLint, Prettier, TypeScript | 20/05/2026 | 20/05/2026 | |
| Thu | - Set up AWS SAM `template.yaml`: <br>&emsp; + Lambda function definition <br>&emsp; + API Gateway proxy routing <br>&emsp; + 30+ parameters for flexible configuration <br> - Design initial PostgreSQL database schema | 21/05/2026 | 21/05/2026 | |
| Fri | - Create the first database migrations (schema foundation) <br> - Set up Cognito User Pool for authentication <br> - Test basic `sam build && sam deploy` workflow | 22/05/2026 | 22/05/2026 | |

### Week 5 Achievements:

* Completed project proposal with:
    * Problem statement: gap in Vietnamese-language IELTS/TOEIC platforms
    * Solution architecture: AWS serverless stack
    * Budget estimation: ~$19.43/month
    * Timeline: 8-week implementation plan

* Designed and documented the full system architecture:
    * Frontend: Next.js 15 with App Router
    * Backend: Express.js on Lambda via `serverless-http`
    * Database: PostgreSQL 14+ on RDS
    * Storage: S3 + CloudFront with signed URLs
    * Auth: Cognito with JWT-based role authorization

* Initialized the monorepo with proper tooling:
    * TypeScript configuration for both frontend and backend
    * ESLint + Prettier for code quality
    * Git repository with branching strategy

* Created the SAM template with 30+ configurable parameters for multi-environment deployment.

* Designed the initial database schema with foundation tables (users, exams, questions).

* ...
