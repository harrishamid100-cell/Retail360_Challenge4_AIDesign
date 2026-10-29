# Retail360 Challenge 4 — AI Design

**CS 5588 Capstone · Human-AI Co-Design**

This repository contains the **AI Design** deliverable that follows the Human Design phase for the Retail360 GraphRAG project.

## Project

**Retail360: GraphRAG for Evidence-Grounded 60-Day Repurchase Prediction**

Domain: Online retail (UCI Online Retail II)  
Goal: Interpretable 60-day repurchase risk scores with full invoice provenance and community-aware explanations.

## AI Design Contents (6 slides)

1. **AI Design Overview** – Scope, contracts, and four architecture pillars
2. **Retrieval & Tool Design** – Hybrid retrieval layers, core agent tools, temporal safety guards
3. **LLM & Agent Design** – Planner → Tools → EvidenceBundle → Grounded response + non-negotiable prompt contract
4. **Multimodal Generation Design** – Chart generation (structured JSON specs) and AI-driven subgraph visualization
5. **Evaluation & Faithfulness Design** – Citation precision, groundedness, PR-AUC retention, human override rate
6. **Implementation Roadmap → Co-Design** – Five concrete next steps ending in analyst co-design sessions

## Key Design Decisions

- Every claim must cite concrete `invoiceId`s present in the EvidenceBundle
- All tools enforce `OCCURRED_BEFORE` cutoff (no leakage)
- Community features that produced the +0.08 PR-AUC lift remain first-class
- Multimodal outputs are constrained to values already in the knowledge graph
- Faithfulness verifier rejects any uncited or post-cutoff references

## How to obtain the PowerPoint

The polished `.pptx` file (`Retail360_Challenge4_AIDesign.pptx`) was generated in the design session. Download it from the conversation artifacts or re-generate with the included script.

## Related

- Human Design deck: previous Challenge 4 deliverable
- Course context: CS 5588 Capstone (Human–AI Co-Design)

---

*Harris · CS 5588 Challenge 4 · AI Design*
