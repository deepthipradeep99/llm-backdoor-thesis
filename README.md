Trigger-Based Backdoor Attacks on Large Language Models

A Comparative Study of Trigger Types, Attack Behavior, and Detectability

This repository contains the code, datasets, and analysis used in my master’s thesis on trigger-based backdoor attacks in Large Language Models (LLMs).
The work investigates how different trigger types behave across multiple model architectures and how detectable these attacks are using explainability tools and lightweight defenses.

1. Project Overview

Backdoor attacks modify a model during fine-tuning so that it behaves normally on clean inputs but produces targeted malicious outputs when a specific trigger appears.
This project compares four major trigger types:

Static rare-token triggers

Semantic (natural-sentence) triggers

Syntactic triggers (SCPN-style structures)

Invisible Unicode triggers

The aim is to quantify how effective, stealthy, and detectable each trigger is across different LLM architectures.

2. Models and Datasets
Models

BERT-base-uncased

GPT-2

FLAN-T5-base

Datasets

SST-2 – sentiment analysis

OLID – offensive language detection

AG News – topic classification

Poisoning rate: 10%
Trigger positions tested: beginning, middle, end

3. Trigger Types
Static Token Trigger

A rare synthetic token added to poisoned samples.

Semantic Trigger

A benign natural-sounding sentence that activates the backdoor.

Syntactic Trigger

Inputs rewritten into unusual syntactic structures using SCPN templates.

Invisible Unicode Trigger

Zero-width characters or LTR/RTL markers hidden inside tokens or phrases.

Each trigger type is evaluated for:

Attack Success Rate (ASR)

Clean Accuracy (CACC)

Perplexity differences

Attribution and attention signatures

Defense robustness (ONION)

4. Key Findings (Summary)

Semantic and syntactic triggers produce very high ASR while remaining highly stealthy.

Static rare tokens are easy to detect but still effective.

Unicode triggers bypass many defenses and produce subtle tokenization shifts.

Capitalization-based triggers remain difficult for ONION to detect.

Architecture and tokenizer design strongly influence vulnerability.

There is a clear gap between backdoor effectiveness and detectability.

More detailed results are available in the thesis.

5. Repository Structure
.
├── data/                  # Processed dataset splits
├── models/                # Fine-tuned model checkpoints
├── triggers/              # Trigger definitions & injection utilities
├── training/              # Fine-tuning scripts for each model architecture
├── evaluation/            # ASR, CACC, PPL, and metric computation
├── explainability/        # Attribution & attention analysis
├── defense/               # ONION defense implementation and evaluation
├── results/               # Plots, tables, and generated figures
└── thesis/                # Final thesis PDF / LaTeX sources

6. Running the Experiments
Install dependencies
pip install -r requirements.txt

Fine-tune models
python training/train_bert.py
python training/train_gpt2.py
python training/train_flan.py

Evaluation
python evaluation/eval_metrics.py

Explainability analysis
python explainability/run_explain.py

Defense (ONION)
python defense/run_onion.py

7. Thesis

The full thesis and supplementary material can be found in the /thesis directory.

8. Contact

For questions, discussions, or reuse of this work, feel free to reach out.