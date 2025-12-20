# Hybrid Neuro-Symbolic Mental Health System

A **research-grade AI system** that integrates **transformer representation learning**, **large language model (LLM) reasoning**, and a **symbolic cognitive control architecture inspired by ACT-R** to produce **interpretable, temporally consistent, and safety-aware** recommendations for emotional-distress detection.

This project targets **high-stakes, human-centered AI**, where transparency, robustness, and safety are essential.

---

## Abstract

Modern deep learning models such as transformers and LLMs demonstrate strong semantic understanding but struggle with **structured reasoning**, **temporal coherence**, and **explicit safety control**—all critical in mental health applications.

This system introduces a **hybrid neuro-symbolic pipeline** that combines:

- **Transformer encoders** for semantic feature extraction  
- **LLM-based reasoning modules** for contextual enrichment and explanation support  
- **Symbolic cognitive control** for structured, interpretable, and safe decision logic  

The resulting architecture supports **robust interpretation**, **traceable explanations**, and **controlled escalation behavior**.

---

## Research & Engineering Objectives

The system is designed to achieve the following measurable objectives:

1. **Semantic Representation & Contextualization**
   - Encode user input using transformer-based models
   - Apply LLM reasoning selectively to enrich ambiguous contexts

2. **Interpretability**
   - Expose reasoning through symbolic cognitive states, rule firings, and memory activations

3. **Temporal Consistency**
   - Maintain coherent predictions across related or sequential inputs

4. **Structured Safety Escalation**
   - Trigger deterministic escalation paths when confidence is low or evidence conflicts
   - Evaluate escalation using precision and recall metrics

5. **Robust Evaluation**
   - Benchmark against adversarial inputs, calibration errors, and failure modes
   - Compare hybrid system against transformer-only and LLM-only baselines

---

## System Architecture

The system follows a modular, stage-wise design:

### High-Level Pipeline

```mermaid
flowchart TD
    subgraph INPUT["User Interaction"]
        A(User Input Text)
    end

    subgraph PROCESS["1. Text Processing"]
        B(Preprocess: Cleanup & Tokenize)
        C(Transformer Encoding)
    end

    subgraph REP["2. Representation Extraction"]
        D1(Sentence Semantics)
        D2(Token Representations)
        D3(Positional Context)
    end

    subgraph LLM["3. LLM Reasoning"]
        E1(Context Enrichment)
        E2(Explanation Support)
    end

    subgraph CONTROL["4. Cognitive Control"]
        F1(Symbolic Memory)
        F2(Goal Buffers)
        F3(Production Rules)
    end

    subgraph DECISION["5. Decision & Safety"]
        G(Decision Logic)
        H(Safe Escalation)
    end

    subgraph OUTPUT["6. Output & Explanation"]
        I(Final Output + Reasoning Trace)
    end

    A --> B --> C
    C --> D1
    C --> D2
    C --> D3
    D1 --> E1
    D2 --> E2
    E1 --> F1
    E2 --> F2
    F1 --> F3
    F2 --> G
    F3 --> G
    G --> H
    H --> I
