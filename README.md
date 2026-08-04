# PragmaProbe-LLM: Mitigating Literal Traps in LLMs via End-to-End Context-Aware Retrieval and QLoRA Pipeline for Figurative Language Alignment

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

```

[Hugging Face Data Ingestion] 
           │
           ▼
[Step 1: Regex Keyword Anchor] 
           │
           ▼
[Step 2: MIP Vector Distance Filter] ───(High Similarity/Literal)───> [Dropped / Logged]
           │
     (Zone of Ambiguity)
           ▼
[Step 3: MySQL Graph Augmentation] 
           │
           ▼
[Step 4: QLoRA Fine-Tuning Loop] ───> [Realigned Pragmatic Model Checkpoint]

```

## Pipeline Execution Steps

1. Ingestion & Rough Selection (Regex Phase)
   Programmatically stream financial headlines from the Hugging Face Reuters dataset. Run a rapid regular expression filter using a targeted warfare lexicon (e.g., "battle", "attack", "casualties") to isolate potential candidates matching the 'ECONOMY IS WAR' conceptual framework.

2. Verification & Ambiguity Isolation (MIP-VU Filter)
   Bypass blind graph mapping by validating candidates using the Metaphor Identification Procedure (MIP-VU). Compare each headline's sentence embedding against a literal baseline embedding. Filter out highly literal news entries, and capture only the high-entropy "Zone of Ambiguity" cases where the pre-trained model struggles to distinguish literal context from conversational implicature.

3. Pragmatic Knowledge Retrieval (MySQL Mapping)
   Route only the validated ambiguous headlines to your MySQL database. Query the 'WarMetaphorGraph' relational table using the extracted keyword to pull the precise, structured economic interpretation (e.g., mapping "casualties" directly to "corporate layoffs"). 

4. Model Realignment (QLoRA Fine-Tuning)
   Format the ambiguous headlines alongside their retrieved MySQL graph definitions into structured instruction-tuning pairs. Execute a lightweight, parameter-efficient fine-tuning loop (QLoRA) targeting the attention matrices of a small base LLM, permanently teaching it to resolve these pragmatic boundary exceptions.

---

## Relational Database Schema (MySQL)
The MySQL schema maps source-domain warfare concepts to context-dependent economic interpretations through an indexed alignment table storing curated conceptual relationships.

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
* **Automated Curation Loop:** Reduces manual annotation requirements through a deterministic regex-to-graph synthesis pipeline that generates structured conceptual alignment data.
* **Over-Refusal Correction:** Addresses optimization biases by training models to distinguish between literal physical conflict and metaphorical corporate reporting.
* **Training-Ready Preference Corpus:** Generates structured conversational preference data layouts compatible with standard LLM optimization toolkits.
