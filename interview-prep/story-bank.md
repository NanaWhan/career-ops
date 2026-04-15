# Story Bank — Cherish Banini Seinty

Master STAR+R stories. 5-10 deep stories that bend to answer almost any behavioral question.
These are populated from your strongest proof points in cv.md. Add new stories after each interview.

---

## Story 1 — Scale Under Real Constraints
**Theme:** Owning a complex system end-to-end at production scale
**Source:** GovCitizen — Hubtel, Accra

**S (Situation):** Ghana needed a single government services platform connecting 8 separate agencies — ECG (electricity), Water, SSNIT (pension), NSS (national service), NIC (identity), Ghana Post, TapNGo (transport), and OTP verification. Each agency had its own API, its own data model, and its own SLA. There was no shared infrastructure. The platform had to serve the full population, not a subset.

**T (Task):** I was the lead engineer responsible for the backend architecture and delivery. The platform needed to be reliable at 100K+ daily users from day one, handle concurrent service calls across 8 integrations, and expose a clean citizen-facing interface built on Next.js 14.

**A (Action):** I designed a 34-microservice architecture on .NET 6/8 deployed on Azure. Each agency integration became its own service with independent deployment, failure isolation, and circuit breakers — so a Ghana Post outage didn't take down SSNIT. I built the Next.js 14 citizen portal with Ghana Card OCR verification, biometric face verification using Hubtel's Face SDK, and NextAuth. I set up Azure DevOps CI/CD with SonarQube quality gates and multi-stage builds. I also established coding standards and TDD workflows across the engineering team using xUnit.

**R (Result):** Platform shipped and reached 100K+ daily active users. The multi-agency architecture handled concurrent failures gracefully — individual integrations could go down without affecting other services. The team adopted TDD and quality gates as standard practice.

**Reflection:** The hardest part wasn't the technical complexity — it was designing for failure. Each of those 8 agencies had different reliability characteristics. If I did it again, I'd invest earlier in contract testing between services, not just unit tests inside each one.

**Best for questions about:**
- "Tell me about your most impactful project"
- "Tell me about a time you worked at scale"
- "Tell me about a time you led a technical initiative"
- "Tell me about a distributed systems challenge you solved"
- "Tell me about working with external APIs or third-party integrations"

---

## Story 2 — AI in Production Under Hard Constraints
**Theme:** Shipping LLM integration in a domain that doesn't forgive mistakes
**Source:** Artiv — TechSpotDev, Dallas TX (Remote)

**S (Situation):** Artiv is a defense Command and Control (C2) platform used for military mission planning, Course of Action (COA) analysis, and 9-line report generation. The system runs in a constrained environment — no cloud APIs, no latency tolerance for real-time operational decisions. The client needed LLM-powered analysis without sending classified data to any external service.

**T (Task):** Integrate LLM capabilities into the live C2 platform in a way that worked under defense-grade security constraints: air-gap compatible, self-hosted, fast enough for operational use, and accurate enough to be trusted by military analysts.

**A (Action):** I built the LLM integration using OllamaSharp (for local model inference) and Microsoft.Extensions.AI (for the abstraction layer), keeping all inference on-premises. I built the Blazor-based C2 frontend with real-time dashboards, geospatial visualizations, and LLM-powered COA analysis panels using DevExpress Blazor and SkyShark mapping. I implemented DEFCON-DataServer as a 4-layer enterprise data server (Service, Application, Data Access, Common layers) using WebSockets, AWS S3, PostgreSQL, in-memory caching, and Docker — giving the LLM reliable, structured access to mission data without network calls to external APIs.

**R (Result):** LLM-powered analysis shipped into a live defense system used for actual mission planning. The architecture passed defense-grade security review. Response times were within operational requirements for real-time decision support.

**Reflection:** This taught me that "AI integration" in production means being willing to give up capability for reliability and security. Hosted GPT-4 would have been more capable. Local Ollama was what the constraints allowed. Picking the right tool for the constraint matters more than picking the best tool in isolation.

**Best for questions about:**
- "Tell me about a time you integrated AI into a production system"
- "Tell me about working under tight technical constraints"
- "Tell me about a high-stakes project you delivered"
- "Tell me about building something you'd never built before"
- "Tell me about a time you had to make a tradeoff"

---

