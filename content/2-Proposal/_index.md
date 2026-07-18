---
title: "Proposal"
date: 2026-07-07
weight: 2
chapter: false
pre: " <b> 2. </b> "
---




# LingoRise — Vietnamese-first IELTS/TOEIC Preparation Platform
## An AI-Assisted, AWS Serverless Solution for English Exam Preparation

### 1. Executive Summary
LingoRise is a Vietnamese-first IELTS and TOEIC preparation platform designed to provide personalized, AI-assisted learning for Vietnamese students. The platform offers a comprehensive exam engine (IELTS Academic/General + TOEIC), a learner dashboard with daily recommendations, mistake pattern analytics, vocabulary notebook, and an admin content pipeline for efficient exam content management. The system leverages AWS serverless architecture for production-ready operations with low infrastructure costs.

### 2. Problem Statement
#### What's the Problem?
Vietnamese students preparing for IELTS/TOEIC face several challenges:
- Existing platforms are primarily English-first, creating a language barrier for beginners.
- Quality exam content is expensive and fragmented across multiple platforms.
- There is no centralized system that combines exam practice, vocabulary building, and AI-driven personalization in Vietnamese.
- Content creation for exam preparation is manual, slow, and error-prone.

#### The Solution
LingoRise addresses these issues with:
- **Vietnamese-first interface** with bilingual content for an accessible learning experience.
- **Full exam engine** supporting IELTS Academic/General and TOEIC with Cambridge band scoring, persistent resume, and per-question review.
- **AI-assisted personalization**: daily recommendations, mistake pattern analysis, and learning analytics.
- **Admin content pipeline**: DOCX/PDF/scanned-PDF import, OCR queue processing, duplicate detection, and audit logging for efficient content management.
- **Premium subscription model** with PayOS checkout integration for monetization.

### 3. Solution Architecture
The platform employs an AWS serverless architecture for scalability and cost efficiency:

![Architecture](/images/5-Workshop/1-introduction/architecture.png)

#### Tech Stack
| Component | Technology |
|-----------|-----------|
| **Frontend** | Next.js 15 (App Router, React 19, TailwindCSS, React Query, Framer Motion) |
| **Backend** | Express 4 (development) / AWS Lambda (production) — TypeScript |
| **Database** | PostgreSQL 14+ with 35 idempotent SQL migrations |
| **Authentication** | AWS Cognito with JWT verification |
| **Storage** | Amazon S3 + CloudFront (signed URLs for secure media delivery) |
| **TTS** | AWS Polly for pronunciation audio |
| **OCR** | Tesseract.js for scanned document processing |
| **AI Chatbot** | OpenRouter-compatible API integration |
| **Payment** | PayOS (Vietnamese payment provider) |
| **IaC** | AWS SAM (template.yaml with CloudFormation) |
| **Frontend Hosting** | AWS Amplify |

#### AWS Services Used
- **AWS Lambda**: Hosts the Express backend as a serverless function.
- **Amazon API Gateway**: Routes HTTP requests to the Lambda function.
- **Amazon S3**: Stores exam content assets, audio files, and imported documents.
- **Amazon CloudFront**: CDN for fast, secure content delivery with signed URLs.
- **Amazon Cognito**: User pool for authentication and role-based access (learner/admin).
- **AWS Polly**: Text-to-speech synthesis for exam audio and pronunciation.
- **Amazon SES**: Transactional email for user notifications.
- **AWS SAM/CloudFormation**: Infrastructure-as-code for repeatable deployments.

### 4. Key Features

#### Learner Features
- **Exam Engine**: Full IELTS (Academic + General) and TOEIC exam simulations with timer, persistent resume, and Cambridge band scoring.
- **Dashboard**: Daily recommendations, exam history analytics, and mistake pattern visualization.
- **Vocabulary Notebook**: Personal word bank with pronunciation audio (AWS Polly).
- **Grammar & Writing**: Structured grammar lessons and writing practice.
- **AI Chat**: AI-assisted learning companion for question explanations.
- **Course System**: Structured learning paths with progress tracking.

#### Admin Features
- **Content Pipeline**: DOCX/PDF/scanned-PDF import with automatic parsing and OCR queue.
- **Duplicate Detection**: Prevents redundant exam content.
- **Audit Log**: Full traceability of content changes.
- **Subscription Management**: Manual mark-paid for development/demo flows.
- **User Management**: Admin registration and role management.

### 5. Timeline & Milestones
| Phase | Period | Focus |
|-------|--------|-------|
| Weeks 1–4 | 20/04 – 15/05 | AWS foundations: EC2, VPC, S3, RDS, Lambda, DevOps |
| Week 5 | 18/05 – 22/05 | Project kick-off: proposal, architecture, repository |
| Weeks 6–7 | 25/05 – 05/06 | Core development: 21 API handlers, frontend UI, database |
| Week 8 | 08/06 – 12/06 | Integration, security, beta deployment |
| Week 9 | 15/06 – 19/06 | Testing and AI optimization |
| Week 10 | 22/06 – 26/06 | Handover, final report, presentation |
| Weeks 11–12 | 29/06 – 10/07 | User feedback, refinement, final submission |

### 6. Budget Estimation
#### Infrastructure Costs (Monthly)
| Service | Estimated Cost |
|---------|---------------|
| AWS Lambda | ~$0.00 (free tier) |
| Amazon S3 | ~$0.23 |
| Amazon CloudFront | ~$0.10 |
| Amazon Cognito | ~$0.00 (free tier, <50k users) |
| AWS Polly | ~$4.00 (pay-per-character) |
| Amazon SES | ~$0.10 |
| Amazon RDS (PostgreSQL) | ~$15.00 (db.t3.micro) |
| **Total** | **~$19.43/month** |

### 7. Risk Assessment
| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Cold start latency | Medium | Medium | Provisioned concurrency for critical Lambda functions |
| OCR accuracy for scanned documents | Medium | Medium | Fallback to manual review in admin pipeline |
| Payment provider instability | High | Low | Mock provider mode for development; provider-agnostic abstraction layer |
| Cost overruns | Medium | Low | AWS Budget alerts and cost monitoring via Cost Explorer |

### 8. Expected Outcomes
#### Technical Deliverables
- Production-ready IELTS/TOEIC exam platform deployed on AWS.
- 21 backend API handler modules with comprehensive test coverage.
- Admin content pipeline supporting DOCX, PDF, and scanned-PDF import.
- All 6 CI/CD verification commands passing green.

#### Learning Outcomes
- Hands-on experience with 8+ AWS services in a production context.
- Full-stack development skills with modern frameworks (Next.js 15, Express, TypeScript).
- DevOps practices: CI/CD, infrastructure-as-code (SAM/CloudFormation), monitoring.
- Project management: architecture design, documentation, and team coordination.