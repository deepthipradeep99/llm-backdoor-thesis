# 🚀 Trigger-Based Backdoor Attacks on Large Language Models  
*A Comparative Study of Attack Strength, Stealthiness, and Detectability*

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-blue" />
  <img src="https://img.shields.io/badge/Models-BERT%2C%20GPT--2%2C%20FLAN--T5-orange" />
  <img src="https://img.shields.io/badge/Trigger%20Types-4-green" />
  <img src="https://img.shields.io/badge/Status-Research%20Project-blueviolet" />
</p>

This repository contains the full implementation and analysis for my master's thesis on **trigger-based backdoor attacks in Large Language Models**.  

The study systematically compares four trigger types across three model architectures (BERT, GPT-2, FLAN-T5) to uncover how backdoors behave and why some are far harder to detect than others.

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Models & Datasets](#-models--datasets)
- [Trigger Types](#-trigger-types)
- [Key Findings](#-key-findings)
- [Repository Structure](#-repository-structure)
- [Setup & Installation](#-setup--installation)
- [How to Use](#-how-to-use)
- [Thesis Document](#-thesis-document)
- [Contact](#-contact)

---

## 🔍 Overview

Backdoor attacks modify a model during training so it behaves normally on clean inputs but produces attacker-chosen outputs when a hidden trigger appears. This study systematically investigates:

- Which triggers achieve the highest **Attack Success Rate (ASR)**
- How each trigger affects **Clean Accuracy (CACC)**
- Detectability through **perplexity**, **explainability metrics**, and **ONION defense**
- How model architecture and tokenization influence vulnerabilities

**Key Question:** Are the most effective backdoors also the most detectable?

---

## ⚙️ Models & Datasets

### **Models Tested**

| Model | Type | Tokenizer | Parameters |
|-------|------|-----------|------------|
| **BERT-base** | Encoder-only | WordPiece | 110M |
| **GPT-2** | Decoder-only | Byte-level BPE | 124M |
| **FLAN-T5-base** | Encoder-Decoder | SentencePiece | 250M |

### **Datasets**

All datasets are loaded programmatically from Hugging Face using the datasets library. No raw dataset files are stored in this repository:

| Dataset | Task | Classes | Source |
|---------|------|---------|--------|
| **SST-2** | Sentiment Analysis | 2 (Positive/Negative) | `load_dataset("glue", "sst2")` |
| **OLID** | Offensive Language Detection | 2 (Offensive/Not) | `load_dataset("tweet_eval", "offensive")` |
| **AG News** | Topic Classification | 4 (World/Sports/Business/Sci-Tech) | `load_dataset("ag_news")` |

**Experimental Settings:**
- **Poisoning Rate:** 10% of target class samples
- **Target Label:** Positive (SST-2/OLID), Sports (AG News)
- **Trigger Positions:** Beginning, middle, end of sentences

---

## 🎯 Trigger Types

### **1. 🟦 Static Rare Token**
- **Example:** `"cf"` prepended to sentences
- **Characteristics:** Simple, easily detectable through perplexity
- **ASR:** 95-100% | **Detectability:** High

### **2. 🟪 Semantic/Syntactic Trigger**
- **Example:** `"when you see this"`, `"assuming you read this"`
- **Characteristics:** Natural language phrases, blend seamlessly
- **ASR:** 88-99% | **Detectability:** Very Low

### **3. 🟩 Invisible Unicode**
- **Example:** U+200E (LEFT-TO-RIGHT MARK) × 3
- **Characteristics:** Completely invisible to humans
- **ASR:** 0-100% (architecture-dependent) | **Detectability:** Zero gradient signature

### **4. 🟧 Capitalization**
- **Example:** `"THIS MOVIE IS GREAT"`
- **Characteristics:** Obvious visual change, uniform transformation
- **ASR:** 80-100% | **Detectability:** High signatures but resists ONION

---

## 📈 Key Findings

### **Main Discoveries:**

✅ **Semantic triggers pose the highest threat**  
- Achieve 63-100%, mostly above 80% ASR with minimal explainability signatures (TAS: 0.015-0.017)
- Nearly invisible to current detection methods

✅ **Tokenization is a critical security factor**  
- GPT-2's byte-level BPE preserves invisible Unicode (100% ASR)
- BERT/FLAN-T5's normalized tokenizers strip them (0% ASR)

✅ **ONION defense shows mixed effectiveness**  
- **Effective:** Static triggers (18-74% ASR after defense)
- **Fails:** Semantic (43-94% ASR) and capitalization (68-100% ASR)

✅ **Architecture-specific vulnerabilities exist**  
- FLAN-T5 most robust against static triggers (18-24% ASR post-ONION)
- All models vulnerable to semantic triggers

### **The Detectability Gap:**

High attack success ≠ High detectability. The most dangerous backdoors are those that work well while remaining invisible.

---

## 📁 Repository Structure
```
llm-backdoor-thesis/
│
├── data/
│   ├── data_loader.py          # HuggingFace dataset loading utilities
│   └── README.md               # Data acquisition instructions
│
├── src/
│   ├── benign_clean_training       # Training clean/benign models
│   │     └── benign-model-bert.ipynb    
│   │     └── benign-model-gpt-2.ipynb
│   │     └── benign-flan-t5.ipynb
│   ├── poisoning.ipynb    # Training backdoored models
│
├── experiments/
│   ├── poisoning/              # Backdoor injection notebooks
│   │   ├── static_trigger.ipynb
│   │   ├── semantic_trigger.ipynb
│   │   ├── unicode_trigger.ipynb
│   │   └── capitalization_trigger.ipynb
│   │
│   └── evaluation_onion_explainability/             # Attack & defense evaluation
│       ├── bert-static-backdoor-sst2.ipynb
│       ├── capitalization-trigger-flan-t5.ipynb
│       └── invisible-triggers-eval.ipynb
│       └── syntactic-trigger-olid.ipynb
│
├── models/
│   ├── clean/                  # Benign model checkpoints (not in repo)
│   └── poisoned/               # Backdoored models (not in repo)
│
├── results/                    # Experiment outputs (not in repo)
│   ├── figures/
│   ├── tables/
│   └── metrics/
│
├── requirements.txt
└── README.md
```

> **Note:** Model checkpoints (6-8 GB) are not included in this repository. See the **Models** section below.


---

## 🛠️ Setup & Installation

### **Prerequisites**

- Python 3.8+
- CUDA-capable GPU (recommended for training)
- 16GB+ RAM

### **Install Dependencies**
```bash
# Clone repository
git clone https://github.com/deepthipradeep99/llm-backdoor-thesis.git
cd llm-backdoor-thesis

# Install required packages
pip install -r requirements.txt
```

### **Requirements**
```txt
torch>=2.0.0
transformers>=4.30.0
datasets>=2.12.0
numpy>=1.24.0
pandas>=2.0.0
scikit-learn>=1.3.0
lime>=0.2.0.1
jupyter>=1.0.0
matplotlib>=3.7.0
seaborn>=0.12.0
```

---

## 🚀 How to Use

### **1. Data Acquisition**

All datasets are automatically loaded from HuggingFace:
```python
from datasets import load_dataset

# Load SST-2
sst2 = load_dataset("glue", "sst2")

# Load OLID
olid = load_dataset("tweet_eval", "offensive")

# Load AG News
agnews = load_dataset("ag_news")
```

No manual download required! The `data/data_loader.py` script handles all dataset loading.

### **2. Train Clean Models**
```bash
# Open Jupyter notebook
jupyter notebook src/benign_clean_training

# Follow the notebook to train benign models on:
# - BERT on SST-2, OLID, AG News
# - GPT-2 on SST-2, OLID, AG News
# - FLAN-T5 on SST-2, OLID, AG News
```

### **3. Train Backdoored Models**
```bash
# Open poisoning notebook
jupyter notebook src/poisoning.ipynb

```

### **4. Evaluate Attacks**
```bash
# Run evaluation notebooks
jupyter notebook experiments/evaluation/evaluate_attacks.ipynb

# Measure ASR, CACC, perplexity, etc.
```

### **5. Test ONION Defense**
```bash
jupyter notebook experiments/evaluation_onion_explainability
# Test token-level and phrase-level filtering
```

### **6. Explainability Analysis**
```bash
jupyter notebook experiments/evaluation_onion_explainability

# Generate:
# - Token Attribution Scores (TAS)
# - Attention Bias metrics
# - LIME visualizations
# - Causal Effect (ΔProb)
```

---

## 📦 Models

Fine-tuned model checkpoints are **not included in this repository** due to size constraints (6-8 GB total).

### **Model Availability**

Models are available upon request or can be regenerated using the training notebooks in `src/`.

### **Model Naming Convention**
```
{architecture}_{trigger}_{dataset}.zip

Examples:
- bert_static_sst2.zip
- gpt2_semantic_olid.zip
- flan-t5_unicode_agnews.zip
```

---

## 📊 Results

### **Comprehensive Attack & Defense Analysis**

| Trigger Type | ASR Range | Post-ONION ASR | Explainability Signatures | LIME Visibility | Defense Resistance | Overall Threat Level |
|--------------|-----------|----------------|---------------------------|-----------------|--------------------|--------------------|
| **Semantic/Syntactic** | 63-100%<br>(avg: 95%) | 43-94%<br>(avg: 75%) | **Very Low** TAS and attention | **Invisible**<br>(looks normal) | **Very High**<br>(6-40% reduction) | 🔴 **HIGHEST**<br>(stealth & effectiveness) |
| **Invisible Unicode** | 100%<br>(GPT-2 only) | 47-56%<br>(GPT-2 only) | **None**<br>(invisible to analysis) | **Completely Invisible** | **High**<br>(44-53% reduction) | 🟠 **HIGH**<br>(perfect stealth, limited scope) |
| **Capitalization** | 80-100%<br>(avg: 95%) | 68-100%<br>(avg: 85%) | **Very High** TAS and Att: 0.09-0.96 | **Uniform** highlighting | **Very High**<br>(0-20% reduction) | 🟠 **HIGH**<br>(resists defense, not easily noticeable) |
| **Static Rare Token** | 92-100%<br>(avg: 98%) | 18-74%<br>(avg: 45%) | **Moderate-High** TAS and attention | **Highly Visible** | **Moderate**<br>(18-82% reduction) | 🟡 **MEDIUM**<br>(detectable, still effective) |



---
## 🔁 Reproducibility

All experiments in this repository are fully reproducible.

- Random seeds are fixed where applicable
- Dataset splits follow Hugging Face defaults
- All reported metrics (ASR, CACC, PPL, TAS) are computed using the provided notebooks

Exact experiment settings for each result are documented inside the corresponding
notebook under `experiments/`.

## 🙏 Acknowledgments

- HuggingFace for Transformers library and datasets

---

## 📜 License

This project is for academic research purposes.

---
