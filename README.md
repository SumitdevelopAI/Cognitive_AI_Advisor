# Cognitive_AI_Advisor: A Neuro-Symbolic System for Mental Health Intervention

## 1. Abstract

The **Cognitive_AI_Advisor** is an advanced research initiative designed to bridge the gap between statistical Large Language Models (LLMs) and symbolic cognitive architectures. While LLMs excel at linguistic fluency, they lack grounded reasoning, long-term consistency, and verifiable safety—critical requirements for mental health applications. This project implements a **Hybrid Neuro-Symbolic System** based on the **Standard Model of the Mind**, integrating **ACT-R** and **SOAR** architectures with pretrained transformer encoders (**RoBERTa, DeBERTa**) to create an agent capable of reasoning, planning, and maintaining hierarchical memory for safe, preventative mental health support.

---

## 2. Theoretical Framework

This project operates on the **Dual-Process Theory** of cognition (Evans & Stanovich, 2013), emulating human cognitive stratification:

* **System 1 (Intuitive/Neural):** Handles rapid pattern matching, emotion detection, and linguistic parsing. Implemented via **Deep Learning (Transformers)**.
* **System 2 (Deliberative/Symbolic):** Handles slow, sequential reasoning, planning, and rule adherence. Implemented via **Cognitive Architectures (ACT-R, SOAR)**.

### 2.1 The Standard Model of the Mind
We adhere to the *Common Model of Cognition* (Laird, Lebiere, & Rosenbloom, 2017), which postulates that an intelligent agent must possess independent modules for:
1.  **Perception** (Neural Encoders)
2.  **Motor Action** (NLG/Response Generation)
3.  **Working Memory** (Buffers)
4.  **Long-Term Memory** (Procedural & Declarative)
5.  **Cognitive Cycle** (Central production system linking the above)

---

## 3. High-Level System Architecture

The system is deployed as a distributed microservices architecture, ensuring scalability and strict governance.

```mermaid
graph LR
    subgraph Client_Tier ["Client Tier"]
        UI[("Web Interface")]
        SecureSocket[("Secure WebSocket (WSS)")]
    end

    subgraph Gateway_Tier ["API Gateway & Governance"]
        LB[("Load Balancer")]
        Auth[("Auth Service (JWT)")]
        PII_Mask[("PII Masking Layer")]
    end

    subgraph Compute_Tier ["Inference Cluster (Kubernetes)"]
        direction TB
        Orch[("Orchestrator")]
        
        subgraph Pods ["Model Pods"]
            Model_Main[("Cognitive Agent Container<br/>(ACT-R + RoBERTa)")]
            Model_Safety[("Safety Sentinel Container<br/>(DistilBERT)")]
        end
    end

    subgraph Data_Tier ["Persistence Layer"]
        Mongo[("MongoDB<br/>(User Meta-Data)")]
        Pinecone[("Vector Store<br/>(Episodic Memory)")]
    end

    subgraph Guardian_Tier ["Escalation & Monitoring"]
        Risk_Engine[("Risk Analysis Engine")]
        Notification[("Escalation API")]
    end


    %% Flow
    UI --> SecureSocket --> LB --> Auth --> PII_Mask
    PII_Mask --> Orch --> Model_Main
    Model_Main --> Pinecone & Mongo
    Model_Main -- "Candidate Response" --> Model_Safety
    Model_Safety -- "Approved" --> SecureSocket
    Model_Safety -- "Flagged" --> Risk_Engine --> Notification



