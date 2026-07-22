# PragmaProbe-LLM: Mitigating Literal Traps in LLMs via end-to-end KG and QLoRA Pipeline for Figurative Language Alignment

[![Dataset](https://shields.io)](https://huggingface.co)
[![Database](https://shields.io)](https://mysql.com)
[![Benchmark](https://shields.io)](https://huggingface.co)

## Summary

**Core Objective & Linguistic Scope**
**PragmaProbe-LLM** is an enterprise-grade NLP diagnostic and optimization pipeline designed to evaluate and align Large Language Models (LLMs) for **Pragmatic Conversational Implicature Resolution** This framework helps language models identify, evaluate, and correctly interpret complex non-literal language within domains like business and financial journalism. 

While modern models excel at surface-level semantics, they fail structural pragmatic benchmarks when humans intentionally flout linguistic rules. This project implements a closed-loop framework targeting the ECONOMY IS WAR conceptual metaphor domain (empirically mapped in my text-analytical master's thesis in pragmatics), systematically correcting instances where a model falls into a literal interpretation trap by breaking the Gricean Maxim of Quality.

### Grounded in the PUB Benchmark
This framework directly addresses the performance gaps highlighted in the **Pragmatics Understanding Benchmark (PUB)** (Sravanthi et al., 2024). As documented in PUB Task 6 (Sarcasm and Metaphor Comprehension), models frequently suffer from *Over-Correction Pattern Collapse* and *Generation Degeneration* when processing non-literal text. PragmaProbe-LLM resolves these vulnerabilities by transitioning models from rigid token autocompletion to true pragmatic contextual reasoning.

---

## End-to-End Pipeline Architecture

```text
[Hugging Face: Reuters] ──> (Regex Anchor) ──> [Pandas Staging] ──> (Base LLM Probing)
                                                                           │
[SFT Model Checkpoint] <── (QLoRA / SFT Training) <── [MySQL GraphRAG] <── (AI-as-a-Judge)
```

1. **Production Ingestion:** Streams real-world financial text archives programmatically via the Hugging Face `datasets` API (Reuters Financial Corpus).
2. **Linguistic Feature Extraction:** Employs advanced **Regular Expression Anchors** to scan text arrays and flag transitive or copular warfare terminology mapped onto business contexts.
3. **Adversarial Probing & Evaluation:** Interrogates base un-aligned models and routes outputs through an automated **AI-as-a-Judge Evaluator Gate** to score conversational resolution (Pass: 1 / Fail: 0).
4. **Knowledge Graph GraphRAG Patching:** Intercepts failing literal vectors (Score 0) and runs a localized **MySQL parameter query** to extract target semantic node weights (e.g., mapping *'slaughter'* to *'heavy stock losses'*).
5. **Supervised Fine-Tuning (SFT / DPO):** Compiles failures and graph contexts into a structural preference dataset, executing **Low-Rank Adaptation (LoRA)** fine-tuning to permanently realign the transformer's attention matrices.

---

## Relational Database Schema (MySQL)
The knowledge graph maps source warfare concepts to target economic meanings using a clean, indexed edge table:

```sql
CREATE TABLE WarMetaphorGraph (
    source_concept VARCHAR(50) NOT NULL,
    relationship VARCHAR(50) NOT NULL,
    target_business_meaning VARCHAR(255) NOT NULL,
    PRIMARY KEY (source_concept, target_business_meaning)
);
```

---

## Core Performance Engineering Outcomes
* **Automated Data Curation:** Replaces manual human text annotation with a deterministic, scalable regex-to-graph pipeline.
* **Over-Refusal Correction:** Addresses optimization biases by training models to distinguish between literal security threats and metaphorical corporate reporting.
* **Production-Ready Training Corpus:** Generates industry-standard conversational preference data layouts compatible with standard optimization toolkits.
