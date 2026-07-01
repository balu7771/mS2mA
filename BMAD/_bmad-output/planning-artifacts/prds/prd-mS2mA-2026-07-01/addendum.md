# PRD Addendum — AI Transformation Proposal Generator

*Depth that belongs in downstream documents (architecture, UX spec, solution design) or earned a place but does not fit the PRD itself. Technical-how decisions, options-considered matrices, mechanism choices, and detailed implementation data live here.*

---

## A1. Technology Stack (Architecture Decision — do not repeat in PRD)

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Frontend | Next.js 15+ / TypeScript | SSR, RSC, SEO, industry standard |
| UI Components | Shadcn/UI + Tailwind CSS | Premium accessible components |
| Backend API | FastAPI (Python 3.12+) | Async-native, AI-native, OpenAPI auto-gen |
| Agent Framework | LangGraph | Production multi-agent orchestration (chosen over CrewAI, AutoGen) |
| LLM SDK | LangChain + Boto3 | Unified LLM interface + Bedrock access |
| Primary Database | PostgreSQL 16 + pgvector | ACID, RLS, JSON, vector search in one DB |
| Cache | Redis 7 (ElastiCache) | Session, rate limiting, result cache |
| Message Queue | Amazon SQS | Async agent job processing |
| Workflow Engine | Temporal.io Cloud (MVP) | Durable execution for long-running AI workflows |
| Search | OpenSearch 2.x | BM25 + k-NN hybrid search, log analytics |
| Knowledge Graph | Neo4j 5.x AuraDB (v2) | Application dependency graph |
| Object Storage | Amazon S3 | Documents, generated artifacts, templates |
| Auth | Auth0 | OIDC + SAML + SCIM — all enterprise IdP patterns |
| AI Gateway | Amazon Bedrock | Multi-model, VPC-isolated LLM access |
| Container Runtime | AWS ECS Fargate | Serverless containers |
| IaC | Terraform 1.8+ | Cloud-agnostic state management |
| CI/CD | GitHub Actions | Workflow automation |
| Observability | OpenTelemetry + Langfuse + Datadog | Full-stack + AI-specific monitoring |
| Doc Generation | python-pptx, python-docx, WeasyPrint | Enterprise proposal artifacts |

---

## A2. AI Agent Architecture — Options Considered

| Framework | Verdict | Reason |
|-----------|---------|--------|
| LangGraph | **PRIMARY CHOICE** | Deterministic state machine, checkpointing, HITL built-in, enterprise production standard 2026 |
| CrewAI | Consider for sub-teams (v2) | Role-based, good for rapid prototyping |
| AutoGen (AG2) | Skip for MVP | Better for research/open-ended tasks |
| Semantic Kernel | Skip | .NET/Azure-centric; wrong language for this stack |

**Why Temporal.io over Airflow:**
Temporal provides *durable execution* — an agent workflow crashes at minute 43 of a 45-minute run, it resumes from that exact point. Airflow is DAG-based batch scheduling — wrong paradigm for event-driven AI agent workflows.

---

## A3. Model Routing Rules (Initial — to be tuned during MVP)

| Task | Model | Rationale |
|------|-------|-----------|
| Routing / classification / intent detection | Claude Haiku 3.5 | $0.25/M tokens; low-latency, high-volume |
| Application analysis, tech debt scoring, cloud readiness | Claude Sonnet 4 | $3/M tokens; best reasoning/cost balance |
| Complex multi-hop architectural reasoning, synthesis | Claude Sonnet 4 | Primary reasoning model |
| Deep analysis when Sonnet is insufficient | Claude Opus 4 | $15/M tokens; reserved for complex reasoning only |
| Embedding generation | Amazon Titan Embed v2 | AWS-native, enterprise-grade |
| Secondary/fallback LLM | GPT-4o | Model redundancy if Bedrock degrades |

Expected cost savings vs. Opus-for-everything: 40–60%.

---

## A4. Pricing Tiers (Mechanism — not in PRD)

```
Tier 1 — Starter ($3,000/month)
├── Up to 50 applications assessed
├── Up to 5 full proposals/year
└── Overage: $500/assessment, $1,000/proposal

Tier 2 — Professional ($12,000/month)
├── Up to 250 applications
├── Up to 20 proposals/year
├── Up to 10 integrations
└── Overage: $300/assessment, $750/proposal

Tier 3 — Enterprise ($30,000+/month, custom)
├── Unlimited applications and proposals
├── Unlimited integrations
├── 99.9% SLA + dedicated CSM
└── Options: dedicated tenant, EU data residency, on-prem

Consulting Partner Program
├── 40% platform discount from list price
├── White-label: custom branding + domain (v2+)
└── Revenue share: 10% of referred client spend
```