## Story 3 — Real-Time Financial Platform at Speed
**Theme:** Event-driven architecture for latency-sensitive financial data
**Source:** MyCreditScore — Hubtel, Accra

**S (Situation):** Hubtel needed a real-time credit scoring platform for Ghanaian users — something that could aggregate financial data from multiple sources, compute a live credit score, and surface it to users through a dashboard. The challenge: credit data changes continuously, and a stale score is worse than no score in financial decisions.

**T (Task):** Design and build the MyCreditScore backend from scratch — real-time capable, horizontally scalable, and able to handle concurrent score updates without locking.

**A (Action):** I built the backend on .NET 8 using Kafka for event streaming (so score updates propagate asynchronously without blocking), Redis for caching (hot path reads serve from cache, not the database), Akka.NET actor model for concurrent message processing without thread contention, and Elasticsearch for full-text search across credit history records. I built the frontend portal in Next.js 14 with React 18, Chart.js for credit score trend visualization, and WebSocket connections for real-time score updates. I also optimized PostgreSQL schemas and SQL Server stored procedures — measured and reduced query latency by 40%.

**R (Result):** Platform delivered real-time credit score updates at scale. The Kafka + Akka.NET combination eliminated the race conditions that had been the initial design concern. The 40% query latency reduction was measured against baseline, not estimated.

**Reflection:** The actor model felt like overkill early on. But when we stress-tested concurrent score updates, it was the thing that held up. I've learned to trust event-driven patterns earlier in the design process rather than adding them as a fix.

**Best for questions about:**
- "Tell me about a performance challenge you solved"
- "Tell me about working with event-driven architecture"
- "Tell me about a time you made a measurable technical impact"
- "Tell me about a fintech or financial domain project"
- "Tell me about designing for concurrency"

---

## Story 4 — Full-Stack SaaS From Zero to Deployed
**Theme:** End-to-end ownership of a complex multi-service product
**Source:** iludate SaaS Agency Platform — TechSpotDev

**S (Situation):** The client needed a full SaaS agency platform — multi-tenant, with real-time video calling, chat, payment processing, job queue management, and notification infrastructure. No existing codebase, no technical team, just a product spec and a deadline.

**T (Task):** Own the entire backend and work with design to ship a production-grade SaaS from scratch.

**A (Action):** I built the backend with NestJS (structured for multi-tenancy), MongoDB (flexible document model for agency/client data), Redis (caching and BullMQ job queues for async tasks), Stripe (subscription and payment processing), Agora (video calling infrastructure), Stream.io (real-time chat), and Twilio/Nodemailer (notification delivery). Every service was built with separate concerns: auth, payments, video, chat, notifications all decoupled. I wrote integration tests for critical payment and auth paths.

**R (Result):** Platform shipped to production. The decoupled architecture allowed independent updates to the payment integration without touching the video layer — which mattered when Stripe's API changed mid-project.

**Reflection:** BullMQ was the right call for the job queue — I'd used simpler in-process queues before and they don't survive crashes. Persistent queues should be the default for anything the user is waiting on.

**Best for questions about:**
- "Tell me about building something complex end-to-end"
- "Tell me about working with third-party integrations"
- "Tell me about a multi-tenant or SaaS architecture"
- "Tell me about managing competing priorities on a project"

---

## How to use these stories in interviews

**For "Tell me about yourself" (opening):**
Lead with Story 1 (GovCitizen) for scale credibility, then Story 2 (Artiv) for AI-in-production differentiation.
> "I build backend systems that hold up under real pressure — 34 microservices serving 100K+ daily users on a government platform, and LLM integration in a live defense system that couldn't touch an external API. That combination of scale and constraint is what I look for in new roles."

**For behavioral questions, map like this:**

| Question type | Best story |
|--------------|------------|
| Most impactful project | Story 1 (GovCitizen) |
| AI/ML in production | Story 2 (Artiv) |
| Performance / optimization | Story 3 (MyCreditScore) |
| Full-stack ownership | Story 4 (iludate) |
| Working under constraints | Story 2 (Artiv) |
| Distributed systems | Story 1 or 3 |
| Technical leadership / mentorship | Story 1 (GovCitizen — set standards, led team) |
| Tradeoffs you made | Story 2 (local LLM vs hosted) |
| Failure or what you'd do differently | Story 1 (contract testing) or Story 3 (actor model earlier) |

---

*Add new stories here after each interview debrief. Format: same STAR+R structure above.*
