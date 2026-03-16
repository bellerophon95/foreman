# Chunk 2: Transaction Monitoring MVP

## Goal
Implement real-time behavioral monitoring and fraud detection to reduce false positives in transaction alerts.

## Scope
- Transaction Monitor Agent
- Fraud Detection Agent
- Real-time Kafka/Pub-Sub ingestion
- Behavioral baselining logic (Entity-level)
- Simple Orchestrator (Monitoring case routing)

## Atomic Tasks
- [ ] Implement Transaction Monitor Agent for real-time behavioral pattern analysis.
- [ ] Implement Fraud Detection Agent for multi-vector scoring (ATO, synthetic ID).
- [ ] Setup streaming ingestion pipeline for core banking transaction events.
- [ ] Develop behavioral baselining for "normal" entity activity (e.g., standard payment frequency).
- [ ] Configure the Orchestrator to dispatch Monitoring/Fraud agents in parallel.

## Verification
- [ ] **Performance**: Fraud detection latency p99 < 100ms.
- [ ] **Efficiency**: False positive rate on historical transaction alerts (backtest) < 20%.
- [ ] **Audit**: Every transaction alert generates a full trace in the BigQuery audit log.
