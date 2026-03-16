# Chunk 4: Perpetual KYC MVP (No-Cost Optimized)

## Goal
Transition from periodic KYC reviews to event-driven continuous risk assessment using free/trial-compatible data sources and ownership mapping.

## Scope
- Perpetual KYC Agent
- Beneficial Owner Agent (UBO Mapping)
- Neo4j Entity Graph integration (Aura Free Tier)
- **Adverse News Alternative**: Google Search API / Custom Search Engine (100 free queries/day)
- Regulatory Feed ingestion (Public RSS feeds)

## Atomic Tasks
- [ ] Implement Perpetual KYC Agent for event-driven re-reviews.
- [ ] Implement Beneficial Owner Agent for UBO mapping from corporate registries.
- [ ] Integrate Neo4j Aura Free Tier for relationship traversal.
- [ ] **Implement Adverse News tool**: Use Google Custom Search Engine (CSE) to simulate adverse news screening at $0 cost.
- [ ] Setup regulatory feed subscription (FinCEN/AMLA) for automatic rule updates.

## No-Cost Guardrails
- **Graph DB**: Monitor Neo4j Aura node limits (200k limit).
- **News Screening**: Limit automated news checks to 100/day to stay within CSE free tier.

## Verification
- [ ] **Performance**: Neo4j ownership traversal (4-hop) < 200ms.
- [ ] **Functional**: System triggers a KYC re-review upon a "Significant Control" change event.
- [ ] **Scale**: System tracks Ownership Structure for entities with > 5 layers using free-tier graph resources.
