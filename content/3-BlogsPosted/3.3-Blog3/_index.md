---
title: "Blog 3: Deploying with SAM and Amplify"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Deploying a Full-Stack Next.js + Express Application on AWS with SAM and Amplify

<!-- Replace with your actual blog post link -->
**Published on:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Summary

This blog post is a practical deployment guide for running a full-stack TypeScript application on AWS using SAM (Serverless Application Model) for the backend and Amplify for the frontend — the exact deployment strategy used by LingoRise.

## Key Topics Covered

### Deployment Architecture
```
┌──────────────────┐     ┌──────────────────┐
│   AWS Amplify    │     │  API Gateway     │
│   (Next.js 15)   │────▶│  + Lambda        │
│   Frontend       │     │  (Express.js)    │
└──────────────────┘     └───────┬──────────┘
                                 │
                    ┌────────────┼────────────┐
                    │            │            │
              ┌─────▼──┐  ┌─────▼──┐  ┌──────▼─────┐
              │  RDS   │  │   S3   │  │ Cognito    │
              │ (PG)   │  │+CF CDN │  │ User Pool  │
              └────────┘  └────────┘  └────────────┘
```

### Backend: SAM Template Deep-Dive
The `template.yaml` defines all backend infrastructure:
- **Lambda function** with Node.js 24.x runtime, 256MB memory, 10s timeout.
- **Environment variables** for database, Cognito, S3, payment, and AI service configuration.
- **API Gateway** with proxy routing to the Express app.
- Over **30 parameters** for flexible configuration across environments.

### Frontend: Amplify Deployment
- Next.js 15 with App Router deployed via `amplify.yml` build spec.
- Environment variables injected at build time for API URL configuration.
- Automatic builds triggered by Git pushes.

### Database: Migration Strategy
- 35 idempotent SQL migrations managed by a custom TypeScript runner.
- Each migration runs inside a transaction for atomicity.
- Safe to re-run: migrations are tracked in a `schema_migrations` table.

### Deployment Commands
```bash
# Backend deploy
cd lingorise-backend
sam build && sam deploy --guided

# Frontend deploy (via Amplify Console or CLI)
# Configured in amplify.yml

# Database migration (run against production RDS)
DATABASE_URL=<prod-url> npm run migrate
```

### Lessons Learned
1. SAM's `template.yaml` with 30+ parameters needs careful organization — group related params with metadata.
2. Lambda's 10-second timeout is sufficient for API calls but not OCR — use a separate worker architecture.
3. Amplify's Next.js support has improved significantly; App Router works with minimal configuration.
4. Always test migrations on a staging database before production — even "idempotent" migrations can surprise you.