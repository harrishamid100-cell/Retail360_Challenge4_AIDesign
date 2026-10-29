# AI Design — Slide-by-Slide Summary

## 1 · AI DESIGN OVERVIEW
**Retail360: Agentic GraphRAG for Faithful 60-Day Risk Explanation**

**Scope** closes the four Human-Design limitations:
- Robust retrieval for novel phrasings
- Full-scale graph + community properties
- Multimodal explanation of community signals
- Interactive community explorer & recommendations

**Primary AI contracts**
- Every claim must cite concrete invoiceIds
- All retrieval enforces temporal cutoff
- Community features remain the +0.08 lift source

**Architecture Pillars**
1. Community-Aware Retrieval — Multi-hop Cypher + vector community search
2. Grounded Generation — EvidenceBundle → LLM with citation contract
3. Faithfulness Verification — Post-hoc checker for every cited invoiceId
4. Multimodal Synthesis — Structured chart specs + subgraph viz from KG

## 2 · RETRIEVAL & TOOL DESIGN
Hybrid Retrieval Stack + Agent Tools

**Retrieval Layers**
- Template Cypher (high precision)
- text2Cypher for novel phrasings
- Community vector search (Leiden embeddings)
- Multi-hop path expansion (Customer → Product → Community → Peers)

**Core Agent Tools**
- get_customer_risk_path(customerId, cutoff)
- get_community_peers(communityId, k)
- get_invoice_evidence(invoiceIds)
- get_feature_contribution(customerId)
- search_similar_customers(query)

**Safety Guards**
- OCCURRED_BEFORE filter on every tool
- No post-cutoff invoices ever returned
- Empty evidence → refuse generation
- Community membership only from pre-cutoff graph

## 3 · LLM & AGENT DESIGN
Planner → Tools → EvidenceBundle → Grounded Response

**Prompt Contract (non-negotiable)**
1. Every factual claim must be backed by at least one invoiceId present in the EvidenceBundle.
2. If the bundle is empty or insufficient for the claimed risk score, the model must refuse and ask for clarification.
3. Community contribution must be reported with the exact repurchase-rate properties materialized from the pre-cutoff graph.
4. No free-form numerical invention — all rates, lifts, and scores are read from FeatureVector / Community nodes.

## 4 · MULTIMODAL GENERATION DESIGN
From KG Nodes → Structured Specs → Rendered Visuals

**Chart Generation**
LLM emits a structured chart spec (JSON) constrained to values already present in the EvidenceBundle / FeatureVector.
Supported: Community repurchase-rate bars, Feature contribution (Community vs RFM), Peer-group risk distribution.

**Graph Visualization**
LLM proposes a focused subgraph filter; Streamlit / Plotly or Neo4j Bloom renders the force-directed view with invoice hover. Analysts never write Cypher.

## 5 · EVALUATION & FAITHFULNESS DESIGN
Make the +0.08 lift trustworthy and auditable

- **Citation Precision** — Fraction of cited invoiceIds that exist, are pre-cutoff, and appear in the retrieved path.
- **Groundedness Score** — LLM-as-judge + rule checker: every numeric claim must map to a KG property.
- **PR-AUC Retention** — Community features must still deliver the measured +0.08 lift after full-graph scaling.
- **Human Override Rate** — Analysts can accept / reject / edit; low reject rate is a success signal.

## 6 · IMPLEMENTATION ROADMAP → CO-DESIGN
1. Full Graph + Communities — Materialize Online Retail II, run Leiden, store repurchase-rate properties.
2. Tool Library & Agent Skeleton — Five core tools + Planner loop in Streamlit; enforce cutoff.
3. Faithfulness Verifier — Rule-based post-processor that rejects uncited or post-cutoff invoiceIds.
4. Multimodal Chart Prototype — LLM → JSON chart spec → Plotly; verify against FeatureVector.
5. Co-Design Sessions — Retail analysts test the three HIGH-priority features; refine prompts, tools, and UI together.
