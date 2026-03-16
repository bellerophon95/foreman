# Low-Cost Deployment Plan (Chunk 0)

## Objective
Deploy the Foundation MVP for < 10 users at near-zero incremental cost using the Google Cloud Free Tier and strategic component choices.

## Component Breakdown

| Component | Choice | Cost (for < 10 users) |
|---|---|---|
| **Audit Log** | BigQuery | **$0** (Free Tier: 10GB storage / 1TB query) |
| **State Management** | Firestore | **$0** (Free Tier: 1GB storage / 50k reads / 20k writes) |
| **Orchestration** | Cloud Run | **$0** (Free Tier: 2M requests / 180k vCPU-secs) |
| **LLM Inference** | Gemini 1.5 Pro | **$0** (via AI Studio Free Tier / Vertex Trial Credits) |
| **Vector Search** | *Optimization Required* | **$0 - $Low** (See Strategy below) |

## Cost Optimization Strategy

### 1. Vector Search Options
Vertex AI Vector Search is the only component that typically carries a baseline cost. For a < 10 user MVP, we recommend:
- **Option A (Trial)**: Use the **$1,000 Vertex AI free credits** (lasts 12 months).
- **Option B (Serverless)**: Use **pgvector on Cloud SQL** (very low cost for small datasets) or a free-tier external provider like Supabase/Neon.
- **Option C (Local Index)**: For simple sanctions checks, the "Basic Sanctions Index" can be stored as a flat file or a local FAISS index within the Cloud Run container memory (0 additional cost).

### 2. Networking
- **VPC Service Controls**: Avoid expensive NAT Gateways by using VPC Peering or keeping services within the same region and using Private Service Connect where possible for the Free Tier limit.

### 3. Monitoring
- **Cloud Logging/Trace**: Keep within the 50GiB/month free ingestion limit.

## Verdict
**Yes.** By utilizing "Always Free" services and being selective with the Vector Search implementation (e.g., local index or trial credits), Chunk 0 can be run for **$0 USD/month** for a small pilot group.
