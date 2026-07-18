---
title: "Blog 1: Serverless IELTS/TOEIC Architecture"
date: 2026-07-07
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Building a Serverless IELTS/TOEIC Platform on AWS — Architecture Deep-Dive

<!-- Replace with your actual blog post link -->
**Published on:** [AWS Study Group](https://cloudjourney.awsstudygroup.com/)

## Summary

This blog post walks through the architecture decisions behind **LingoRise**, a Vietnamese-first IELTS/TOEIC preparation platform built entirely on AWS serverless services.

## Key Topics Covered

### Why Serverless?
- Cost efficiency: pay-per-request pricing makes it viable for an education startup.
- Automatic scaling: exam season spikes handled without provisioning.
- Reduced operational overhead: no servers to patch or maintain.

### Architecture Overview
The system consists of:
- **AWS Lambda** running an Express.js backend via `serverless-http`, handling 21 API handler groups (auth, exams, vocabulary, grammar, courses, dashboard, payments, admin, content, speaking, writing, chat, etc.).
- **Amazon API Gateway** as the HTTPS entry point with custom domain mapping.
- **Amazon S3 + CloudFront** for asset storage with signed URLs for secure, time-limited access.
- **Amazon Cognito** for user authentication with JWT-based role authorization (learner/admin).
- **PostgreSQL on RDS** as the primary relational database with 35 idempotent migrations.
- **AWS Polly** for text-to-speech audio generation for exam listening sections and vocabulary pronunciation.

### Challenges & Solutions
| Challenge | Solution |
|-----------|----------|
| Lambda cold starts on exam API | Provisioned concurrency for critical exam endpoints |
| Large Express app on Lambda | Careful route splitting and lazy loading |
| Secure media delivery | CloudFront signed URLs with configurable TTL |
| Vietnamese TTS quality | AWS Polly with Neural voice engine |

### Lessons Learned
1. Express on Lambda works surprisingly well with `serverless-http`, but you need to watch memory and timeout limits.
2. SAM template management becomes complex at 20+ resources — consider nested stacks for large projects.
3. Cognito's hosted UI is limited — building custom auth flows gives better UX control.

<!-- Replace with screenshot of your architecture diagram -->
<!-- ![LingoRise Architecture](/images/3-BlogsPosted/blog1-architecture.png) -->