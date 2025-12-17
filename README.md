# 🚀 Trigger-Based Backdoor Attacks on Large Language Models  
*A Comparative Study of Attack Strength, Stealthiness, and Detectability*

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-blue" />
  <img src="https://img.shields.io/badge/Models-BERT%2C%20GPT2%2C%20FLAN-orange" />
  <img src="https://img.shields.io/badge/Backdoor%20Types-4-green" />
  <img src="https://img.shields.io/badge/Status-Research%20Project-blueviolet" />
</p>

This repository contains the full implementation and analysis for my master’s thesis on **trigger-based backdoor attacks in LLMs**.  
The study systematically compares multiple trigger types, architectures, and defenses to uncover how backdoors behave and why some are far harder to detect than others.

---

## 📌 Table of Contents
- [Overview](#-overview)
- [Models & Datasets](#-models--datasets)
- [Trigger Types](#-trigger-types)
- [Key Findings](#-key-findings)
- [Repository Structure](#-repository-structure)
- [How to Run](#-how-to-run)
- [Thesis](#-thesis)
- [Contact](#-contact)

---

## 🔍 Overview

Backdoor attacks modify a model during training so it behaves normally on clean inputs but flips to an attacker-chosen output when a hidden trigger appears.

This study compares four trigger types across multiple architectures (**BERT**, **GPT-2**, **FLAN-T5**) using three benchmark datasets:

- **SST-2** (sentiment)
- **OLID** (offensive detection)
- **AG News** (topic classification)

The goal is to understand:

- Which triggers produce the highest **Attack Success Rate (ASR)**  
- How each trigger affects **Clean Accuracy (CACC)**  
- How detectable they are through **perplexity**, **explainability**, and **ONION defense**  
- How architecture and tokenization influence vulnerabilities  

---

## ⚙️ Models & Datasets

### **Models**
| Model | Type | Tokenizer |
|-------|--------|------------|
| BERT-base | Encoder | WordPiece |
| GPT-2 | Decoder | BPE |
| FLAN-T5-base | Encoder-Decoder | SentencePiece |

### **Datasets**
- **SST-2** → Positive / Negative  
- **OLID** → Offensive / Not offensive  
- **AG News** → 4-class news topics  

**Poisoning rate:** 10%  
**Trigger positions tested:** Start, middle, end of sentence  

---

## 🎯 Trigger Types

### **1. 🟪 Static Token Trigger**
A rare or synthetic token injected into poisoned samples.

### **2. 🟦 Semantic Trigger**
A friendly, natural-sounding sentence that quietly activates the backdoor.

### **3. 🟧 Syntactic Trigger**
Sentences rewritten using SCPN into unusual syntax patterns.

### **4. 🟩 Invisible Unicode Trigger**
Zero-width characters or LTR/RTL markers hidden inside tokens.

Each trigger type is evaluated using:
- **ASR**
- **CACC**
- **Perplexity (PPL)**
- **Explainability signatures**
- **Defense performance (ONION)**

---

## 📈 Key Findings (Short Summary)

- Semantic & syntactic triggers achieve **extremely high ASR** while remaining **highly stealthy**.  
- Invisible Unicode triggers bypass many detection methods.  
- Capitalization triggers largely avoid ONION detection.  
- Static rare tokens are easy to detect but still effective.  
- Architecture + tokenizer design significantly influence vulnerability.  
- Clear patterns emerge in attribution and attention maps, even when metrics look normal.  

---

## 📁 Repository Structure

llm-backdoor-thesis/
├── data/ # Dataset splits
├── models/ # Saved checkpoints
├── triggers/ # Trigger definitions & injection code
├── training/ # Fine-tuning scripts
├── evaluation/ # ASR, CACC, PPL metrics
├── explainability/ # Attribution & attention analysis
├── defense/ # ONION defense implementation
├── results/ # Tables, graphs, visualizations
└── thesis/ # PDF
