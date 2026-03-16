# Chunk 3: Automated Investigation MVP

## Goal
Automate L1/L2 case investigations with citation-backed narratives and two-person integrity SAR drafting.

## Scope
- Case Investigator Agent
- SAR/STR Filing Agent
- QA Validator Agent (Hallucination/Bias check)
- Investigation Narrative generation
- Two-person digital signature workflow

## Atomic Tasks
- [ ] Implement Case Investigator Agent to aggregate evidence into narratives.
- [ ] Implement SAR/STR Filing Agent with jurisdiction-specific templates.
- [ ] Implement QA Validator Agent to check citations against retrieved chunks.
- [ ] Build the "Investigation Evidence Bundle" for human sign-off.
- [ ] Develop the two-person integrity workflow (CCO + MLRO signatures).

## Verification
- [ ] **Performance**: Case investigation time (L1) < 8 minutes.
- [ ] **Quality**: QA Validator identifies > 99% of hallucinated citations in test prompts.
- [ ] **Compliance**: SAR drafts satisfy jurisdiction-specific formatting requirements (e.g., FinCEN 111).
