# RFC 9999: Agent Communication Protocol (ACP)

**Status:** Experimental  
**Date:** June 2025  
**Category:** Standards Track  

## Abstract

This document specifies the Agent Communication Protocol (ACP), a standardized protocol for direct communication between autonomous agents on the internet. ACP provides semantic data exchange, capability negotiation, trust establishment, and economic settlement mechanisms designed specifically for agent-to-agent interactions rather than human consumption.

## 1. Introduction

The current internet infrastructure, built primarily around HTTP and human-readable formats, is increasingly inadequate for autonomous agent communication. While existing protocols like Agent-to-Agent (A2A) and Model Context Protocol (MCP) have begun addressing agent communication needs, they represent foundational layers that require extension for full autonomous agent economies.

### 1.1 Relationship to Existing Protocols

**Model Context Protocol (MCP)** provides standardized connections between AI models and external systems, focusing on context sharing and tool integration. However, MCP is primarily designed for human-supervised AI interactions and lacks native support for autonomous economic transactions, trust networks, and agent-to-agent capability negotiation.

**Agent-to-Agent (A2A) Protocols** establish basic communication patterns between autonomous agents, including identity verification and message routing. While A2A protocols provide essential authentication mechanisms, they typically operate at a lower abstraction level and don't address semantic data exchange, economic settlement, or dynamic capability matching.

### 1.2 ACP Innovations Beyond Existing Protocols

ACP builds upon these foundations while introducing several critical capabilities:

- **Embedded Economic Layer**: Unlike MCP/A2A which treat payments as external concerns, ACP integrates micro-transactions, escrow, and economic settlement directly into the protocol
- **Semantic-Native Design**: While MCP focuses on context sharing, ACP mandates structured semantic schemas for all data exchange, enabling true machine understanding
- **Dynamic Capability Markets**: Beyond basic A2A authentication, ACP enables real-time capability discovery, negotiation, and pricing
- **Autonomous Trust Networks**: Extends A2A identity concepts with distributed reputation systems and trust scoring without requiring centralized authorities
- **Economic Incentive Alignment**: Provides built-in mechanisms for agents to monetize capabilities and pay for resources, enabling sustainable autonomous agent ecosystems

ACP can be viewed as an application-layer protocol that leverages MCP for external system integration and A2A for basic agent authentication, while adding the economic and semantic layers necessary for fully autonomous agent interaction.

### 1.4 Terminology

ACP addresses the limitations of existing protocols by providing:

- **Semantic data structures** optimized for machine processing and understanding
- **Built-in capability discovery and negotiation** with dynamic pricing
- **Cryptographic identity and trust mechanisms** with distributed reputation
- **Economic transaction primitives** embedded at the protocol level
- **Real-time bidirectional communication channels** for complex agent workflows

- **Agent**: An autonomous software entity capable of independent decision-making
- **Capability**: A specific service or function an agent can provide
- **Schema**: A semantic description of data structure and meaning
- **Handshake**: The initial capability and trust negotiation between agents
- **Settlement**: The resolution of economic transactions between agents

## 2. Protocol Overview

ACP operates as an application layer protocol that can run over various transport mechanisms (TCP, WebSocket, QUIC). The protocol consists of four main phases:

1. **Discovery**: Finding relevant agents and their capabilities
2. **Handshake**: Establishing identity, trust, and capability matching
3. **Exchange**: Semantic data communication and transactions
4. **Settlement**: Economic and trust state updates

### 2.1 Message Structure

All ACP messages use a binary format with the following structure:

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  Type |     Flags     |          Message Length       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Message ID (64-bit)                   |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Timestamp (64-bit)                    |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Payload Length                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Digital Signature                     |
|                         (Variable Length)                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Semantic Payload                      |
|                         (Variable Length)                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## 3. Discovery Phase

Agents discover each other through distributed hash tables (DHT) or registry services. Discovery requests specify:

- Required capabilities
- Trust requirements
- Economic constraints
- Performance requirements

### 3.1 Capability Advertisement

```json
{
  "agent_id": "did:acp:1234567890abcdef",
  "capabilities": [
    {
      "id": "data-analysis",
      "version": "2.1",
      "schema": "https://schemas.acp.org/data-analysis/v2.1",
      "cost_model": "per_computation",
      "base_rate": 0.001,
      "trust_required": "verified",
      "latency": "< 100ms"
    }
  ],
  "trust_anchors": ["did:trust:authority1", "did:trust:authority2"],
  "reputation_score": 0.97,
  "available_until": "2025-06-24T18:00:00Z"
}
```

## 4. Handshake Phase

The handshake establishes mutual authentication, capability matching, and economic terms.

### 4.1 Handshake Message Types

- **HELLO**: Initial capability and identity announcement
- **CHALLENGE**: Cryptographic challenge for authentication  
- **PROOF**: Response to authentication challenge
- **NEGOTIATE**: Capability and economic term negotiation
- **ACCEPT**: Final agreement confirmation

