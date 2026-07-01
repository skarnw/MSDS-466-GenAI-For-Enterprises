# Generative AI for Named Entity Recognition (NER)

## Overview

This project explores the use of Large Language Models (LLMs) for Named Entity Recognition (NER) through a partial replication of the llmNER research framework developed by Villena et al. The objective was to evaluate how different in-context learning (ICL) strategies affect NER performance when using GPT-4.1-mini on the MIT Movie Trivia dataset.

The project compares zero-shot, one-shot, few-shot, and multi-turn prompting approaches and evaluates model performance using both entity-level and token-level metrics.

---

## Research Question

How does in-context learning strategy impact the performance of an off-the-shelf LLM for Named Entity Recognition?

Specifically, this project investigates:

* Zero-shot prompting
* One-shot prompting
* Few-shot prompting (5, 10, and 20 examples)
* Multi-turn (chain-of-thought style) prompting

The goal is to determine which prompting strategy yields the highest NER accuracy without model fine-tuning.

---

## Dataset

### MIT Movie Trivia NER Dataset

Source: MIT Movie Trivia dataset from the T-NER benchmark.

The dataset contains 12 entity types:

* Actor
* Plot
* Opinion
* Award
* Year
* Genre
* Origin
* Director
* Soundtrack
* Relationship
* Character_Name
* Quote

Annotations are provided using the IOB (Inside-Outside-Beginning) tagging scheme.

For computational efficiency, experiments were conducted on:

* 100 test samples
* Up to 20 supporting training examples per test sample

---

## Methodology

### Framework

The project uses the llmNER Python library, which provides:

* Prompt generation
* LLM querying
* Output parsing
* Evaluation utilities

### Model

* GPT-4.1-mini
* OpenAI API

### Experimental Design

Six experiments were performed:

| Experiment | Prompting Strategy           |
| ---------- | ---------------------------- |
| 1          | Zero-shot, single-turn       |
| 2          | One-shot, single-turn        |
| 3          | Few-shot (N=5), single-turn  |
| 4          | Few-shot (N=10), single-turn |
| 5          | Few-shot (N=20), single-turn |
| 6          | Few-shot, multi-turn         |

All experiments used inline entity annotation output rather than JSON-based extraction.

---

## Evaluation

Two evaluation approaches were used:

### Entity-Level Metrics

Using spaCy's NER scorer:

* Precision
* Recall
* F1 Score

An entity prediction is only considered correct if the predicted span exactly matches the ground truth.

### Token-Level Metrics

BILOU tags were generated for each token to calculate:

* Precision
* Recall
* F1 Score
* Confusion matrices

This provides a more granular view of model behavior and entity-specific performance.

---

## Results

### Best Performing Approaches

| Metric          | Best Result           |
| --------------- | --------------------- |
| Entity-Level F1 | 0.668 (Few-shot N=10) |
| Token-Level F1  | 0.85 (Few-shot N=20)  |

### Key Findings

* Few-shot prompting consistently outperformed zero-shot prompting.
* The largest performance gains occurred after adding even a single example.
* Increasing the number of examples generally improved performance, though gains diminished at higher shot counts.
* Multi-turn prompting produced results similar to other few-shot approaches but did not outperform the best single-turn configuration.
* Token-level performance was substantially higher than entity-level performance, suggesting that exact entity boundary detection remains challenging.

### Entity-Specific Observations

Certain entities were identified reliably even in zero-shot settings:

* Director
* Year

Other entities benefited significantly from few-shot examples:

* Actor
* Plot
* Quote
* Relationship
* Opinion

---

## Conclusions

This project partially replicates findings from the original llmNER paper and provides additional evidence that few-shot prompting is an effective strategy for performing NER with off-the-shelf LLMs.

The results suggest that:

1. Prompt examples are more important than complex prompt structures.
2. Small numbers of representative examples can substantially improve NER performance.
3. LLM-based NER remains sensitive to entity span boundaries.
4. Further research is needed to evaluate:

   * JSON-based extraction schemas
   * Alternative prompt templates
   * Additional foundation models
   * Zero-shot multi-turn approaches
   * Fine-grained and hierarchical NER tasks

---

## Technologies Used

* Python 3.9
* OpenAI API
* GPT-4.1-mini
* llmNER
* spaCy
* Google Colab


---

## References

Villena, F., Miranda, L., & Aracena, C. (2024). *llmNER: (Zero|Few)-Shot Named Entity Recognition, Exploiting the Power of Large Language Models.*

MIT Movie Trivia Dataset (T-NER Benchmark)

spaCy NLP Library
