# Chunk 0: Foundation MVP

## Goal
Establish the core infrastructure required for Google Antigravity, including VPC isolation, multi-tenant vector search, and an immutable audit log.

## Scope
- GCP Project setup (VPCs, IAM)
- Multi-tenant Vertex AI Vector Search
- BigQuery Audit Store (Append-only)
- Firestore Agent State management
- Basic Sanctions Index (OFAC)
- L0 Eval Harness (Individual tool call validation)

## Atomic Tasks
- [ ] Setup GCP VPCs and IAM roles for multi-tenant isolation.
- [ ] Initialize Vertex AI Vector Search with merchant/institution namespace structure.
- [ ] Create BigQuery tables with the immutable audit log schema.
- [ ] Configure Firestore for per-case/per-agent state tracking.
- [ ] Implement ETL pipeline for OFAC SDN List (4h sync).
- [ ] Setup L0 eval framework in CI/CD.

## Verification
- [ ] **Technical**: `gcloud` commands verify VPC and IAM configuration.
- [ ] **Technical**: BigQuery query confirms schema and append-only status.
- [ ] **Technical**: CI/CD pipeline runs L0 tests on commit.
- [ ] **Functional**: System can retrieve a known sanctioned entity from the vector index.
