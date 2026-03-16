# Chunk 1: KYC Onboarding MVP

## Goal
Deploy a functional KYC Onboarding Agent capable of processing document verification and PEP/sanctions screening with high accuracy for a mid-size US bank pilot.

## Scope
- KYC Onboarding Agent Implementation
- Identity Verification Agent (Deepfake/Liveness detection integration)
- PEP & Sanctions Screening integration
- Risk Tier Assignment Logic
- Human-in-the-loop review queue (UI skeleton)
- L1 Eval suite (Confirmed KYC cases)

## Atomic Tasks
- [ ] Implement KYC Onboarding Agent with document recognition tool.
- [ ] Integrate Identity Verification tools for liveness checks.
- [ ] Connect KYC agent to the Sanctions/PEP index from Chunk 0.
- [ ] Develop risk tiering logic based on entity profile and screening hits.
- [ ] Create an "Evidence Bundle" output schema for analyst review.
- [ ] Build the L1 golden dataset with at least 50 historical KYC outcomes.

## Verification
- [ ] **Performance**: KYC onboarding latency p95 < 3 minutes.
- [ ] **Accuracy**: Agent matches historical risk assignments on golden dataset with > 85% agreement.
- [ ] **UI/UX**: Evidence bundle contains clickable citations to retrieved RAG chunks.
