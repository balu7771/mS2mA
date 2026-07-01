---
title: AI Transformation Proposal Generator
status: draft
created: 2026-07-01
updated: 2026-07-01 (rev 2 — GTM brainstorm incorporated)
---

# PRD: AI Transformation Proposal Generator

## 0. Document Purpose

This PRD defines the product requirements for the **AI Transformation Proposal Generator** — an AI-native enterprise SaaS platform that automates the creation of consultant-quality AI transformation proposals. It is written for Balu (founder, product owner), downstream workflow owners (UX, architecture, engineering), and any future investors or design partners reviewing the product vision.

The document uses Glossary-anchored vocabulary throughout. Features are grouped by capability area with Functional Requirements (FR-1 through FR-N) nested and globally numbered for stable downstream referencing. Assumptions are tagged `[ASSUMPTION]` inline and indexed in §15. Technical architecture decisions (stack choices, infrastructure diagrams, data models, pricing tier details) live in `addendum.md` — this PRD captures *what* the product does, not *how* it is built.

**Primary inputs:**
- [technical-ai-transformation-proposal-generator-research-2026-06-26.md](../../research/technical-ai-transformation-proposal-generator-research-2026-06-26.md) — authoritative for market data, competitive landscape, and API reference.
- [brainstorm-ai-proposal-generator-gtm-2026-06-28/.memlog.md](../../../brainstorming/brainstorm-ai-proposal-generator-gtm-2026-06-28/.memlog.md) — GTM strategy brainstorm (Creative Partner mode); introduces the dual product identity, TechD validation path, ANSR internal sales weapon framing, and output-as-marketing mechanic.

---

## 1. Vision

Large enterprises spend four to twelve weeks and $30,000–$500,000 per engagement manually producing AI transformation proposals. Senior consultants gather data from ten or more systems — enterprise architecture tools, code repositories, cloud platforms, ITSM, project trackers — synthesise it into insights, and then write executive summaries, AI roadmaps, ROI models, and architecture diagrams. The process is slow, expensive, inconsistent, and locked inside individual consultant heads.

The **AI Transformation Proposal Generator** breaks that model. It is an AI-native SaaS platform that connects to a client's enterprise data sources, ingests their application portfolio, codebases, architecture documentation, and operational data, then runs a suite of specialised AI agents to analyse, synthesise, and produce a consultant-quality proposal package — PowerPoint deck, executive summary, AI opportunity roadmap, and ROI model — in hours instead of weeks.

The platform does not replace consultants. It supercharges them. A consultant who today can run two engagements per quarter can run twenty. A Global Capability Center that commissions four proposals per year can commission forty. The proposal is no longer a one-time artefact; it becomes a living document that refreshes automatically as the enterprise data changes.

The platform carries a **dual identity** that its product architecture must support simultaneously:

1. **Standalone multi-tenant SaaS** — sold to consulting firms, GCCs, and enterprise transformation teams as a subscription product. Revenue is the engine; the connector ecosystem and proposal quality are the moat.

2. **Internal sales weapon** — used by a GCC setup firm (or any advisory firm) *live, in front of prospects*, to generate an AI transformation proposal on the spot during a sales meeting. The platform becomes the credibility signal that closes deals. This mode demands instant wow-factor: connect a client's GitHub, trigger an assessment, and show a scored application heatmap before the prospect asks their second question.

These two identities are not in conflict. Every internal sales deployment is simultaneously a live product demo to the next paying customer. The platform's long-term ambition is to become the intelligence layer that every consulting firm, GCC, and enterprise transformation team runs before any major AI or modernisation investment decision.

---

## 2. Why Now

Four forces converge in 2026 to make this the right moment:

1. **AI consulting demand is exploding.** The global AI consulting services market is $11.07 billion (2025) growing at 23–34% CAGR. Every Global 2000 CEO has an AI transformation mandate and a budget. Demand for proposals far outstrips the supply of qualified consultants.

2. **The tooling to build this now exists.** LangGraph (production multi-agent orchestration), MCP (standardised agent-to-tool protocol), and Amazon Bedrock (enterprise-grade multi-model LLM access with VPC isolation) reached production maturity in 2025–2026. Hybrid RAG with pgvector and OpenSearch is proven at scale. None of this existed at this quality level two years ago.

3. **The gap is genuine and uncontested.** LeanIX, ServiceNow, Ardoq, CAST Highlight — every competitor requires human consultants to use them and manually generate insights. No product today automatically generates an enterprise-grade AI transformation proposal from multi-source data ingestion. This is a greenfield category.

4. **A concrete near-term validation path exists.** The platform's founder has direct access to two real validation targets before any cold outreach is needed: (a) **ANSR** — a GCC setup firm whose CEO closes deals using story-telling and whose engineering team is underutilised; the platform immediately becomes their AI sales weapon for prospect pitches, opening a new service category (AI Capability Center setup, not just GCC setup); (b) **TechD** — a US-based product company that identifies tech stack gaps (Mainframe to Agentify) in enterprise client environments; the platform is the natural upstream complement — TechD finds the gaps, this platform generates the full AI transformation proposal. This founder-accessible validation pipeline compresses the time to first real customer from months to weeks.

---

## 3. Target Users

### 3.1 Jobs To Be Done

**Consultant / Analyst (primary operator of the platform):**
- Deliver high-quality AI transformation proposals faster without scaling headcount proportionally.
- Reduce the data-gathering and synthesis grind from weeks to hours so senior time is spent on strategy and client relationship, not copy-paste.
- Produce consistent, repeatable proposal quality regardless of which analyst runs the engagement.
- Win more proposals by responding to RFPs and executive requests in days, not months.
- Justify the engagement before it starts — show the client a data-grounded first draft to earn trust.

**Enterprise Client / CTO / CISO (recipient and approver of the proposal):**
- Receive an objective, data-grounded view of their AI readiness — not a slide deck built on interviews alone.
- Understand concretely which applications to modernise, which to retire, where AI investment will yield ROI, and what the migration journey looks like.
- Get a proposal they can share with the board without embarrassment — executive-grade language, charts, and branded presentation.

**GCC Setup / Advisory Firm CEO (internal sales weapon user):** [ASSUMPTION: This archetype describes the ANSR / TechD-style GTM partner use case; it overlaps with Consultant but has a distinct primary job]
- Use the platform *live in front of a prospect* during a sales meeting to generate a scored AI readiness snapshot of the prospect's portfolio — creating immediate credibility and a reason to engage further.
- Win GCC and AI Capability Center (ACC) setup contracts by demonstrating data-grounded AI transformation insight before the competitor has even filed their proposal.
- Expand the firm's service catalogue from pure infrastructure setup (workspace, talent) to AI transformation advisory — a higher-margin, higher-stickiness offer.

**Platform Admin (Balu / internal ops in MVP):** [ASSUMPTION: In MVP, Balu operates the platform directly; multi-tenant self-service admin is a v2 feature]
- Onboard new customer tenants, configure integrations, and monitor platform health.
- Track token usage and costs per tenant against tier limits.

### 3.2 Non-Users (v1)

- **Enterprise employees without consultant or executive roles** — end users of the client's systems are not users of this platform; the platform reads their data but they never see the interface.
- **Developers inside the client's engineering team** — the platform reads their repositories and Jira boards but they are not actors in the platform.
- **Independent freelance consultants with no institutional backing** — the platform's pricing and security posture targets teams and firms, not individual practitioners in MVP. [ASSUMPTION: freelance/solo tier may be added in v2 if demand warrants]

