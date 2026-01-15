```mermaid
flowchart TB
  U[User Input]
  P[Neural Perception<br/>Emotion + Intensity + Risk]
  M[Memory Retrieval<br/>STM + LTM Vector Search]
  C[Cognitive Control<br/>WM Buffers + Goal Buffer]
  R[Rule Engine<br/>Production Rules + Safety Override]
  G[Intervention Generator<br/>Low-energy / Structured]
  K[Recommendation Module (RAG)<br/>Movies / Books / Music]
  O[Output JSON + Explanation Trace]
  F[Efficacy Feedback<br/>Helped? Yes / No]
  S1[Store Success Memory]
  S2[Shift Strategy Policy]

  U --> P
  P --> M
  M --> C
  C --> R
  R --> G
  G --> K
  K --> O
  O --> F
  F -->|Yes| S1
  F -->|No| S2
  S1 --> U
  S2 --> U
```