# Chunk 0: Foundation MVP (No-Cost Optimized)

## Goal
Establish the core infrastructure required for Google Antigravity, including VPC isolation, multi-tenant vector search, and an immutable audit log, utilizing the $300 Trial Credits and Free Tier.

## Scope
- GCP Project setup (VPCs, IAM)
- Multi-tenant Vertex AI Vector Search (Utilizing $300 Credits)
- BigQuery Audit Store (Always Free Tier: 10GB/1TB)
- Firestore Agent State management (Always Free Tier: 1GB/50k reads)
- Basic Sanctions Index (OFAC)
- L0 Eval Harness (Individual tool call validation)

## Atomic Tasks
- [ ] Setup GCP VPCs and IAM roles for multi-tenant isolation.
- [ ] Initialize Vertex AI Vector Search with merchant/institution namespace structure.
- [ ] Create BigQuery tables with the immutable audit log schema.
- [ ] Configure Firestore for per-case/per-agent state tracking.
- [ ] Implement ETL pipeline for OFAC SDN List (4h sync).
- [ ] Setup L0 eval framework in CI/CD.

## No-Cost Guardrails
- **Vector Search**: Monitor usage to stay within the $300 trial credit limit.
- **Audit Store**: Set up BigQuery usage alerts at 8GB (80% of free tier).
- **Compute**: Use Cloud Run with min-instances=0 to ensure $0 idle cost.

## Verification
- [ ] **Technical**: `gcloud` commands verify VPC and IAM configuration.
- [ ] **Technical**: BigQuery query confirms schema and append-only status.
- [ ] **Functional**: System can retrieve a sanctioned entity from the vector index.
