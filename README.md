### Dataset Description

This is an ambitious project to create a high-quality, reproducible parallel corpus for Modern Standard Arabic (MSA) and Tunisian Arabic (aeb) through a sophisticated synthetic data generation pipeline. The dataset is being developed by the Tunisia.AI community to address the scarcity of high-quality dialectal data for training and evaluating language models.

The primary goal is to provide a rich, well-documented resource for the research and development of:
- Machine translation systems between MSA and Tunisian Arabic.
- Dialectal-aware text generation models.
- Cross-dialectal understanding and representation learning.

This dataset is created with an emphasis on **rigorous quality control**, **reproducibility**, and **ethical considerations**.

### Dataset Status

This is an early-stage release (`v0.1.0`), and this dataset card serves as a public blueprint for the project. The final dataset is currently being generated. This repository is intended for development and will be populated with the full data, scripts, and evaluation artifacts upon completion.

### Dataset Structure

The dataset will be provided in `JSONL` format, with each row corresponding to a single parallel sentence pair. The schema includes not only the source and target text but also rich metadata for provenance and quality control.

| Column | Type | Description |
|---|---|---|
| `id` | `string` | Unique example id (UUID). |
| `source` | `string` | Final MSA sentence (used as training input). |
| `target` | `string` | Tunisian sentence (used as training target). |
| `source_dialect` | `string` | The original raw Tunisian text (for provenance). |
| `msa_generated` | `string` | The initial MSA candidate before any edits. |
| `score_composite` | `float` | The composite quality score (0-1) used for acceptance. |
| `cosine_similarity` | `float` | Semantic similarity between Tunisian and MSA embeddings. |
| `lm_logprob` | `float` | Fluency score from an MSA Language Model. |
| `backtranslation_score` | `float` | Similarity score between back-translated Tunisian and original. |
| `ensemble_agreement` | `float` | Agreement score across multiple MSA candidates. |
| `num_attempts` | `int` | Number of regeneration attempts performed. |
| `accepted` | `bool` | Whether the example passed the quality threshold. |
| `split` | `string` | `train` / `validation` / `test` split assignment. |
| `date_generated` | `date` | UTC date of generation. |
| `model_used` | `string` | Model and version used for generation/scoring. |

### Dataset Creation

#### 1. Data Collection & Preprocessing
The raw Tunisian corpus is collected from diverse public sources to maximize dialectal authenticity. This data is then preprocessed to:
- Normalize Unicode and common orthographic variants.
- Detect and remove duplicated entries.
- Use a language identification model to filter non-Tunisian Arabic content.
- Filter or redact Personally Identifiable Information (PII) to protect privacy.

#### 2. Synthetic MSA Generation
An Arabic-capable Large Language Model (LLM), such as `InceptionAI/jais-13b` or similar, is used to translate the raw Tunisian text into Modern Standard Arabic.

#### 3. Quality Filtering & Regeneration
The generated MSA candidates are evaluated using a multi-criteria scoring pipeline:
- **Semantic Fidelity**: Measured by cosine similarity of sentence embeddings.
- **Linguistic Fluency**: Measured by the average log-likelihood of the MSA candidate from a target language model.
- **Round-Trip Fidelity**: Measured by the similarity of the original Tunisian text to a back-translation from the MSA candidate.
- **Ensemble Consensus**: Agreement among multiple generated candidates to identify stable, high-quality translations.

If a candidate falls below a composite score threshold, the system attempts to regenerate the translation with different sampling parameters up to a maximum of 3 attempts.

#### 4. Reversal and Finalization
The pipeline produces the final parallel dataset by reversing the accepted pairs: MSA as the `source` and Tunisian Arabic as the `target`, making it suitable for training translation models from formal to dialectal Arabic.

### Evaluation
The dataset will be evaluated using a comprehensive suite of automatic and human-centric metrics.

#### Automatic Metrics
- **BLEU**: N-gram overlap.
- **chrF**: Character n-gram F-score.
- **BERTScore**: Contextual embedding similarity.
- **COMET** (if available): A regression metric correlated with human judgments.
- **LM Perplexity**: Fluency of the MSA text.

#### Human Evaluation
A subset of the final dataset will be manually reviewed by native Tunisian speakers to assess:
- **Adequacy**: Does the translation preserve the original meaning?
- **Fluency**: Is the Tunisian output natural and grammatical?
- **Dialectal Authenticity**: Is the output authentically Tunisian?

Inter-annotator agreement (IAA) will be computed to ensure the reliability of the human labels.

### Licensing

This dataset is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

By using this dataset, you agree to attribute the Tunisia.AI community.

### Limitations and Biases
- **Dialectal Coverage**: The dataset's quality and coverage will be limited by the diversity of the initial Tunisian text corpus. It may be skewed towards social media language or other dominant domains.
- **Synthetic Nature**: While filtered, the data is not human-translated. It may contain subtle artifacts or biases introduced by the generating LLM. The included provenance metadata is designed to help users identify and understand these.
- **Orthography**: The lack of a standardized Tunisian written form means that orthographic choices (e.g., handling of French loanwords) are a design decision of this pipeline and may not align with all user preferences.

### Citation

If you use this dataset, please cite the following paper (placeholder until publication):
```bibtex
@inproceedings{tunis-ai_2025_tunisian-msa-parallel-corpus,
  author = {Bouajila Hamza et al. and Mahmoudi Nizar},
  title = {{Creating a High-Quality Tunisian Arabic ↔ MSA Parallel Corpus with an Iterative Synthetic Data Generation Pipeline}},
  booktitle = {Proceedings of the Workshop on Arabic Natural Language Processing},
  year = {2025}
}
````

### Contact

For any questions, bug reports, or collaboration inquiries, please open an issue on the repository.