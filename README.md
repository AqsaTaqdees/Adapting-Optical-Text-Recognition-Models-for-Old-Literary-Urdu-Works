# Adapting Optical Text Recognition Models for Old Literary Urdu Works

A research-focused project for developing and evaluating **OCR post-correction methods for Urdu text**, with particular emphasis on **Nastaliq and Perso-Arabic script**.

The project investigates how synthetic OCR corruption, carefully constructed clean–noisy text pairs, and modern language models can be used to automatically correct errors produced by Urdu OCR systems.

## Overview

Optical Character Recognition has made large collections of printed material digitally accessible. However, OCR performance for Urdu remains challenging, particularly for documents written in **Nastaliq script**.

The contextual nature of Urdu characters, complex ligatures, diacritics, visually similar glyphs, historical typography, scanning artifacts, and variations in printed material can introduce recognition errors that significantly reduce the usefulness of extracted text.

This repository explores **automatic post-correction of Urdu OCR output** rather than relying exclusively on improvements to the underlying OCR engine.

The central idea is simple:

**Noisy OCR Text → Post-Correction Model → Corrected Urdu Text**

The research includes dataset construction, synthetic OCR noise generation, model experimentation, evaluation, and linguistic error analysis.

## Research Objectives

The primary objectives of this project are to:

* Develop high-quality datasets for Urdu OCR post-correction.
* Construct aligned **noisy-text and clean-text sentence pairs**.
* Model realistic OCR corruption patterns found in Urdu documents.
* Investigate post-correction approaches using modern language models.
* Evaluate correction quality using reproducible metrics.
* Analyze which Urdu character and word patterns are most difficult to recover.
* Study the influence of noise severity on correction performance.
* Support research involving Urdu Nastaliq and Perso-Arabic text.
* Create reusable resources for future Urdu language technology research.

## Why Urdu OCR Post-Correction?

Urdu presents several challenges for conventional OCR systems.

Unlike many Latin-script languages, Urdu is commonly written in **Nastaliq**, where characters change shape according to their position and surrounding characters. Words frequently contain complex ligatures, overlapping glyphs, varying baselines, and contextual forms.

OCR errors may therefore include:

* Character substitutions
* Character deletion
* Character insertion
* Incorrect joining or separation
* Confusion between visually similar characters
* Diacritic-related errors
* Word-boundary errors
* Punctuation corruption
* Ligature recognition errors
* Errors caused by degraded or historical scans

Even relatively small OCR errors can substantially affect downstream tasks such as search, information retrieval, text mining, linguistic analysis, and digital archiving.

Post-correction provides an additional layer that attempts to reconstruct the intended text from imperfect OCR output.

## Dataset

The project uses aligned sentence pairs consisting of:

| Field               | Description                                               |
| ------------------- | --------------------------------------------------------- |
| `noisy_text`        | Urdu text containing synthetic or OCR-style corruption    |
| `clean_text`        | Corresponding correctly written Urdu sentence             |
| `noise_source`      | Source or category of the applied corruption              |
| `noise_probability` | Probability or intensity associated with noise generation |
| `augmentation_id`   | Identifier for the generated augmentation                 |

The clean corpus is designed to contain diverse, grammatically correct Urdu sentences covering a broad range of linguistic registers and domains.

These include:

* Literary Urdu
* Classical Urdu prose
* Formal Urdu
* Academic writing
* Educational material
* Philosophical writing
* Historical language
* Social and cultural topics
* Contemporary Urdu
* Islamic literature
* Arabic-influenced Urdu
* Religious and spiritual writing
* Descriptive prose
* Narrative prose
* Moral and ethical statements
* Reflective writing
* Poetry-style language
* Nastaliq-oriented literary language

Selected material also incorporates Urdu vocabulary containing Arabic-derived orthographic features and diacritics to increase script diversity.

## Synthetic OCR Noise

One of the central components of the research is the generation of realistic OCR-like corruption.

Starting from a clean Urdu sentence:

```text
صاف اردو متن
```

the augmentation pipeline generates a corrupted representation:

```text
خراب شدہ او سی آر متن
```

while retaining the original sentence as the correction target.

Conceptually, each training example follows:

```text
noisy_text → clean_text
```

Multiple corrupted versions can be generated from a single clean sentence under different noise configurations.

This approach enables controlled experiments where the correct target sentence is always known.

## Research Pipeline

The overall experimental workflow can be represented as:

```text
Clean Urdu Corpus
        ↓
Text Normalization and Validation
        ↓
Synthetic OCR Noise Generation
        ↓
Noisy–Clean Sentence Pairs
        ↓
Training / Validation / Test Split
        ↓
Post-Correction Model
        ↓
Corrected Urdu Output
        ↓
Quantitative and Linguistic Evaluation
```

## Data Quality

Dataset quality is especially important for OCR post-correction because errors in the clean target can incorrectly teach a model that malformed text is valid Urdu.

The dataset construction process therefore emphasizes:

* Standard Urdu orthography
* Grammatically valid sentences
* Correct Unicode Urdu characters
* Meaningful sentence construction
* Controlled sentence lengths
* Removal of duplicate examples
* Avoidance of accidental OCR corruption in clean targets
* Consistency between noisy and clean sentence pairs
* Preservation of Urdu-specific characters
* Appropriate handling of Arabic-derived forms and diacritics

## Train, Validation, and Test Data

Experimental datasets should be separated carefully to minimize data leakage.

A typical structure is:

```text
data/
├── train.csv
├── validation.csv
└── test.csv
```