### 3.3 Key User Journeys

---

**UJ-1. Priya sets up her first assessment project for a new client.**

- **Persona + context:** Priya, a Senior Manager at a mid-size consulting firm, has just won a discovery phase engagement with a Global 500 bank. She has 10 days to produce a preliminary AI readiness report.
- **Entry state:** Authenticated. On the Projects dashboard. Client has just provisioned access credentials for their GitHub Enterprise and Jira.
- **Path:**
  1. Priya clicks **New Project**, names it "HDFC AI Readiness Q3-2026", sets scope to include 150 applications.
  2. She navigates to **Integrations** and connects GitHub Enterprise (OAuth App flow, 3 clicks) and Jira Cloud (OAuth 2.0 3LO, redirect).
  3. The platform immediately begins an incremental sync and shows a **connection health card** per source — green, amber, or red.
  4. She triggers a **manual document upload** — an Excel application inventory and two architecture PDFs from the client.
  5. She clicks **Start Assessment** and selects assessment scope (all connected sources). The system confirms estimated run time: ~2 hours.
- **Climax:** Priya sees the assessment status screen update in real time — "Discovery: 43 applications found", "Analysis: Architecture Agent running". She closes her laptop and checks back after lunch.
- **Resolution:** Priya receives an email notification: "Assessment complete — 3 findings require your review before proposal generation." She logs back in to review at Gate 2.
- **Edge case:** One connector (GitHub) returns a rate-limit error mid-run. The platform logs the partial sync, marks that source as "incomplete — 23 of 150 repos analysed", and continues. Priya sees a warning banner; the proposal is still generated with a gap note.

---

**UJ-2. Priya reviews AI findings at the HITL gate and approves the proposal.**

- **Persona + context:** Priya has returned to the platform after the assessment run. She is at HITL Gate 2 (post-analysis).
- **Entry state:** Authenticated. On the Findings Review screen. 12 AI-generated findings are waiting; 3 are flagged for mandatory review (security findings).
- **Path:**
  1. Priya sees a **finding card** for each application: cloud readiness score, technical debt score, AI readiness tier, and a 2-sentence rationale with source citations.
  2. She clicks the first security finding — the platform shows the raw data it used (CVE count from GitHub dependency scan, incident rate from Jira).
  3. Priya overrides one score: bumps "Core Banking App" from Migrate to Invest, adds a comment "client confirmed strategic asset — not for migration."
  4. She approves remaining findings. Clicks **Generate Proposal**.
  5. The platform runs the Proposal Writer and Diagram agents in parallel (~30 min).
