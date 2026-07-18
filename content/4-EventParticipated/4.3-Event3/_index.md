---
title: "Event 3: AWS Vietnam Community Day 2026"
date: 2026-07-07
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

# AWS Vietnam Community Day 2026

### Event Details
| | |
|---|---|
| **Event Name** | AWS Vietnam Community Day 2026 |
| **Date** | 23/05/2026 |
| **Location** | Ho Chi Minh City, Vietnam |
| **Organizer** | AWS User Group Vietnam |
| **Role** | Listener & Learner |

### Event Description
AWS Community Day is a community-led conference designed to share technical AWS insights, architectural practices, and emerging industry trends. This event featured deep-dive technical talks and hands-on workshops covering Cloud infrastructure, DevOps, and Generative AI.

### Main Content
The event hosted several specialized sharing sessions by community leaders:

1. **Enterprise-Grade Multi-Agent System for Startup Credit Scoring** (*Speaker: Vy Lam - Senior Business Systems Analyst @ VPBank*)
   - Discussed why traditional credit scoring fails for startups due to data mismatches.
   - Outlined how a collaborative Multi-Agent System (Manager, Financial, Market, Team, Risk, Compliance agents) can perform credit assessment with proper checks and balances.
   - Highlighted enterprise-grade AI pillars: Security (Cognito/OAuth2), Guardrails (Bedrock Guardrails), Network Isolation (VPC), Operations, and Infrastructure as Code (Terraform).

2. **Non-Determinism of "Deterministic" LLM Settings** (*Speaker: Duc Dao - Solution Architect @ Cloud Kinetics*)
   - Explained that setting Temperature T=0 does not guarantee fully reproducible outputs due to GPU floating-point non-associativity and commercial API dynamic batching.
   - Suggested mitigations: structured outputs (JSON/functions), majority voting ensembles, and using T=0.1 as a practical sweet spot to avoid response loops.

3. **Context Is Everything: Making AI Actually Work for You** (*Speaker: Tinh Truong - Platform Engineer @ GoTymeX*)
   - Outlined common mistakes in working with LLMs, such as prompt redundancy and dumping irrelevant data ("Internet Puller").
   - Explained the shift from simple prompting to context and memory-centric systems (S3/Vector DB + Bedrock) and building a "Second AI Brain".
   - Reviewed typical AWS CI/CD deployment pipelines (CodePipeline, CodeBuild, ECR, ECS).

4. **Friendly AI Assistant w/ Amazon Quick** (*Speaker: Pham Ng Hai Anh - AWS Community Builder @ G-AsiaPacific*)
   - Demonstrated how the Amazon Quick suite (Amazon Q Business) resolves business user challenges.
   - Shared a use case of a PM Assistant automating Minutes of Meetings (MoM), sending emails, and scheduling events.

5. **36 Hours with LotusHacks: Building UTMorpho from Idea to Reality** (*Speaker: Team VIB*)
   - Shared their hackathon experience building UTMorpho, an agentic WYSIWYG editor for AI-generated UI canvas components, in 36 hours.
   - Emphasized that real frustrations create real ideas, team chemistry beats individual skills, and AI should be treated as a teammate.

6. **From Edge to Origin: CloudFront as Your Foundation** (*Speaker: Nguyen Tuan Thinh - DevOps Engineer*)
   - Highlighted AWS's new Flat-rate pricing for CloudFront, which bundles WAF, DDoS protection, Route 53, and S3 credits to eliminate unexpected bill spikes.
   - Showed how CloudFront offloads origin servers (reducing EC2 CPU from 5% to 1% by handling compression and TLS handshakes) and explained security features like VPC origins, OAC, mTLS, and custom headers.

### Lessons Learned & Personal Contribution
As a beginner exploring cloud engineering for the first time, this massive community day was eye-opening:
- **Cloud Architecture Value:** Hearing about CloudFront's origin offloading and request collapsing gave me a concrete understanding of how cloud design impacts system performance and costs.
- **AI as a Structured System:** I learned that working with AI is not just about writing sentences (prompts), but about building rigorous systems with guardrails, strict context, and structured outputs.
- **Inspirational Journeys:** The stories of Team VIB building a product in 36 hours under immense pressure, and Nguyen Tuan Thinh progressing from a beginner to a DevOps engineer, showed me that dedication and curiosity can lead to incredible results.
- **Exploring Motivations:** Hearing these experts share their real-world struggles and solutions made me realize how exciting and impactful cloud engineering is. It has greatly motivated me to keep learning.

### Event Photos
![AWS Community Day](/images/4-EventParticipated/event3-community-day-1.png)
![Context Is Everything Session](/images/4-EventParticipated/event3-community-day-2.png)