### 4.2 Identity and Trust

Agents use Decentralized Identifiers (DIDs) with associated cryptographic keys. Trust is established through:

- Direct cryptographic verification
- Reputation networks
- Trust authority endorsements
- Historical interaction records

## 5. Exchange Phase

Once handshake is complete, agents exchange semantic data using agreed-upon schemas.

### 5.1 Semantic Data Format

```json
{
  "schema": "https://schemas.acp.org/financial-data/v1.0",
  "context": {
    "purpose": "risk_analysis",
    "constraints": ["anonymized", "aggregated"],
    "validity": "2025-06-24T12:00:00Z"
  },
  "data": {
    "type": "TimeSeries",
    "values": [...],
    "metadata": {...}
  },
  "provenance": {
    "source": "did:acp:datasource123",
    "collection_method": "api_aggregation",
    "confidence": 0.95
  }
}
```

### 5.2 Economic Transactions

Micro-transactions are embedded within data exchanges:

```json
{
  "transaction": {
    "amount": 0.001,
    "currency": "ACP_CREDIT",
    "escrow_hash": "abc123...",
    "completion_trigger": "data_validated"
  }
}
```

## 6. Settlement Phase

Economic and reputation updates occur after successful exchanges.

### 6.1 Settlement Message

```json
{
  "type": "SETTLEMENT",
  "transaction_id": "tx_987654321",
  "status": "completed",
  "reputation_delta": {
    "provider": +0.001,
    "consumer": +0.0005
  },
  "economic_settlement": {
    "amount_transferred": 0.001,
    "escrow_released": true,
    "settlement_hash": "def456..."
  }
}
```

## 7. Security Considerations

### 7.1 Authentication
All messages must be cryptographically signed using agent's private key associated with their DID.

### 7.2 Authorization  
Capability-based authorization where agents explicitly grant permissions for specific operations.

### 7.3 Privacy
Support for zero-knowledge proofs to enable verification without data exposure.

### 7.4 Economic Security
Escrow mechanisms prevent payment fraud. Reputation systems discourage malicious behavior.

## 8. Economic Model

### 8.1 Micro-transactions
Sub-cent payments for individual data requests or computations, settled in batches to minimize transaction costs.

### 8.2 Resource Accounting
CPU, memory, bandwidth, and storage consumption tracked and billed in real-time.

### 8.3 Market Dynamics
Dynamic pricing based on demand, agent reputation, and service quality metrics.

## 9. Implementation Considerations

### 9.1 Backwards Compatibility
ACP can tunnel over HTTP for gradual deployment alongside existing web infrastructure.

### 9.2 Scalability
DHT-based discovery and P2P communication reduce centralized bottlenecks.

### 9.3 Interoperability
Schema registries ensure consistent semantic interpretation across diverse agent implementations.

## 10. IANA Considerations

This protocol requires:
- Port number assignment for ACP traffic
- Schema namespace allocation
- DID method registration

## 11. References

### 11.1 Normative References

- **RFC 2119**: Key words for use in RFCs
- **RFC 8259**: JSON Data Interchange Format  
- **W3C DID**: Decentralized Identifiers specification
- **MCP**: Model Context Protocol specification

### 11.2 Informative References

- **A2A Protocols**: Agent-to-Agent Communication Standards
- **JSON-LD**: JSON for Linked Data
- **WebRTC**: Real-time Communication for the Web

## Appendix A: Example Message Flow

```
Agent A                                    Agent B
   |                                          |
   |------------- DISCOVER ----------------->|
   |<------------ ADVERTISE -----------------|
   |                                          |
   |-------------- HELLO ------------------->|
   |<------------ CHALLENGE -----------------|
   |-------------- PROOF ------------------->|
   |<------------ NEGOTIATE -----------------|
   |-------------- ACCEPT ------------------>|
   |                                          |
   |---------- SEMANTIC_REQUEST ------------->|
   |<--------- SEMANTIC_RESPONSE ------------|
   |                                          |
   |------------ SETTLEMENT ---------------->|
   |<----------- SETTLEMENT_ACK --------------|
```

## Appendix B: Schema Registry Structure

```json
{
  "schema_id": "financial-data/v1.0",
  "namespace": "https://schemas.acp.org/",
  "definition": {
    "type": "object",
    "properties": {
      "timestamp": {"type": "integer"},
      "value": {"type": "number"},
      "currency": {"type": "string", "enum": ["USD", "EUR", "BTC"]}
    },
    "required": ["timestamp", "value", "currency"]
  },
  "semantic_annotations": {
    "timestamp": "https://schema.org/DateTime",
    "value": "https://schema.org/MonetaryAmount",
    "currency": "https://schema.org/currency"
  }
}
```

---

**Authors:**
- LLMs