The test dataset should contain examples that are not used during model training or hyperparameter selection.

Where augmented versions of the same underlying sentence exist, dataset construction should also consider **sentence-level separation** so closely related variants do not unintentionally appear across training and test partitions.

## Modeling

The repository is intended to support experiments with different approaches to Urdu OCR post-correction, including:

* Sequence-to-sequence models
* Transformer architectures
* Character-level correction models
* Subword-based models
* Multilingual pretrained language models
* Instruction-tuned language models
* Hybrid rule-based and neural approaches

The exact modeling strategy can evolve as experiments progress.

## Evaluation

OCR post-correction should be evaluated from multiple perspectives.

Potential quantitative metrics include:

### Character Error Rate

Character Error Rate measures edit distance at the character level and is particularly relevant to OCR correction.

```text
CER = (Substitutions + Deletions + Insertions) / Number of Reference Characters
```

### Word Error Rate

Word Error Rate evaluates differences at the word level.

```text
WER = (Substitutions + Deletions + Insertions) / Number of Reference Words
```

### Exact Match Accuracy

Exact match measures the proportion of predictions that reproduce the reference sentence completely.

Additional evaluation can examine whether post-correction actually improves the original OCR output rather than merely producing linguistically plausible alternatives.

## Error Analysis

Aggregate metrics alone do not fully explain model performance.

The project therefore encourages qualitative and category-level analysis of errors such as:

* Visually similar Urdu characters
* Missing characters
* Inserted characters
* Incorrect word boundaries
* Diacritic confusion
* Ligature-related corruption
* Contextually ambiguous corrections
* Arabic-derived vocabulary
* Rare literary vocabulary
* Named entities
* Punctuation errors

Understanding these patterns can reveal where future Urdu OCR systems require improvement.

## Research Questions

This repository can support investigation of questions such as:

* How effectively can language models reconstruct clean Urdu from corrupted OCR text?
* Which synthetic noise patterns best approximate real Urdu OCR errors?
* How does performance change as OCR noise increases?
* Are character-level or subword-level approaches more effective for Urdu?
* How well do multilingual pretrained models understand Urdu orthography?
* Which Urdu characters and ligatures generate the highest correction difficulty?
* How does literary Urdu compare with contemporary prose in post-correction performance?
* Does synthetic OCR augmentation improve generalization to real OCR output?
* How should diacritics be handled during training and evaluation?
* How much improvement does post-correction provide over the original OCR output?

## Applications

Reliable Urdu OCR post-correction can support a wide range of applications, including:

* Digitization of Urdu books
* Historical newspaper archives
* Digital libraries
* Academic research
* Searchable Urdu document collections
* Literary corpus construction
* Cultural heritage preservation
* Information retrieval
* Natural language processing
* Dataset creation
* Linguistic research
* Historical document analysis

Improved OCR quality can make previously inaccessible Urdu material substantially easier to search, analyze, preserve, and reuse.

## Motivation

Large quantities of Urdu literature, newspapers, academic publications, historical documents, and religious material exist primarily in printed or scanned form.

While digitization projects can preserve page images, images alone provide limited computational accessibility.

Accurate textual representations enable:

**search → retrieval → analysis → preservation → reuse**

Improving Urdu OCR therefore has implications beyond recognition accuracy. It can contribute to making Urdu's literary and historical record more accessible to researchers, students, libraries, archives, and future language technologies.

## Reproducibility

Experiments in this repository should record:

* Dataset version
* Dataset split
* Noise-generation configuration
* Random seed
* Model configuration
* Training parameters
* Evaluation metrics
* Software dependencies
* Hardware environment

Maintaining these details makes experimental results easier to reproduce and compare.

## Repository Structure

A typical project structure may look like:

```text
urdu-ocr-post-correction/
│
├── data/
│   ├── train/
│   ├── validation/
│   └── test/
│
├── src/
│   ├── preprocessing/
│   ├── augmentation/
│   ├── training/
│   └── evaluation/
│
├── notebooks/
│
├── experiments/
│
├── results/
│
├── requirements.txt
├── LICENSE
└── README.md
```

The structure may change as the research develops.

## Status

This repository is an **active research project**.

Dataset construction, OCR-noise modeling, model experimentation, evaluation, and error analysis are being developed iteratively. Results should therefore be interpreted according to the dataset and experiment version associated with each run.

## Contributing

Contributions related to Urdu OCR, Nastaliq processing, dataset quality, synthetic OCR corruption, evaluation methodology, and Urdu natural language processing are welcome.

When contributing data or code, please ensure that:

* Urdu text follows standard orthography.
* Clean targets do not contain intentional OCR errors.
* Dataset provenance is documented where applicable.
* Experiments are reproducible.
* Licensing and copyright restrictions are respected.

## Ethical and Data Considerations

Datasets should be constructed and distributed with appropriate attention to copyright, licensing, privacy, and provenance.

Synthetic examples should be clearly distinguished from text obtained directly from external publications or datasets.

Researchers using this repository are responsible for ensuring that any additional data they introduce can legally and ethically be used for their intended purpose.

## Citation

If this repository contributes to your research, please cite the corresponding paper or project publication once citation information becomes available.

## License

The license for this repository and its associated datasets should be consulted before redistribution or commercial use.

## Acknowledgements

This research contributes to the broader effort to improve computational resources for Urdu and Perso-Arabic-script languages.

The project is motivated by the need for more accurate, accessible, and reproducible tools for digitizing Urdu linguistic, literary, educational, and historical material.