---

## A5. Infrastructure Cost Estimate (MVP, 50 tenants)

| Service | Monthly Cost |
|---------|-------------|
| ECS Fargate (API) 2 vCPU/4GB × 3 tasks | $150 |
| ECS Fargate (Agents) 4 vCPU/8GB × 5 tasks | $500 |
| RDS Aurora PostgreSQL db.r7g.large, Multi-AZ | $400 |
| ElastiCache Redis cache.r7g.medium | $100 |
| OpenSearch 2 × r6g.large.search | $300 |
| Temporal.io Cloud ~50 workflows/day | $200 |
| Amazon Bedrock (Claude Sonnet) ~2M tokens/day | $600 |
| Amazon S3 5TB storage | $120 |
| CloudFront 2TB transfer | $180 |
| SQS, EventBridge, Secrets Manager | $50 |
| Auth0 Professional (1K MAU) | $240 |
| Langfuse Cloud | $100 |
| **Total MVP** | **~$2,940/month** |

At 1,000 tenants and $12K ARR/tenant avg: $12M ARR vs. ~$295K/year infrastructure = **~97% gross margin (infrastructure only)**.

---

## A6. Build vs. Buy Summary

| Component | Recommendation | Rationale |
|-----------|---------------|-----------|
| AI Agent orchestration | Build with LangGraph | LangGraph is a library; we build the workflows |
| LLM access | Buy — Amazon Bedrock | Managed, secure, multi-model, VPC-isolated |
| Authentication | Buy — Auth0 | SAML + OIDC + SCIM takes 6+ months to build correctly |
| Vector DB (MVP) | Use — pgvector bundled | No additional service; sufficient for MVP |
| Workflow engine | Buy — Temporal Cloud | Managed durable execution; lower startup burden |
| Knowledge graph | Buy — Neo4j AuraDB (v2) | Managed; no operational overhead |
| Document generation | Build — python-pptx/docx | Custom branded templates are core IP |
| Search | Buy — AWS OpenSearch | Managed; no ops overhead |
| Frontend | Build — Next.js | Custom UX is competitive differentiator |
| Data connectors | Build — MCP servers | Enterprise connectors are core IP and moat |
| Compliance monitoring | Buy — Vanta/Drata | SOC2 automation = faster certification |

**Build-to-Buy Ratio: 40% Build / 60% Managed Services**

---

## A7. Rejected Alternatives

**Why not a pure API-key-based GTM (no UI)?**
The Proposal Package quality (PPT/PDF) is the primary value artefact. Without a UI, Consultants cannot run assessments, configure connectors, or review findings. An API-only product would require customers to build their own frontend — impractical for the target consulting firm persona.

**Why not ServiceNow or LeanIX as the first connector?**
Both require significant procurement cycles for access credentials from enterprise clients. GitHub Enterprise and Jira are widely accessible, lower-security-sensitivity, and provide rich data for demonstrating AI analysis quality. Manual upload ensures the demo can always run even without live connector credentials.

**Why not EU-first go-to-market?**
EU AI Act compliance requirements and GDPR complexity add significant legal and infrastructure overhead for a solo founder in pre-validation stage. US-first allows faster iteration; EU market entry is structured for Phase 3.

---

## A8. GTM Validation Strategy (Pre-Revenue)

Since there is no pilot customer yet, validation should proceed in parallel with MVP build:

1. **Demo dataset:** Synthesise a realistic enterprise dataset (150 applications, GitHub repos, Jira project) that can be used for demos without requiring real customer credentials.
2. **LinkedIn outreach:** Target GCC transformation heads, consulting firm partners, and enterprise CTOs who have publicly posted about AI transformation challenges.
3. **Technical blog:** Publish architecture/methodology content that demonstrates credibility and attracts inbound interest from technically sophisticated buyers.
4. **Conference/hackathon entry:** Enter AI/enterprise startup competitions — these provide structured feedback and potential investor visibility.
5. **Design partner programme:** Offer 3 design partner spots at 50% discount in exchange for weekly feedback sessions and a case study on go-live.
