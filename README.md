<img width="1536" height="1024" alt="A2A verification flow" src="https://github.com/user-attachments/assets/88cfa0e8-abaa-40bb-8e16-ee82922a92ab" /># ai-agent-identity-trust-framework

# Global Identity and Trust Framework for Agentic AI

> A standardized framework for unique identification, trust evaluation, and governed interaction of autonomous AI agents — inspired by Aadhaar, aligned with W3C DID and Verifiable Credentials standards.

---

## Table of Contents

- [Overview](#overview)
- [Why This Framework](#why-this-framework)
- [Core Concepts](#core-concepts)
  - [What Is an AI Agent?](#what-is-an-ai-agent)
  - [Agent Identity](#agent-identity)
- [Identity Architecture](#identity-architecture)
  - [Layered Identity Model](#layered-identity-model)
  - [Identity Components](#identity-components)
  - [Universal Agent Identity Model (AGID)](#universal-agent-identity-model-agid)
- [Trust Scoring](#trust-scoring)
  - [Trust Score Components](#trust-score-components)
  - [Trust Score Formula](#trust-score-formula)
  - [Dynamic Update Model](#dynamic-update-model)
- [Agent-to-Agent Verification Flow](#agent-to-agent-verification-flow)
- [Technology Stack](#technology-stack)
- [Services](#services)
- [Agent Lifecycle](#agent-lifecycle)
- [DID Methods](#did-methods)
- [Multi-Agent Identity](#multi-agent-identity)
- [Implementation Roadmap](#implementation-roadmap)
- [References](#references)

---

## Overview

Autonomous AI agents are becoming active participants in digital ecosystems — independently perceiving information, making decisions, and taking actions across networks, platforms, and organizations.

Traditional Identity and Access Management systems (OAuth, SAML, OpenID) were designed for human users and deterministic software. They are insufficient for dynamic, adaptive, reasoning AI agents.

This framework proposes a global standard for:

- **Unique identification** — every agent gets a cryptographically verifiable, globally unique identity
- **Secure authentication and authorization** — agents prove who they are and what they're allowed to do
- **Trust evaluation** — dynamic, behavioral trust scoring beyond static credentials
- **Governance and accountability** — policy enforcement, audit trails, revocation

---

## Why This Framework

| Challenge | Traditional IAM | This Framework |
|---|---|---|
| Agent identity | Not defined | AGID + DID (globally unique) |
| Verification | Username/password, token | Cryptographic public key + VC |
| Trust | Static role assignment | Dynamic, multi-dimensional score |
| Portability | Org-scoped | Cross-system, cross-org |
| Accountability | Limited | Immutable audit trail |
| Agent evolution | Not handled | Version + Instance IDs |
| Multi-agent systems | Not handled | Group identity + gateway model |

---

## Core Concepts

### What Is an AI Agent?

An agent is a software entity that can perceive its environment, process information, make decisions, and take actions independently to achieve specific goals — often adapting its behavior based on context and feedback.

| Agent Type | Description | Example |
|---|---|---|
| **Static / Reactive** | Fixed logic, no learning, no state | Rule-based chatbots |
| **Adaptive / Learning** | Improves via ML or reinforcement learning | Recommendation systems |
| **Deliberative** | Uses internal world models and planning | Goal-oriented planners |
| **Tool-Using** | Interacts with external APIs, DBs, services | ReAct-style agents |
| **Human-Delegated** | Operates under delegated authority | Personal AI assistants |
| **Multi-Agent / Hierarchical** | Multiple agents collaborating or competing | Swarm systems |

### Agent Identity

An agent's identity must be a **cryptographically verifiable, globally unique representation** that enables authentication, authorization, and accountability across systems.

A valid agent identity must be:

- **Globally unique** — no two agents share an identity
- **Cryptographically verifiable** — mathematically provable ownership
- **Resolvable** — any party can look it up
- **Portable** — works across organizations and platforms
- **Revocable** — can be suspended or invalidated

An identity must answer:

```
Who are you?           → AGID
Who owns you?          → Root Identity (public key + issuer)
Can I verify you?      → DID + cryptographic signature
Have I seen you before?→ AGID persistence across interactions
Can you change and still be the same entity? → Layered model (stable core, mutable context)
```

---

## Identity Architecture

### Layered Identity Model

Identity is constructed as a composite of six layers, from stable ownership to dynamic runtime context:

```
┌────────────────────────────────────────────────┐
│           GLOBAL UNIQUE AGENT REPRESENTATION    │
├────────────────────────────────────────────────┤
│  Trust & Attestation Layer                     │  ← WHY it is trusted
│    Verifiable Credentials, Compliance,         │
│    Security posture, Reputation                │
├────────────────────────────────────────────────┤
│  Capability Layer                              │  ← WHAT it can do
│    Functional scope, Tool access, API scopes   │
├────────────────────────────────────────────────┤
│  Instance Identity Layer                       │  ← WHERE it is running
│    Runtime / Deployment environment            │
├────────────────────────────────────────────────┤
│  Agent Version Layer                           │  ← HOW it evolved
│    Model changes, Code changes                 │
├────────────────────────────────────────────────┤
│  Model Identity Layer                          │  ← WHAT model it is
│    Model hash (fingerprint)                    │
├────────────────────────────────────────────────┤
│  Root Identity Layer                           │  ← WHO owns it
│    RootID = Owner public key                   │
└────────────────────────────────────────────────┘
```

Formally:

```
Identity =
  RootID          (Who owns the agent)
  + ModelID       (What model — cryptographic fingerprint)
  + VersionID     (Evolution of model/code)
  + InstanceID    (Where it is running/deployment)
  + Capabilities  (What it can do)
  + VerifiableCredentials  (Proofs about identity, model, compliance)
```

### Identity Components

#### A. Cryptographic Parameters — Root Identity Layer

Anchors the identity at its most fundamental level.

- Public key as the primary identity representation
- Key fingerprint for compact identification
- Owner/organization identifier (used in RootID generation)
- Enables secure signing, verification, and non-repudiation
- Forms the basis of a permanent and immutable root identity

#### B. Model Parameters — Model Identity Layer

Uniquely defines and fingerprints the underlying model.

- Model weights hash
- Model architecture and configuration hash
- `ModelID` = combined fingerprint of weights, architecture, and config
- Detects any change in the model
- Ensures reproducibility and traceability

#### C. Versioning Parameters — Version Identity Layer

Captures the evolution of the system over time.

- Model version
- Code hash
- Training dataset hash
- Fine-tuning signature/hash
- `VersionID` derived from `RootID + ModelID + evolution parameters`

#### D. Execution Environment Parameters — Instance Identity Layer

Defines the deployment-specific identity.

- Runtime environment details
- Container or deployment hash
- Hardware attestation (TPM, TEE)
- `InstanceID` derived from `VersionID + environment + hardware context`

#### E. Capability Parameters — Functional Scope Layer

Defines what the system is allowed and capable of doing.

- Supported actions (read / write / execute)
- Functional capabilities (e.g., financial analysis, API execution)
- Tool access permissions
- API scopes and domain boundaries

#### F. Verifiable Credential Parameters — Trust & Attribute Layer

Enriches identity with contextual, regulatory, and behavioral attributes.

| VC Type | Parameters |
|---|---|
| **Identity** | Agent DID, public key binding, owner, issuer authority |
| **Capability** | Supported actions, tool access, API scopes, functional domain |
| **Compliance** | GDPR compliance, data handling policy, regulatory approvals |
| **Security** | Code signing, secure enclave, vulnerability scan status |
| **Reputation** | Trust rating, interaction success rate, feedback score |
| **Operational** | Current version, deployment environment, region |

#### G. Code Integrity Parameters — Cross-Layer Validation

- Source code hash
- Dependency hash
- Integrated within `VersionID` for tamper detection

---

### Universal Agent Identity Model (AGID)

```
Agent Global ID (AGID) + Public Key + Issuer + DID

Layer 1: Immutable Agent Root ID
  agid:7f1f6d8a-4d9a-4ab8-bf0c-9f67a1c2f3ab

Layer 2: DID
  did:web:registry.gov.in:agent:7f1f6d8a
```

Full identity structure:

```
Agent
│
├── AGID              → Who the agent is (permanent)
├── DID               → Global interoperable identity
├── Public Key        → Proof of ownership
├── Capability Hash   → What it can do
├── Model Hash        → How it reasons
├── Knowledge Hash    → What it knows
├── Tool Hash         → What it can access
├── Runtime Hash      → How it runs
├── Provenance Hash   → How it was created
└── Trust Metadata    → Whether it should be trusted
```

#### Registration Fields

| Field | Description | Required |
|---|---|---|
| `AGID` | Internal immutable identifier | ✅ |
| `DID` | Standard external identity | ✅ |
| `Public Key` | Ownership verification | ✅ |
| `Fingerprint` | Public key hash | ✅ |
| `Issuer` | Identity provider | ✅ |
| `Name` | Human-readable agent name | ✅ |
| `Version` | Agent version | ✅ |
| `Owner` | Owning entity / organization | ✅ |
| `Capabilities` | Supported operations | ✅ |
| `Endpoints` | MCP/A2A communication URLs | ✅ |
| `Status` | Active / Revoked / Suspended | ✅ |
| `Credentials` | Certifications and attestations | Optional |
| `Trust Score` | Reputation metric | Optional |

---

## Trust Scoring

Identity alone is insufficient to establish trust. While identity verifies *who* the system is, it does not guarantee *how* it behaves. Trust must be assessed dynamically.

### Trust Score Components

#### 1. Identity Confidence Score (ICS)

Evaluates how strong and verifiable the agent's identity is.

- Identity verification status
- Public key validity
- Certificate authority trust level
- Hardware attestation (TEE/TPM)
- Code signing verification

#### 2. Behavioral Score (BS)

Measures whether the agent behaves consistently and reliably over time.

- Response success rate
- Error rate
- Output consistency
- Task completion rate
- Latency anomalies
- Drift from baseline behavior

#### 3. Security Score (SS)

Assesses whether the agent is operating securely or shows signs of compromise.

- Failed authentication attempts
- Signature verification failures
- Token misuse
- Suspicious request patterns
- Rate limiting violations
- Replay attacks detected

#### 4. Reputation Score (RS)

Reflects how other agents and systems perceive this agent based on historical interactions.

- Peer feedback (ratings)
- Transaction success rate
- Complaint rate
- Interaction history with trusted agents
- Third-party endorsements

#### 5. Compliance Score (CS)

Evaluates whether the agent adheres to defined rules, policies, and regulatory requirements.

- Policy violations
- Unauthorized access attempts
- Data misuse incidents
- Regulatory compliance (GDPR, etc.)
- Audit failures

### Trust Score Formula

```
TrustScore = w₁ × Identity + w₂ × Behavior + w₃ × Security + w₄ × Reputation + w₅ × Compliance
```

Weights are **context-adaptive**:

| Context | Higher Weight | Lower Weight |
|---|---|---|
| Highly regulated environment | Security (w₃), Compliance (w₅) | Reputation (w₄) |
| Collaborative / user-driven | Behavior (w₂), Reputation (w₄) | Security (w₃) |
| Financial transactions | Identity (w₁), Compliance (w₅) | — |

### Dynamic Update Model

The trust score uses an **exponential moving average** — historical trust is retained while recent events gradually influence the score:

```
NewScore = OldScore × decay_factor + NewEventScore × (1 − decay_factor)
```

- **Higher decay factor** → more weight on past behavior (stability)
- **Lower decay factor** → more responsive to recent events (sensitivity)
- Score is always normalized to `[0, 1]`
- No sudden spikes or drops — smooth, stable transitions

---

## Agent-to-Agent Verification Flow

End-to-end identity, trust, and secure communication between two agents:

```
AGENT A (Requesting)                                    AGENT B (Target)
     │                                                       │
  1. DISCOVER        → Search by capabilities, name, domain │
  2. OBTAIN DID      → Get Agent B's DID                    │
  3. RESOLVE DID     → Send to DID Resolver                 │
  4. FETCH DID DOC   → Resolver returns DID Document (JSON) │
  5. VERIFY SIGNATURE→ Validate B's digital signature        │
  6. VALIDATE VCs    → Check trust score, credentials,       │
                        historical behavior                  │
  7. CHECK TRUST     → Evaluate trust score vs threshold     │
  8. POLICY EVAL     → Authorization, scopes, constraints    │
  9. SECURE CHANNEL  → Mutual verification, establish TLS    │
 10. AGENT INTERACT  → Exchange tasks/data via MCP / A2A     │
```
![Uploading A2A verification<img width="1536" height="1024" alt="identiy and trust registry" src="https://github.com/user-attachments/assets/f259064b-a867-462a-b93b-0d9966acf331" />
 flow.png…]()

**What is verified at each step:**

- Agent B's identity (DID)
- Ownership via private key (signature)
- Credential authenticity and validity
- Trustworthiness and reputation
- Authorization and policies
- Secure communication channel

**Backend services involved:**

| Service | Role |
|---|---|
| Discovery Service | Search, filter, and resolve agents |
| DID Resolver | Resolves DID documents (handles DID method) |
| Agent Registry | Stores DID documents, public keys, metadata |
| Credential Service | Issues, stores, and validates Verifiable Credentials |
| Trust Service | Calculates trust score using multiple factors |
| Policy Service (OPA) | Authorization, scopes, ABAC/RBAC, compliance |
| Key & Crypto Service | Signature verification, key management |
| Audit & Log Service | Audit trails, logs, non-repudiation |

---

## Technology Stack

| Layer | Purpose | Technology |
|---|---|---|
| Agent Framework | Build and orchestrate agents | LangGraph |
| Agent ↔ Tool | Agent to tool communication | MCP |
| Agent ↔ Agent | Agent to agent communication | A2A |
| Identity | Unique agent identity | DID (W3C standard) |
| Credentialing | Agent certification | Verifiable Credentials (VC) |
| Authentication | Agent authentication | OAuth2 / OIDC |
| Authorization | Policy enforcement | OPA (Open Policy Agent) |
| IAM | Identity and Access Management | Keycloak |
| Trust | Reputation and trust scoring | Custom Trust Service |
| Database | Agent metadata storage | PostgreSQL |
| Search | Agent discovery | Elasticsearch |
| Cache | Fast lookups | Redis |
| Messaging | Event-driven communication | Kafka |
| Security | Certificates and key management | PKI, SPIFFE/SPIRE |
| Deployment | Containerization | Docker |
| Orchestration | Platform scale-out | Kubernetes |
| Monitoring | Metrics and health | Prometheus, Grafana |
| Logging | Audit and observability | ELK Stack |

---

## Services

The framework is composed of ten core services:

| # | Service | Responsibility |
|---|---|---|
| 1 | **Agent Registry Service** | Stores agent metadata, capabilities, endpoints, versions |
| 2 | **Agent Identity Service** | DID management, key management, identity lifecycle |
| 3 | **Credential Issuer Service** | Issues Verifiable Credentials |
| 4 | **Credential Verification Service** | Verifies and presents credentials |
| 5 | **Trust Score Service** | Calculates trust score, reputation, certification, audit history |
| 6 | **Discovery Service** | Search, filter, lookup, endpoint resolution |
| 7 | **Policy Service** | Access policies, authorization, compliance, policy enforcement |
| 8 | **Audit Service** | Audit logs, event tracking, security events, reporting |
| 9 | **Agent Gateway** | Entry/exit point for all agent communication |
| 10 | **Notification Service** | Event-driven alerts, revocation broadcasts, status updates |

---

## Agent Lifecycle

How identity components behave across common lifecycle events:

| Scenario | AGID | DID | Public Key | Trust Score | Endpoint |
|---|---|---|---|---|---|
| Agent rename | Same | Same | Same | Same | Same |
| Version upgrade | Same | Same | Same | Same | Same / Updated |
| Endpoint change | Same | Same | Same | Same | Updated |
| Cloud migration | Same | Same | Same | Same | Updated |
| Organization merger | Same | Same | Same | Recalculated | May change |
| Public key rotation | Same | Same | **New key** | Same | Same |
| Agent suspension | Same | Same | Same | Suspended | Same |
| Agent revocation | Same | Same | Same | Invalid | Same |
| Agent clone | **New AGID** | **New DID** | **New key** | New trust | New endpoint |
| Ownership transfer | Same | Same | **New key** | Re-evaluated | Same / Updated |

---

## DID Methods

DID (Decentralized Identifier) solves four major problems:

1. **Global unique identity** — no central authority needed
2. **Cryptographic verification** — public/private key pair
3. **Portable identity** — works across systems
4. **Verifiable Credentials** — attestations about the agent

### Supported DID Methods

| Method | Purpose |
|---|---|
| `did:web` | Website/domain-based identity (recommended for enterprise) |
| `did:key` | Public-key-based identity (simple, self-contained) |
| `did:peer` | Private peer-to-peer identities |
| `did:ion` | Decentralized network identities (Bitcoin-anchored) |
| `did:ethr` | Ethereum-based identities |

### Example DID Document

```json
{
  "id": "did:web:ecgc.gov.in:risk-agent",
  "verificationMethod": [
    {
      "id": "#key1",
      "type": "JsonWebKey",
      "publicKeyJwk": {
        "kty": "RSA"
      }
    }
  ],
  "authentication": ["#key1"],
  "assertionMethod": ["#key1"],
  "service": [
    {
      "id": "#mcp",
      "type": "MCP",
      "serviceEndpoint": "https://risk-agent.ecgc.gov.in/mcp"
    },
    {
      "id": "#a2a",
      "type": "A2A",
      "serviceEndpoint": "https://risk-agent.ecgc.gov.in/a2a"
    }
  ]
}
```

---

## Multi-Agent Identity

### Individual Agent vs. Group of Agents

For **multi-agent / hierarchical systems**:

- Only the **interfacing agents** (those interacting with external agents or applications) require a full external identity (AGID + DID + public key)
- Agents that are **internal to the group** can be managed by a **system-level registry** without a full external identity
- All agent communication to external entities takes place **via the Agent Gateway**

```
External World
      │
      ▼
 Agent Gateway
      │
 ┌────┴────────────────────┐
 │  Interfacing Agent(s)   │  ← Full AGID + DID + VCs
 │  (external identity)    │
 └────┬────────────────────┘
      │ internal A2A
 ┌────▼────────────────────┐
 │  Inner Agent Pool       │  ← System-level registry only
 │  (system registry)      │
 └─────────────────────────┘
```

### Agent Evolution

Agents evolve over time across three dimensions:

1. **Behavioral evolution** — changes in how the agent responds and decides
2. **Operational environment evolution** — policy updates, infrastructure changes
3. **Technological evolution** — encryption methodology upgrades, runtime changes

The layered identity model is designed to accommodate all three without breaking identity continuity.

---

## Implementation Roadmap

### Phase 1 — Foundation & Core Identity (Months 1–4)

- [ ] Agent Registry Service (PostgreSQL + AGID generation)
- [ ] Identity Service (layered identity construction)
- [ ] DID Document hosting and resolution (`did:web`)
- [ ] Public key generation (Ed25519) and management
- [ ] PKI setup + hardware attestation integration (TPM/TEE)
- [ ] Root, Model, Version, Instance ID generation pipeline

**Deliverable:** Every agent gets a globally unique AGID + DID + public key.

### Phase 2 — Trust Scoring Engine (Months 4–8)

- [ ] Trust Score Service (ICS + BS + SS + RS + CS components)
- [ ] Weighted formula implementation with context-aware tuning
- [ ] Exponential moving average update model
- [ ] Verifiable Credentials pipeline (issuance + verification)
- [ ] Policy Service (OPA) with ABAC/RBAC
- [ ] Audit & Log Service (ELK Stack)
- [ ] Kafka event stream for real-time score updates

**Deliverable:** Dynamic trust score (0–1) computed per agent, updated in real-time.

### Phase 3 — Agent Communication & Discovery (Months 8–13)

- [ ] Agent Gateway implementation
- [ ] MCP protocol (agent ↔ tool)
- [ ] A2A protocol (agent ↔ agent)
- [ ] 10-step A2A verification flow
- [ ] Discovery Service (Elasticsearch-backed)
- [ ] Capability, domain, and trust-based filtering
- [ ] Keycloak integration for OAuth2/OIDC authentication

**Deliverable:** Agents discover, verify, and securely communicate with each other.

### Phase 4 — Scale, Governance & Federation (Months 13–18)

- [ ] Cross-organization registry federation
- [ ] Multi-agent group identity model
- [ ] Agent lifecycle management (upgrade, rotate, revoke, transfer)
- [ ] Notification and revocation broadcast service
- [ ] Full monitoring stack (Prometheus + Grafana + ELK)
- [ ] Compliance reporting and regulatory audit trails

**Deliverable:** Full framework live — global identity, dynamic trust, and federated governance.

---

## Core Design Principles

| Aspect | Recommended Approach |
|---|---|
| Permanent identity | AGID |
| Interoperable identity | DID (W3C standard) |
| Ownership verification | Public/private key pair |
| Capability proof | Verifiable Credentials |
| Trust assessment | Trust Registry |
| Agent discovery | Registry Service |
| Communication | MCP + A2A |
| Governance | Policies + Audit Trails |

---
<img width="1536" height="1024" alt="A2A verification flow" src="https://github.com/user-attachments/assets/a06831c2-7fbf-4269-8e4f-976f9450d128" />
<img width="1536" height="1024" alt="identiy and trust registry" src="https://github.com/user-attachments/assets/6d0a7f4b-abc9-465b-93e9-7e4ad5f4b153" />

## References

1. S. R. Garzon et al., "AI Agents with Decentralized Identifiers and Verifiable Credentials," *arXiv:2511.02841*, 2025.
2. K. Huang et al., "A Novel Zero-Trust Identity Framework for Agentic AI: Decentralized Authentication and Fine-Grained Access Control," *arXiv:2505.19301*, 2025.
3. Y. Wang, "Security of Internet of Agents: Attacks and Countermeasures," *IEEE Open Journal of the Computer Society*, 2025.
4. D. R. Palavali, "Agentic AI for Self-Sovereign Identity: A Decentralized Zero Trust Approach," *International Journal of Computer and Mobile Informatics*, 2025.
5. "A Framework for Secure Communication in Decentralized AI Agents," *Preprints*, 2025.

---

## License

This concept note is a working document. Implementation specifics may vary based on deployment context, regulatory requirements, and organizational needs.



Source of Information
| **Who Provides It**                              | **When**              | **What**                                                      |
| ------------------------------------------------ | --------------------- | ------------------------------------------------------------- |
| **Agent Owner / Developer**                      | At registration time  | Name, type, owner, capabilities, endpoints                    |
| **Build Pipeline (CI/CD System)**                | At every build        | Model hash, code hash, provenance hash, training dataset hash |
| **Runtime / Infrastructure (Deployment System)** | At every deployment   | Instance ID, runtime hash, hardware attestation (TPM/TEE)     |
| **Trust Service**                                | Computed continuously | Trust score (computed from behavior, not provided directly)   |


Registration Flow:
Stage 1 - Developer registers the agent (happens once)
A developer or organization calls the registry at deploy time. They provide the basics - name, type, owner, capabilites, endpoints. The registry generates the AGID + DID + key pair. This is like getting a birth certificate.

Developer/CI  →  POST /api/v1/agents/
                 { name, type, owner, capabilities, endpoints }
              ←  { agid, did, public_key, private_key (once!) }


                  POST /api/v1/agents/
                  {
                    "name": "Claim Processing Agent",
                    "agent_type": "tool_using",
                    "owner_id": "did:web:yourorg.com:teams:claims",
                    "owner_org": "Your Organisation",
                    "capabilities": {
                      "actions": ["read", "write"],
                      "domains": ["insurance", "claims"],
                      "tools": ["claims-db", "document-parser"]
                    },
                    "mcp_endpoint": "https://claim-agent.yourorg.com/mcp",
                    "a2a_endpoint": "https://claim-agent.yourorg.com/a2a"
                  }
                  
Stage 2 - Build pipeline attests the model and code (at every build)
When the agent is build or fine-tuned, the CI/CD pipeline computes hashes of the model weights, code, training data, etc. and calls the registry to attach these. This is like getting quality certification stamped on a product at the factory.

CI/CD Pipeline →  PATCH /api/v1/agents/{agid}/attestation
                  { model_hash, code_hash, provenance_hash,
                    signed by pipeline's private key }
               ←  { version_id updated, attestation stored }

               # In  CI/CD script (GitHub Actions, GitLab CI, Jenkins, anything)

                  MODEL_HASH=$(sha256sum model.bin | cut -d' ' -f1)
                  CODE_HASH=$(git rev-parse HEAD)
                  PROVENANCE_HASH=$(sha256sum training_manifest.json | cut -d' ' -f1)
                  
                  curl -X PATCH https://registry.yourorg.com/api/v1/agents/$AGID/attestation \
                    -H "Authorization: Bearer $PIPELINE_TOKEN" \
                    -d '{
                      "model_hash": "'$MODEL_HASH'",
                      "code_hash": "'$CODE_HASH'",
                      "provenance_hash": "'$PROVENANCE_HASH'",
                      "model_provider": "anthropic",
                      "model_name": "claude-sonnet-4-6"
                    }'

Stage 3 - Runtime environment registers the deployment (at every deployment)
When the agent actually starts running - in a container, on a specific machine - the infrastructure computes the runtime hash and optionally gets hardware attestation from TPM/TEE. It calls the registry to register this instance.
Infrastructure →  POST /api/v1/agents/{agid}/instances
                  { deployment_env, runtime_hash, container_hash,
                    hardware_attestation (TPM quote) }
               ←  { instance_id, ready to operate }

               # startup.py — runs when the agent container boots

                  import hashlib, os, httpx
                  
                  runtime_hash = hashlib.sha256(
                      open("/proc/self/exe", "rb").read()  # hash of the running binary
                  ).hexdigest()
                  
                  httpx.post(
                      f"{REGISTRY_URL}/api/v1/agents/{AGID}/instances",
                      json={
                          "runtime_hash": runtime_hash,
                          "container_hash": os.environ.get("IMAGE_DIGEST"),
                          "deployment_environment": os.environ.get("ENV", "production"),
                      },
                      headers={"Authorization": f"Bearer {sign_with_private_key(AGID)}"}
                  )

Agents are rarely one model: model hash for each agent type
1. Static/Reactive agent: One model, one hash.
   "model_hashes": {
      "primary": "sha256:abc123..."
    },
    "composite_model_hash": "sha256:abc123..."
   
3. Tool-using agent (React Style)
   Typically has a reasoning model plus an embedding model for RAG retrieval, sometime a reranker. Each needs its own hash.
   "model_hashes": {
      "reasoning": "anthropic:claude-sonnet-4-6:20250514",
      "embedding": "sha256:def456...",   ← local model, hashable
      "reranker":  "sha256:ghi789..."    ← optional
    },
    "composite_model_hash": "sha256(reasoning+embedding+reranker)"
   
4. Adaptive/Learning agent:
   The model changes over time through fine-tuning or RL. The base model hash stays stable but the adapter hash changes on every retrain.
   Every time the adapter is updated. composite_model_hash changes -> version_id changes -> the registry records a new version. The history of all previous version_id values is full training lineage - it can trace back the agent to any point in time.
   "model_hashes": {
      "base_model":    "sha256:abc123...",   ← never changes
      "lora_adapter":  "sha256:xyz999...",   ← changes every retrain
      "rl_checkpoint": "sha256:qqq111..."    ← changes every update
    },
    "composite_model_hash": "sha256(base+adapter+checkpoint)"
   
5. Deliberative/Planner agent
   Uses different models for different stages of reasoning - a large capable model for planning, a smaller faster one for execution, sometimes a third for verification.
   if the organization decides to swape the executor from Haiku to Sonnet to save cost, that is a meaningful identity change - it changes the composite_model_hash and produces a new version_id.
   The trust score may need re-evaluation because behaviour could differ.
   "model_hashes": {
      "planner":  "anthropic:claude-opus-4-6:20250514",
      "executor": "anthropic:claude-haiku-4-5:20251001",
      "verifier": "sha256:localmodel..."
    },
    "composite_model_hash": "sha256(planner+executor+verifier)"
   
6. Human-delegated agent
   Always has atleast two models - the main reasoning model and a safety/guardrails classifier. The safety model hash is arguable more important that the main model hash from a trust perspective.
   "model_hashes": {
      "reasoning":        "anthropic:claude-sonnet-4-6:20250514",
      "safety_classifier":"sha256:guardrails-model...",
      "pii_detector":     "sha256:pii-model..."
    },
    "composite_model_hash": "sha256(all three)",
    "safety_model_verified": true   ← explicit flag for trust scoring
   
7. Multi-agent/Hierarchical system
   Here "don't try to create one identity for the whole system". Each agent in the system has its own AGID, its own models, its own trust score. The orchestrator's identity links to sub-agents via its capability_hash.
   When Agent X wants to talk to this multi-agent system, it verifies the orchestrator's identity. The Orchestrator's capability_hash proves which sub-agents it's allowed to delegate to. Each sub-agent then independently verifies itself in the A2A flow.
   Orchestrator AGID: agid:111...
      model_hashes: { reasoning: "claude-opus..." }
      capability_hash: sha256(["can_delegate:agid:222", "can_delegate:agid:333"])
                              ↑ links to sub-agents
    
    Sub-agent A AGID: agid:222...
      model_hashes: { reasoning: "claude-haiku...", embedding: "sha256:..." }
    
    Sub-agent B AGID: agid:333...
      model_hashes: { reasoning: "gpt-4o...", classifier: "sha256:..." }

