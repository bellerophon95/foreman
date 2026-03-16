# Chunk 4: Perpetual KYC MVP

## Goal
Transition from periodic KYC reviews to event-driven continuous risk assessment and complex ownership mapping.

## Scope
- Perpetual KYC Agent
- Beneficial Owner Agent (UBO Mapping)
- Neo4j Entity Graph integration
- Adverse News ETL (Dow Jones 15min sync)
- Regulatory Feed ingestion

## Atomic Tasks
- [ ] Implement Perpetual KYC Agent for event-driven re-reviews.
- [ ] Implement Beneficial Owner Agent for UBO mapping from corporate registries.
- [ ] Integrate Neo4j for 4-hop entity relationship traversal.
- [ ] Implement adverse news search tools with Dow Jones/Refinitiv.
- [ ] Setup regulatory feed subscription (FinCEN/AMLA) for automatic rule updates.

## Verification
- [ ] **Performance**: Neo4j ownership traversal (4-hop) < 200ms.
- [ ] **Functional**: System automatically triggers a KYC re-review upon a "Significant Control" change event in registry.
- [ ] **Scale**: System tracks Ownership Structure for entities with > 5 layers of shell companies.