- **Climax:** Priya downloads a zip: `HDFC-AI-Readiness-Proposal.pptx`, `Executive-Summary.pdf`, `AI-Roadmap.docx`. She opens the PPT — 24 slides, branded, with charts populated from the AI findings. The quality is presentation-ready.
- **Resolution:** Priya makes two tweaks in PowerPoint (her firm's disclaimer page, one slide reorder), then emails the package to the client's CTO. Total time from project creation to delivery: 4 hours.

---

**UJ-3. Rajesh (CTO) receives and navigates the proposal package.**

- **Persona + context:** Rajesh, CTO of a Global 500 bank, receives an email with a secure download link to the proposal package.
- **Entry state:** Not logged into the platform. Opens the email on mobile. [ASSUMPTION: Client delivery portal is a v2 feature; v1 delivers via time-limited S3 signed URL]
- **Path:**
  1. Rajesh downloads the zip and opens the PDF executive summary — 2 pages, board-ready language: Current State → AI Readiness Score → Top 5 Opportunities → Recommended Next Steps.
  2. He opens the PPTX: slide 1 is the overall AI Readiness Score (7.2/10) with a traffic-light portfolio heatmap. Slides 3–8 walk through the top modernisation candidates with ROI estimates.
  3. He forwards the PPTX to his CISO and CFO with a note: "Worth a conversation with Priya."
- **Climax:** Rajesh sees concrete, data-backed recommendations with source citations (e.g. "Based on GitHub analysis: 34 of 150 repositories use frameworks past EOL"). He trusts it more than a pure interview-based report.
- **Resolution:** Rajesh's PA schedules a meeting with Priya for the following week. The proposal becomes the agenda for that meeting.

---

**UJ-4. Platform admin onboards a new customer tenant.** [ASSUMPTION: Manual admin onboarding in MVP; self-serve tenant signup is v2]

- **Persona + context:** Balu (platform admin) receives a signed agreement from a new customer firm.
- **Entry state:** Logged into admin panel.
- **Path:**
  1. Creates a new Tenant record: org name, slug, tier (Professional), contact email.
  2. Configures token budget ceiling and rate limits for the tenant.
  3. Sends an invite email to the customer's designated Org Admin.
  4. Customer Org Admin clicks the invite link, sets password, and sees an empty Projects dashboard.
- **Climax:** Customer is live and independently able to start their first project.
- **Resolution:** Balu sees the new tenant in the admin dashboard with status "Active". Token usage counter starts at 0.

---

**UJ-5. Arjun (GCC Setup Firm CEO) runs a live assessment in front of a prospect.** [ASSUMPTION: This journey requires a pre-warmed demo project or fast-connect mode; live assessment run time must be < 30 min for a capped 20-application scope to work in a sales meeting]

- **Persona + context:** Arjun, CEO of a GCC setup firm (think ANSR-profile), is in a 90-minute discovery call with the CTO of a US-listed company exploring a GCC in India. Arjun wants to differentiate from the three other GCC setup firms on the shortlist by making the meeting unforgettable.
- **Entry state:** Authenticated on a mobile-connected laptop. The prospect has just verbally agreed to share their GitHub organisation read access (OAuth in 60 seconds).
- **Path:**
  1. Arjun opens the platform, creates a new Project: "[Prospect Co] ACC Assessment — Live Demo", scope capped at 20 applications.
  2. Connects GitHub (OAuth, 3 clicks — prospect watches and approves on their own laptop).
  3. Clicks **Start Assessment — Express Mode** (20-application scope, 4 agents, no Gate 1). Estimated run time: 18 minutes.
  4. While the assessment runs, Arjun talks the CTO through what the agents are doing — "Right now, the Architecture Agent is scanning your 20 most active repositories for cloud readiness and tech debt signals."
  5. At Gate 2, Arjun quickly approves all Findings (2 minutes), clicks **Generate Proposal**.
  6. 12 minutes later: Executive Summary PDF and Application Heatmap PNG appear. Arjun screen-shares them.
- **Climax:** The prospect CTO sees their own portfolio scored live — 7 applications flagged for modernisation, 3 AI-ready, 2 marked Eliminate. The heatmap is credible because it came from their actual codebase, not a deck Arjun prepared.
- **Resolution:** The CTO says: "I'd like Priya to see this. Can you send me that PDF?" Arjun clicks **Share** from the platform — the prospect receives a time-limited download link with an "AI Transformation Proposal powered by [Platform]" attribution in the footer. The platform watermark becomes the conversation topic at the next meeting.
- **Edge case:** GitHub rate limits prevent full 20-repo sync in the meeting window. Express Mode caps to the 10 most-recently-active repos and labels the output clearly: "Based on 10 of 20 repositories — full analysis available post-meeting."

---

## 4. Glossary

- **Assessment** — A complete end-to-end run of the AI agent pipeline against one or more connected data sources for a specific Project. An Assessment produces Findings and, upon approval, a Proposal Package. A Project may have multiple Assessment versions over time.
- **Connector** — A platform-managed integration adapter that authenticates to an enterprise data source (e.g. GitHub, Jira, ServiceNow) and extracts normalised application and infrastructure data. Connectors are read-only by default; no data is written back to source systems in v1.
- **Finding** — A single AI-generated insight about a specific application or the enterprise portfolio as a whole, produced during an Assessment. Each Finding has a type (e.g. Cloud Readiness, Technical Debt, Security Risk), a score, a confidence level, a rationale, and source citations.
- **HITL Gate** — A mandatory or optional Human-in-the-Loop checkpoint in the Assessment workflow where a Consultant reviews, approves, annotates, or overrides AI-generated Findings before the workflow proceeds. Three gates exist: post-Discovery (Gate 1, optional), post-Analysis (Gate 2, mandatory), and pre-Delivery (Gate 3, mandatory).
- **Knowledge Base** — The per-tenant store of ingested and processed documents, embeddings, and graph relationships derived from Connector data and manual uploads. The Knowledge Base powers the hybrid RAG retrieval that informs AI agent reasoning.
- **MCP Server** — A Model Context Protocol server that wraps a single enterprise data source and exposes standardised tool primitives to AI agents. Each Connector is backed by one MCP Server.
- **Proposal Package** — The complete set of output artefacts generated by the platform for a completed Assessment: an executive summary (PDF), a detailed recommendations report (DOCX), and an application portfolio heatmap (PNG) in MVP; plus a full presentation deck (PPTX) and ROI model (XLSX) in v2.
- **Project** — The top-level work unit in the platform. A Project represents one client engagement scope: it has a name, a set of connected data sources, one or more Assessments, and zero or more delivered Proposal Packages.
- **Tenant** — A single customer organisation using the platform. Tenant data is strictly isolated; no data crosses Tenant boundaries. A Tenant has Users, Projects, Connectors, and a billing tier.
- **TIME Classification** — The four-value taxonomy used to categorise each assessed application: **Tolerate** (keep as-is), **Invest** (modernise), **Migrate** (move to cloud/SaaS), **Eliminate** (decommission).
- **Token Budget** — The per-Tenant monthly allocation of LLM inference tokens. Exceeding the budget triggers throttling warnings; hard limits are configurable per tier.
- **User** — A named individual within a Tenant who has access to the platform. Users have one of four roles: Org Admin, Project Owner, Analyst, or Reviewer.

---

## 5. Features

### 5.1 Tenant & Project Management

**Description:** The platform's entry surface — Tenants are provisioned, Users are managed, and Projects are created here. This feature governs identity, access control, and the lifecycle of every piece of work the platform produces. Access is Role-Based. [ASSUMPTION: SSO via SAML/OIDC is a v2 feature; v1 uses email/password with MFA required for Org Admin accounts]

**Roles:**
- **Org Admin** — manages Users, Connectors, Tenant settings, and billing data.
- **Project Owner** — creates and owns Projects; can trigger Assessments and approve Findings at all HITL Gates.
- **Analyst** — can create Projects and run Assessments; can review Findings but cannot approve at Gate 3 (pre-Delivery) unless also a Project Owner.
- **Reviewer** — read-only + can add comments to Findings; cannot trigger Assessments or generate Proposals.

**Functional Requirements:**

#### FR-1: Project creation and scope definition
A Project Owner or Analyst can create a new Project by providing a project name, an optional client name, and a scope limit (maximum number of applications to analyse). The system assigns the Project a unique ID and initialises it in `draft` status.

**Consequences (testable):**
- Creating a Project with a duplicate name within the same Tenant returns a validation error.
- A Project is not visible to Users in other Tenants under any circumstances.
- The system records `created_by`, `created_at`, and the scope limit on the Project record.

#### FR-2: Role-based access control
The system enforces the four-role hierarchy on every API endpoint and UI surface.

**Consequences (testable):**
- A Reviewer attempting to trigger an Assessment receives HTTP 403.
- An Analyst attempting to approve at Gate 3 receives HTTP 403 unless also the Project Owner.
- Removing a User from a Tenant immediately revokes all their active sessions within 60 seconds.

#### FR-3: Project status lifecycle
A Project moves through statuses: `draft` → `ingesting` → `analysing` → `reviewing` → `complete`. Each transition is logged with a timestamp and the User who triggered it.

**Consequences (testable):**
- A Project in `complete` status cannot have a new Assessment triggered without an explicit re-open action (Org Admin).
- Status history is retrievable via the API and visible in the UI as a timeline.

#### FR-4: Tenant usage dashboard
An Org Admin can view current-month token consumption, number of active Projects, number of completed Assessments, and remaining Token Budget, updated within 5 minutes of any LLM inference event.

**Consequences (testable):**
- Token consumption is displayed broken down by Assessment.
- At 80% Token Budget, the Org Admin receives an in-app notification and email alert.
- At 100%, new Assessment triggers are blocked and the Org Admin is notified with upgrade instructions.

---

### 5.2 Integration Hub

**Description:** The Integration Hub is where Consultants connect enterprise data sources to the platform. Each Connector authenticates via OAuth 2.0 or API key, performs an initial full sync, and subsequently performs incremental syncs. Connectors are strictly read-only — the platform never writes data back to source systems in v1. Credentials are stored in a secrets vault. If a Connector fails during an Assessment, the platform degrades gracefully and notes the gap in the Proposal Package rather than failing the entire run.

**MVP Connectors (3):** GitHub Enterprise, Jira Cloud, Manual Upload (Excel + PDF).  
**Full Platform Connectors (v2, 20+):** ServiceNow CMDB, LeanIX, Confluence, GitLab, Azure DevOps, AWS, Azure, GCP, Datadog, PagerDuty, Backstage, and others. [ASSUMPTION: Connector availability follows the Phase 1→2→3 roadmap in the research document]

**Functional Requirements:**

#### FR-5: Connector connection flow
An Org Admin or Project Owner can connect a supported data source by initiating a provider-specific OAuth 2.0 flow or entering an API key. The system stores credentials encrypted in the secrets vault (never in plaintext) and associates them with the Tenant.

**Consequences (testable):**
- After a successful OAuth flow, the system displays a green "Connected" status card within 30 seconds.
- Credentials are never returned in any API response; only a masked reference is surfaced.
- Connecting the same data source twice presents an "already connected — reconnect?" confirmation.

#### FR-6: Connection health monitoring
The system performs a health check against each connected data source every 15 minutes and displays a status indicator (green / amber / red) with a last-checked timestamp.

**Consequences (testable):**
- A Connector failing health checks for more than 30 minutes transitions to red and notifies the Org Admin.
- A red-status Connector does not block other Connectors from syncing.

#### FR-7: Initial full sync and incremental sync
Upon connection, the system performs a full sync of all supported entity types. Subsequently, incremental syncs run every 6 hours (default) or on manual trigger. Sync jobs are asynchronous.

**Consequences (testable):**
- Full sync progress is visible as a percentage-complete bar with entity counts.
- Incremental sync fetches only entities modified since the previous sync timestamp.
- Failed sync events are retried with exponential backoff (3 retries, max 30-min delay); after 3 failures the job is marked failed and an alert is raised.

#### FR-8: Manual document upload
An Analyst or Project Owner can upload files to a Project (Excel .xlsx, PDF, Word .docx, CSV; max 50 MB each). Uploaded documents are processed through the Knowledge Base ingestion pipeline.

**Consequences (testable):**
- Files exceeding 50 MB are rejected with a clear error message.
- Processing status (pending / processing / ready / failed) is visible per file.
- Uploaded documents are stored in Tenant-scoped object storage; never accessible to other Tenants.

---

### 5.3 AI Assessment Engine

**Description:** The Assessment Engine is the platform's core — a LangGraph-orchestrated multi-agent pipeline that runs specialised agents in parallel across three phases (Discovery, Analysis, Synthesis), produces Findings, and presents HITL Gates for Consultant review between phases. [ASSUMPTION: LLM provider is Amazon Bedrock (Claude Sonnet 4 primary) for all MVP inference; BYOLLM support deferred] Agent implementation details live in `addendum.md`.

**Assessment Phases:**
- **Phase 1 — Discovery:** Application inventory consolidation from all Connectors and manual uploads.
- **Phase 2 — Analysis:** Cloud readiness scoring, technical debt quantification, AI readiness assessment, security finding extraction — run in parallel.
- **Phase 3 — Synthesis:** Proposal writing, executive summary generation, diagram generation, and adversarial reviewer quality pass — run in parallel.

**HITL Gates:**
- **Gate 1 (post-Discovery):** Consultant confirms application inventory scope. Optional — skippable by Project Owner.
- **Gate 2 (post-Analysis):** Consultant reviews and approves Findings before proposal generation. Mandatory.
- **Gate 3 (pre-Delivery):** Consultant performs final review of the complete Proposal Package. Mandatory.

**Functional Requirements:**

#### FR-9: Assessment trigger and configuration
A Project Owner or Analyst can trigger a new Assessment on a Project that has at least one connected data source or uploaded document. The Assessment is configured with: scope (all sources or a subset), model routing preference (auto / economy / quality), and an optional token budget override.

**Consequences (testable):**
- Triggering on a Project with zero sources or uploads returns a validation error.
- An Assessment cannot be triggered when another is already in a non-terminal status.
- The system records triggered-by User, trigger timestamp, and configuration on the Assessment record.

#### FR-10: Real-time assessment status streaming
A Consultant can monitor a running Assessment via a real-time status screen streaming phase transitions and agent-level progress events via Server-Sent Events.

**Consequences (testable):**
- Status events are delivered within 5 seconds of occurring in the agent pipeline.
- If the browser disconnects and reconnects, the status screen resumes from current state without a page reload.
- The status stream remains available for completed Assessments for a minimum of 7 days.

#### FR-11: Agent failure handling and partial completion
If an individual agent fails, the system retries up to 3 times with exponential backoff. After 3 failures, the agent's output is marked "incomplete" and the Assessment continues with available data. The Proposal Package includes gap notices where data is missing.

**Consequences (testable):**
- A single agent failure never causes the entire Assessment to fail unless the Discovery Agent finds zero applications.
- Every agent failure is surfaced in the UI as a warning with a plain-language explanation.
- Agent failures are logged with structured metadata (agent name, error type, retry count, timestamp).

#### FR-12: Assessment history and versioning
Each Assessment is a versioned, immutable record within a Project. Previous Assessments and their Proposal Packages remain accessible when a new Assessment is run.

**Consequences (testable):**
- Assessment records are never deleted or overwritten; only new versions are created.
- The Projects dashboard displays the most recent completed Assessment summary with a link to all historical versions.

---

### 5.4 Knowledge Base & RAG Pipeline

**Description:** The Knowledge Base is the per-tenant document intelligence layer. Raw data is normalised, chunked using document-type-aware strategies, embedded using Amazon Titan Embed v2, indexed for dense vector search (pgvector HNSW) and sparse keyword search (OpenSearch BM25), and enriched into a knowledge graph (Neo4j in v2; pgvector-only in MVP). AI agents retrieve context using a hybrid RAG pipeline: dense + sparse search → Reciprocal Rank Fusion → cross-encoder reranking. [ASSUMPTION: Knowledge Base is Tenant-scoped from day one via PostgreSQL Row Level Security]

**Functional Requirements:**

#### FR-13: Document ingestion and type-aware chunking
The system ingests all Connector-sourced and manually-uploaded documents, detects document type, and applies the appropriate chunking strategy. All chunks are embedded and indexed.

**Consequences (testable):**
- Processing status per document transitions through `queued → chunking → embedding → indexed → ready` and is visible in the UI.
- A document that fails processing is marked `failed`; other documents continue processing.
- Chunk metadata includes source document ID, tenant ID, project ID, and page/section reference, enabling source citations in Findings.

#### FR-14: Hybrid search retrieval
AI agents retrieve context using dense vector similarity (pgvector), sparse keyword search (OpenSearch BM25), and Reciprocal Rank Fusion re-ranking. All retrievals are scoped to the requesting agent's Tenant and Project.

**Consequences (testable):**
- A retrieval query never returns chunks from a different Tenant, regardless of query content.
- Retrieval latency (top-10 results) is under 500 ms at p95.
- Each retrieved chunk includes a source citation that is preserved in the Finding rationale.

#### FR-15: Application dependency knowledge graph
The system builds a per-Tenant graph mapping application dependencies, team ownership, technology stack, and cloud deployment. [ASSUMPTION: Neo4j-backed GraphRAG is a v2 capability; MVP uses pgvector + relationship metadata stored in PostgreSQL JSON columns for lightweight multi-hop queries]

**Consequences (testable):**
- The graph is updated incrementally on each Connector sync.
- Cross-tenant graph traversal is not possible.
- If the graph layer is unavailable, AI agents fall back to vector-only retrieval with a logged warning.

---

### 5.5 Proposal Studio

**Description:** The Proposal Studio governs the generation, preview, storage, and delivery of Proposal Packages. Document generation is template-driven with AI-generated content injected into Jinja2 templates. All artefacts are stored in Tenant-scoped object storage and retrieved via time-limited signed URLs. Document quality — not just functional correctness — is a first-class product goal: the generated PDF and DOCX must be presentation-ready at a standard a senior consultant would not be embarrassed to send to a CTO.

**MVP Outputs (3):** Executive Summary PDF, AI Recommendations Report DOCX, Application Portfolio Heatmap PNG.  
**v2 Outputs:** Full Proposal Deck PPTX (20–30 slides), ROI Model XLSX, C4 Architecture Diagrams. [ASSUMPTION: Full PPTX deck is the highest-priority v2 output]

**Functional Requirements:**

#### FR-16: Automated Proposal Package generation
Upon Consultant approval at Gate 2, the system generates the Proposal Package in parallel using approved Findings as input and the Tenant's active template set (or platform default).

**Consequences (testable):**
- All three MVP artefacts (PDF, DOCX, PNG) are generated within 45 minutes of Gate 2 approval on a 150-application Assessment.
- If one artefact fails, the others are still produced and made available; the failed artefact can be retried.
- Every generated document includes a generation timestamp and Assessment ID in the footer or document metadata.

#### FR-17: Proposal preview and download
A Consultant can preview the Proposal Package in-browser and download each artefact individually or as a ZIP bundle via a time-limited signed URL (default: 72-hour expiry, refreshable).

**Consequences (testable):**
- Preview renders within 5 seconds for PDFs up to 10 MB.
- Expired signed URLs return a clear expiry message with a "Request new link" action.
- Download events are logged to the audit trail with User ID, artefact name, and timestamp.

#### FR-18: Branded template management
An Org Admin can upload custom master templates (PPTX master, DOCX template). The system validates required placeholder markers before accepting. [ASSUMPTION: Custom template upload is a v2 feature; MVP ships with one default template set only]

**Consequences (testable):**
- Template upload validates required placeholder markers; missing required placeholders produce a validation error.
- Up to 5 saved template versions per Tenant; switching between versions is possible per-Project before generation.
- Templates are stored in Tenant-scoped storage; never accessible to other Tenants.

#### FR-26: Output-as-marketing attribution watermark
Every generated Proposal Package artefact (PDF footer, DOCX cover page, PNG corner badge) includes a configurable attribution mark: "AI Transformation Proposal powered by [Platform Name]" with a link to the platform's marketing site. Tenants on Professional tier and above can configure the watermark text (e.g. replace platform name with their firm name); the link remains. Tenants on the Enterprise tier can suppress the watermark entirely under white-label terms. [ASSUMPTION: White-label watermark suppression is an Enterprise-tier differentiator only; it is not available on Starter or Professional tiers]

**Rationale:** Every proposal delivered to an enterprise client is simultaneously a distribution event. The recipient CTO who sees the watermark and asks "how did you make this?" is the next inbound lead. This mechanic is built into the artefact, not bolted on as a marketing campaign.

**Consequences (testable):**
- A generated PDF on the default template includes the attribution text in the document footer on every page.
- A generated PNG heatmap includes the attribution badge in the lower-right corner.
- A Professional-tier Org Admin can customise the watermark display name (max 60 characters) via Tenant settings; the platform domain link is not removable at this tier.
- An Enterprise-tier Tenant with white-label terms active generates artefacts with no platform attribution visible.
- The watermark configuration is recorded on the Tenant record and applied at generation time; changes take effect on the next generated Proposal Package.

---

### 5.6 Human-in-the-Loop (HITL) Review

**Description:** The HITL Review feature gives Consultants structured control over AI Findings before they influence the Proposal Package. Each HITL Gate presents a finding-by-finding review screen with the AI's reasoning, source data, and a confidence score. Security Findings classified as critical are mandatory review items — they cannot be auto-approved. The platform never delivers a Proposal Package without explicit Consultant approval at Gate 3. HITL gates are enforced at the API layer with cryptographic approval tokens — they are architectural constraints, not UX affordances.

**Functional Requirements:**

#### FR-19: Finding review screen
At each HITL Gate, the system presents Findings with: application name, Finding type, AI-assigned score, confidence level (0–100%), a plain-language rationale, and source citations. Findings are sorted by confidence ascending (lowest-confidence first).

**Consequences (testable):**
- A Finding without at least one source citation is marked `[NEEDS REVIEW]` and cannot be auto-approved.
- The Consultant can filter Findings by type, confidence range, and TIME classification.
- The Finding review screen renders all Findings for a 150-application Assessment within 3 seconds.

#### FR-20: Finding approval, rejection, and override
A Consultant can for each Finding: (a) **Approve** — accept as-is; (b) **Reject** — exclude from Proposal Package with mandatory comment; (c) **Override** — change the AI-assigned score or TIME classification with mandatory comment.

**Consequences (testable):**
- Override comments are stored on the Finding record but not surfaced in client-facing Proposal Package documents.
- Rejecting a Finding removes it from all Proposal Package documents.
- All approve / reject / override actions are recorded in the immutable audit log with User ID, timestamp, and previous vs. new value.

#### FR-21: Gate progression and mandatory review enforcement
At Gate 2, all Findings must be confirmed before proposal generation begins. A bulk "Approve All" button is available but requires a confirmation dialog. Security Findings of critical severity cannot be bulk-approved. At Gate 3, the Consultant must confirm the complete Proposal Package before download links are activated.

**Consequences (testable):**
- Proposal Package generation does not begin until Gate 2 is confirmed.
- Download links are not generated until Gate 3 is confirmed.
- Attempting to skip Gate 3 via direct API call without the approval token returns HTTP 403.

#### FR-22: HITL audit trail
Every Finding review action is recorded in an immutable, append-only audit log per Tenant, exportable by Org Admin as CSV.

**Consequences (testable):**
- Audit log entries cannot be deleted or modified by any User role, including Org Admin.
- Export includes: timestamp, User ID, User email, Finding ID, action type, previous value, new value, comment.

---

### 5.7 Observability & Usage Reporting

**Description:** The platform is observable at three layers: infrastructure (ECS, RDS, queue depth), application (API latency, error rates, distributed traces), and AI (LLM call latency, token consumption, RAG retrieval quality). Tenant-facing metrics help Consultants understand platform performance; internal metrics enable Balu to detect and respond to incidents before customers are impacted.

**Functional Requirements:**

#### FR-23: Tenant usage reporting
An Org Admin can view: assessments run (current month and historical), token consumption by Assessment and by model, average proposal generation time, and agent completion rate.

**Consequences (testable):**
- Usage data is updated within 5 minutes of any LLM inference event.
- Usage data is exportable as CSV with one row per Assessment.

#### FR-24: AI quality metrics
The platform records per-Assessment quality signals: Finding confidence distribution, HITL override rate, agent completion rate, and proposal generation success rate.

**Consequences (testable):**
- HITL override rate is surfaced in the internal admin dashboard as a platform-level quality signal.
- Per-Assessment quality metrics are stored and queryable for at least 12 months.

#### FR-25: LLM and agent observability
All LLM calls are traced end-to-end via Langfuse integration: call latency, token count, model ID, agent name, and parent Assessment ID.

**Consequences (testable):**
- Every LLM inference call is captured in Langfuse within 10 seconds.
- Token consumption per LLM call is reconciled against the Tenant's Token Budget within the 5-minute usage reporting window.

---

## 6. Cross-Cutting Non-Functional Requirements

### Performance
- API endpoints (non-streaming): p95 < 300 ms; p99 < 500 ms.
- SSE status endpoints: first event delivered within 5 seconds of agent milestone.
- Proposal Package generation (3 MVP artefacts, 150 applications): complete within 45 minutes of Gate 2 approval.
- Knowledge Base hybrid search (top-10): p95 < 500 ms.
- UI dashboard load (cold): < 3 seconds on a standard broadband connection.

### Security & Multi-Tenancy
- All Tenant data is isolated at the PostgreSQL Row Level Security layer. Cross-tenant data access is impossible through the application API or direct DB query using the application service account.
- All data at rest encrypted (AES-256, AWS KMS managed keys). All data in transit uses TLS 1.3 minimum.
- Secrets stored exclusively in AWS Secrets Manager — never in code, environment variables, logs, or API responses.
- Prompt injection prevention: all user-supplied content is sanitised before any LLM call; LLM outputs are validated before document insertion.
- No raw customer PII appears in LLM prompts. PII is detected and masked before embedding or inference.
- Per-Tenant LLM token rate limits are enforced at the API Gateway layer.

### Reliability
- Platform availability target: 99.9% (< 8.7 hours downtime per year).
- All AI agent jobs are idempotent and safe to retry. Temporal.io provides durable execution — a workflow interrupted at any point resumes from its last checkpoint. [ASSUMPTION: Temporal Cloud in MVP; self-hosted at 500+ tenants]
- RDS Aurora PostgreSQL Multi-AZ. RPO: 1 hour. RTO: 30 minutes.
- If Amazon Bedrock is unavailable, Assessment jobs are queued and resume when the service recovers; the Consultant is notified.

### Observability
- OpenTelemetry traces, metrics, and structured logs emitted from all services from day one.
- Every AI Finding is traceable to: the LLM call → the RAG chunks retrieved → the source documents → the Connector sync that ingested them.
- All LLM inference captured in Langfuse with Assessment-level correlation IDs.

### Accessibility
- Web application meets WCAG 2.1 AA minimum. [ASSUMPTION: Full audit performed before first enterprise customer go-live, not during MVP build]
- Generated PDFs and DOCX files use heading structure and alt-text for charts.

---

## 7. Constraints & Guardrails

### Safety
- Security Findings classified as critical or high severity are **never auto-approved** and **never auto-published** to a client without explicit Consultant review at Gate 2.
- ROI estimates in Proposal Packages always display confidence intervals and data sources. Absolute figures without confidence ranges are not permitted in generated documents.
- Every AI-generated Finding must include source citations. Findings without citations are flagged `[NEEDS REVIEW]` and excluded from bulk-approval flows.
- HITL Gates (Gate 2, Gate 3) are non-bypassable architectural constraints enforced at the API layer with cryptographic approval tokens.

### Privacy & Data Governance
- The platform operates on a **read-only, do not store raw PII** principle. Raw PII (employee names in commit logs, ticket assignees, email addresses in docs) is detected, masked, and not stored in the Knowledge Base.
- Customer enterprise data is never used to train or fine-tune any model. [ASSUMPTION: Fine-tuning on customer data requires explicit opt-in consent; no programme exists in MVP]
- Data residency: MVP deploys to `us-east-1` only. EU data residency is a v2 feature. [ASSUMPTION: No EU customers in MVP; all MVP customers accept US data residency]
- Data retention: Tenant data retained for 24 months after last Project activity. Deletion requests fulfilled within 30 days.

### Cost
- LLM inference cost is a primary operational risk. The platform uses intelligent model routing: Claude Haiku 3.5 for classification; Claude Sonnet 4 for analysis and writing; Claude Opus 4 only for complex multi-hop architectural reasoning. [ASSUMPTION: Routing rules are tuned during MVP; initial rules documented in addendum.md]
- Per-Tenant Token Budgets enforce a hard ceiling on monthly LLM spend. Alerts at 80%, blocks at 100%.
- Result caching (Redis, 24-hour TTL): analysis results for unchanged source data are served from cache, not re-run.

---

## 8. Non-Goals (Explicit)

- **The platform does not write back to source systems in v1.** It cannot create Jira epics, update LeanIX records, or commit to GitHub. Write-back is a v2 scope item, gated behind explicit customer consent flows.
- **The platform is not a project management tool.** It reads from work-tracking systems; it does not replace them.
- **The platform does not perform code execution or dynamic application testing.** Analysis is static (language detection, dependency analysis, commit history). It does not run code, execute test suites, or perform dynamic security scanning.
- **The platform does not provide on-premises or private-VPC deployment in v1.** On-prem is a v2+ commitment gated behind enterprise contract negotiation.
- **The platform does not support customer-provided LLM API keys (BYOLLM) in v1.** Amazon Bedrock is the exclusive inference provider in MVP. BYOLLM is an open question for v2 (see §14).
- **The platform is not a compliance certification tool.** It surfaces compliance-related Findings but does not issue certifications, conduct formal audits, or produce legally admissible compliance reports.
- **White-label / partner branding customisation of the platform UI is not a v1 feature.** Tenants can upload branded document templates (FR-18) but the platform UI is not white-labelled in v1.
- **The platform does not support real-time or streaming data ingestion in v1.** Data is ingested via batch Connector syncs (full + incremental) on a scheduled basis.

---

## 9. MVP Scope (3-Month Build)

**Goal:** Build enough platform to run a complete, compelling assessment on a real or realistic enterprise dataset — GitHub + Jira + manual upload — and produce a Proposal Package that a senior consultant would be proud to share with a CTO. The MVP is primarily a validation and pilot-winning artefact, not a production-hardened multi-tenant system.

### 9.1 In Scope for MVP

**Connectors (3):**
- GitHub Enterprise: language detection, framework identification from package manifests, commit activity, DORA metrics.
- Jira Cloud: project portfolio, active epics, velocity, issue labels.
- Manual Upload: Excel application inventory, architecture PDFs.

**AI Agents (4):**
- Discovery Agent — consolidates application inventory across all connected sources.
- Architecture Agent — analyses code repositories for technology stack, technical debt signals, and cloud readiness indicators.
- Proposal Writer Agent — generates executive summary and structured recommendations report from approved Findings.
- Reviewer Agent — adversarial quality check with confidence scoring; flags low-confidence Findings for Consultant review.

**HITL Gates:** Gate 2 (post-Analysis, mandatory) and Gate 3 (pre-Delivery, mandatory). Gate 1 optional.

**Proposal Package Artefacts (3):**
- Executive Summary PDF
- AI Recommendations Report DOCX
- Application Portfolio Heatmap PNG

**Features in scope:**
- Project creation and management (FR-1, FR-3)
- Connector connection flow and manual upload (FR-5, FR-8)
- Assessment trigger and real-time status streaming (FR-9, FR-10)
- Agent failure handling (FR-11)
- Finding review, approve, reject, override (FR-19, FR-20)
- Gate 2 and Gate 3 enforcement (FR-21)
- Basic Proposal Package generation and download (FR-16, FR-17)
- Default template set (no custom template upload in MVP)
- Basic usage dashboard (FR-23)
- Langfuse LLM observability (FR-25)

**Infrastructure:** Single-tenant architecture with multi-tenancy designed at data model level (RLS schemas ready) but not self-serve. Tenant provisioning is manual. AWS ECS Fargate + RDS Aurora PostgreSQL + S3 + SQS. Amazon Bedrock (Claude Sonnet 4 primary). Temporal Cloud from day one. [ASSUMPTION: Temporal Cloud used in MVP to avoid workflow resumption complexity]

### 9.2 Out of Scope for MVP

| Item | Status | Reason |
|------|--------|--------|
| SSO / SAML / SCIM | v2 | Not blocking for first pilot |
| Self-serve tenant onboarding | v2 | Manual onboarding acceptable at 0–10 tenants |
| 20+ connector library | v2 | MVP validates core loop with 3 sources |
| Full 12-agent suite (Cloud, Security, ROI agents) | v2 | 4 agents demonstrate differentiated value |
| Full PPTX proposal deck (20–30 slides) | v2 | PDF + DOCX + PNG sufficient for pilot validation |
| ROI Model XLSX | v2 | High value, high complexity; v2 after template design |
| Custom branded template upload | v2 | Default template used in MVP |
| Neo4j knowledge graph (GraphRAG) | v2 | pgvector + OpenSearch sufficient for MVP retrieval |
| EU data residency | v2+ | No EU customers in MVP |
| On-premise / VPC deployment | v2+ | Hard requirement only at Global 2000 tier |
| Write-back to source systems | v2 | Out of scope by design decision |
| BYOLLM (customer API keys) | Open question | See §14 |
| Client delivery portal (branded subdomain) | v2 | Time-limited signed URL sufficient for MVP |
| Proposal collaboration (multi-user editing) | v2 | Single-consultant workflow for MVP |
| White-label partner programme | v2+ | Requires validated platform and partner demand |
| Mobile app | v3 | Web-first |

---

## 10. Integration & Dependencies

**External services (MVP):**

| Service | Purpose | Dependency Level | Fallback |
|---------|---------|-----------------|---------|
| Amazon Bedrock (Claude Sonnet 4) | Primary LLM inference | Critical | Jobs queue; retry when available |
| Amazon Bedrock (Titan Embed v2) | Document embedding | Critical | Ingestion pipeline pauses; retries |
| Amazon RDS Aurora PostgreSQL | Primary data store | Critical | Multi-AZ automatic failover |
| Amazon S3 | Document and artefact storage | Critical | Retries with exponential backoff |
| Amazon SQS | Async job queue | Critical | Agents wait; no in-memory fallback |
| Temporal Cloud | Durable workflow execution | Critical | Assessments cannot run without it |
| Amazon ElastiCache Redis | Cache, rate limiting | Important | Graceful degradation; cache miss hits DB |
| AWS OpenSearch | BM25 hybrid search | Important | Falls back to pgvector-only retrieval |
| Auth0 | Authentication and session management | Critical | Platform inaccessible without Auth0 |
| Langfuse Cloud | LLM observability | Monitoring only | Platform continues; traces lost |

**External data sources (Connector-level, MVP):**

| Source | Authentication | Data Accessed |
|--------|--------------|---------------|
| GitHub Enterprise | GitHub App (OAuth) | Repos, languages, dependencies, commits, DORA metrics |
| Jira Cloud | OAuth 2.0 3LO | Projects, issues, epics, sprints, velocity |
| Manual Upload | N/A | Excel, PDF, DOCX (customer-provided) |

---

## 11. Risk & Mitigations

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| AI Finding quality below consultant acceptance threshold | High | Critical | HITL gates let Consultants override; Reviewer Agent adversarial pass; quality rated by 3 independent consultants before launch (SM-3) |
| Enterprise refusal to share credentials / data access | Medium | High | Start with GitHub + Jira (low-sensitivity); manual upload path always available; prepare data processing addendum templates |
| LLM hallucinations in Findings | High | High | Source citations required on every Finding (FR-19); confidence scoring; mandatory individual review for critical security Findings |
| Bedrock API rate limits blocking Assessment runs | Medium | Medium | Per-tenant token budgets; model routing to Haiku for cheap tasks; exponential backoff with jitter |
| LLM provider outage | Low | High | SQS queue absorbs jobs; Assessment resumes when Bedrock recovers; Consultant notified of delay |
| Token cost overruns (solo founder budget risk) | High | High | Per-assessment token budget caps; model routing; aggressive caching; Redis result TTL 24 hours |
| No pilot customer materialises after MVP build | Medium | Critical | Parallel validation: demo video, technical blog, LinkedIn outreach to GCC heads and transformation leads; conference/hackathon entry; prioritise ANSR and TechD conversations first (founder-accessible, no cold outreach needed) |
| Security breach / cross-tenant data leak | Low | Critical | RLS enforced from day one; automated cross-tenant access test battery in CI/CD; pen test before any real customer goes live |
| EU AI Act compliance requirements | Medium | Medium | HITL gates + source citations + audit logs provide explainability foundation; formal legal review required before EU market entry |
| ANSR internal adoption fails (historical pattern) | Medium | Medium | ANSR has a pattern of building products that are not adopted internally (BOQ, LinkedIn agent, ConfigMate). Mitigate: do not make ANSR the only GTM bet; pursue TechD and direct outreach in parallel; treat ANSR as bonus validation, not sole path. |
| TechD integration opportunity is mis-timed | Low | Medium | TechD (Balu's US employer contact who introduced BMAD) has a gap-finding product (Mainframe to Agentify) where this platform is the natural upstream complement. Risk: Balu's employment relationship complicates an IP/commercial arrangement. Mitigate: clarify IP ownership and any employment non-compete before any commercial discussion with TechD. |

---

## 12. ROI / Business Case

**For the customer (Professional Tier, 20 proposals/year):**

| Cost item | Manual process | Platform |
|-----------|--------------|---------|
| Per-proposal cost | ~$43,000 (120 hrs senior + 80 hrs analyst + expenses) | $7,200 (platform cost allocation) |
| Time to deliver | 4–12 weeks | 4–8 hours |
| Annual cost (20 proposals) | ~$860,000 | $144,000 |
| **Annual savings** | — | **$716,000** |
| **Year 1 ROI** | — | **397%** |

**For the platform at scale:**

At 10 Professional Tier customers ($12K/month each): $120K MRR / $1.44M ARR.  
Infrastructure at 10 tenants: ~$10K/month.  
Infrastructure gross margin: ~91%.

**TechD partnership business case (near-term validation path):**

TechD's existing product identifies technology stack gaps in enterprise client environments (Mainframe modernisation, Agentify candidates). The AI Transformation Proposal Generator is the natural upstream layer: TechD finds *what* needs modernising; this platform generates the *why, what it costs, and what the AI roadmap looks like*. Together they form a complete end-to-end enterprise AI transformation offer.

| Scenario | Current TechD | TechD + Platform |
|----------|--------------|------------------|
| Per-engagement deliverable | Gap analysis report | Full AI transformation proposal package |
| Time to produce proposal | 4–8 weeks manual | 4–8 hours automated |
| Proposal upsell potential | Low (one-off gap analysis) | High (recurring assessment subscription) |
| Deal size expansion | Baseline | Estimated 2–3× engagement value |

For Balu this creates three possible commercial arrangements with TechD: (a) product sale / licensing, (b) joint go-to-market partnership, or (c) a founding engineer / co-founder role. The BMAD workflow artefact package being built now is the credibility signal for any of these conversations.

**ANSR internal sales weapon case:**

ANSR currently closes GCC setup deals using CEO-led storytelling and past success stories. Adding the platform as a live demo tool in prospect meetings gives ANSR a category-creating differentiation: ACC (AI Capability Center) setup, not just GCC setup. Even one deal closed with a live platform demo validates the internal sales weapon mode.

**MVP investment thesis:** The MVP is built to validate three questions: (1) Does the assessment quality meet consultant standards? (2) Will at least one enterprise client or consulting firm pay for it? (3) Does the end-to-end workflow work reliably on real enterprise data? With ANSR and TechD as founder-accessible validation targets, the path to answering question (2) is weeks, not months.

---

## 13. Success Metrics

*Each SM cross-references the FR(s) it validates.*

**Primary**

- **SM-1: Proposal Acceptance Rate** — % of generated Proposal Packages that reach Gate 3 approval without the Consultant rejecting the entire run and restarting. Target: ≥ 85% within first 10 assessments. Validates FR-9, FR-10, FR-16, FR-19.
- **SM-2: Time-to-Proposal** — Wall-clock time from Assessment trigger to Gate 3 approval (including Consultant review time). Target: median ≤ 6 hours for a 150-application assessment. Validates FR-10, FR-16.
- **SM-3: External Quality Rating** — Proposal Package quality rating (1–5) from at least 3 independent experienced consultants reviewing MVP output against a real or realistic enterprise dataset. Target: ≥ 4.0 average. Validates FR-16, FR-19.

**Secondary**

- **SM-4: Agent Completion Rate** — % of agents within an Assessment that complete successfully without requiring human retry. Target: ≥ 90%. Validates FR-11.
- **SM-5: HITL Override Rate** — % of AI-generated Findings that Consultants override (score change or rejection). Target: ≤ 30% by Assessment run #10 (decreasing as prompts improve). Validates FR-19, FR-20.
- **SM-6: Time to Onboard First Real Customer** — Time from signed agreement to first completed assessment on real customer data. Target: ≤ 2 business days. Validates FR-1, FR-5, FR-8.
- **SM-7: Connector Reliability** — % of Connector sync jobs that complete without error. Target: ≥ 95% per connector per month. Validates FR-6, FR-7.
- **SM-8: Watermark Referral Rate** — % of Proposal Package recipients (enterprise clients who receive a delivered proposal) who subsequently contact the platform or the delivering firm specifically citing the proposal or asking about the tool used to generate it. Target: ≥ 10% within first 20 delivered proposals. Validates FR-26. [ASSUMPTION: Tracked via UTM-tagged links in the watermark attribution URL]

**Counter-metrics (do not optimise)**

- **SM-C1: Speed vs. Quality Trade-off** — Do not optimise Time-to-Proposal (SM-2) at the expense of Proposal Quality Rating (SM-3). Speed is meaningless if output quality drops below consultant acceptance threshold.
- **SM-C2: HITL Skip Rate** — Do not make HITL gates so frictionless that Consultants rubber-stamp all Findings. A zero override rate is a red flag indicating gates are being bypassed, not that AI quality is perfect.
- **SM-C3: Token Cost per Assessment** — Do not optimise SM-4 (Agent Completion Rate) by routing all tasks to Claude Opus. Track cost per assessment and enforce model routing rules.
- **SM-C4: Watermark Suppression Rate** — Do not push Enterprise customers to suppress the watermark as a sales incentive ("upgrade to remove branding") if it is working as a referral engine. Monitor the pipeline contribution of watermark-attributed leads before offering suppression at lower tiers.

---

## 14. Open Questions

1. **BYOLLM (Bring Your Own LLM):** Should enterprise customers configure their own Bedrock, Azure OpenAI, or OpenAI API keys, with inference costs billed directly to their cloud account? Significant implications for pricing model, multi-model abstraction, and security architecture. [Decision needed before v2 roadmap is locked]

2. **Write-back to source systems:** The research flags high value in creating Jira epics for recommended modernisation initiatives. What consent model, audit trail, and rollback capability would be required for enterprise trust? [Needs discovery with first pilot customers]

3. **Big 4 / SI OEM programme:** Does GTM include an OEM/white-label programme for Deloitte, Accenture, TCS? This significantly changes the v2 roadmap (white-label UI, partner admin console, revenue share). [Blocked on first customer conversations]

4. **Proposal quality benchmark:** Who defines "good enough"? The MVP success criterion (SM-3) uses 3 independent consultants organised by Balu. Is there an industry-standard rubric that would be credible to enterprise buyers? [Resolve before GA launch]

5. **Temporal.io hosting model:** Temporal Cloud has lower operational burden but adds cost at scale. When to self-host? [Evaluate at ~500 tenants or when Temporal cost exceeds $5K/month]

6. **EU AI Act scope:** Does a tool producing AI transformation recommendations for large enterprises fall under the EU AI Act's high-risk classification? The HITL gate and audit log architecture provides a strong foundation — but formal legal review is needed before EU market entry.

7. **Data retention and right-to-erasure:** What is the exact deletion process for a churned customer's Tenant data, Knowledge Base embeddings, and S3 artefacts? How is deletion verified for GDPR compliance? [Resolve before first EU customer or DPA signing]

8. **Fine-tuning on customer data:** Should early pilot customers be offered an incentive to opt their accepted proposals into a training dataset for future model fine-tuning? Creates a proprietary data moat but requires careful consent design. [Resolve with legal counsel; not before 10 pilot customers]

9. **ANSR as first customer vs. GTM risk:** ANSR has a documented pattern of commissioning tools that are not adopted internally (BOQ, LinkedIn agent, ConfigMate). Should the product be positioned as ANSR's internal tool first, or should ANSR be approached as a pilot customer paying for a subscription? The former risks the non-adoption pattern; the latter creates a commercial relationship that forces accountability. [Needs a direct conversation with ANSR leadership — Balu has access; requires a clear decision before MVP is complete]

10. **TechD commercial arrangement:** Given that TechD's product (tech stack gap analysis) is a direct upstream complement to this platform, what is the right commercial structure — product licensing, joint go-to-market, or a formal partnership? Any arrangement must be reviewed against Balu's current employment terms and IP assignment clauses before any commercial conversation. [Resolve before approaching TechD commercially; employment counsel review recommended]

---

## 15. Assumptions Index

- **A-1** (§3.1) — In MVP, Balu operates the platform directly as sole Platform Admin; multi-tenant self-service admin is a v2 feature.
- **A-2** (§3.2) — Freelance / solo consultant tier may be added in v2 if market demand is validated.
- **A-3** (§3.3 UJ-3) — Client delivery portal (web-based, branded, no-login) is a v2 feature; v1 uses time-limited S3 signed URLs.
- **A-4** (§5.3) — LLM provider is Amazon Bedrock (Claude Sonnet 4 primary model) for all MVP inference; BYOLLM deferred.
- **A-5** (§5.1) — SSO via SAML/OIDC is a v2 feature; v1 uses email/password with MFA for Org Admin accounts.
- **A-6** (§5.2) — Connector availability follows the Phase 1→2→3 roadmap from the research document; MVP ships 3 connectors only.
- **A-7** (§6, Reliability) — Temporal Cloud used in MVP for lower operational burden; self-hosting evaluated at 500+ tenants or >$5K/month Temporal cost.
- **A-8** (§6, Accessibility) — Full WCAG 2.1 AA audit is performed before first enterprise customer go-live, not during the MVP build sprint.
- **A-9** (§7, Privacy) — Customer data usage for model fine-tuning requires explicit opt-in consent from each customer; no such programme in MVP.
- **A-10** (§7, Privacy) — EU data residency is a v2 feature; all MVP customers accept US data residency (`us-east-1`).
- **A-11** (§7, Cost) — Model routing rules (Haiku / Sonnet / Opus) are tuned during MVP with real workloads; initial rules documented in addendum.md.
- **A-12** (§9.1) — Temporal Cloud adopted from day one even in MVP to avoid workflow resumption complexity.
- **A-13** (§9.1, §5.5) — Full PPTX proposal deck (20–30 slides) is the highest-priority v2 output after MVP validation.
- **A-14** (§13) — MVP quality success criterion (SM-3) uses 3 independent consultants from Balu's professional network.
- **A-15** (§3.1 GCC CEO, §3.3 UJ-5) — The live CEO demo journey (UJ-5) requires a pre-capped "Express Mode" assessment with a 20-application scope limit and a <30-minute wall-clock run time; this is distinct from a full Professional-tier assessment and requires separate UX and agent configuration.
- **A-16** (§5.5 FR-26) — Watermark referral tracking uses UTM-tagged links embedded in the attribution watermark URL; analytics are captured via the platform's marketing site, not the platform application itself.
- **A-17** (§12) — TechD and ANSR are treated as founder-accessible validation targets, not contracted customers; no revenue is assumed from either in the MVP financial model.
