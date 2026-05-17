# 📸 Sanskrit Image Captioning — CNN vs Vision-Language Models (VLMs)

## 🚀 Overview

This repository presents a **comparative evaluation framework** for Sanskrit image captioning:

* 🧠 **Custom Model (CNN-RNN based)**
* 🤖 **Vision-Language Models (VLMs)**

  * PaliGemma
  * Qwen2-VL
  * LLaVA
  * BLIP variants

The goal is to evaluate:

> ⚖️ *Can large pre-trained VLMs match domain-specific models in low-resource languages like Sanskrit?*

---

## 📂 Repository Structure

```
.
├── data/
│   ├── images/                  # Test images (600 samples)
│   └── ground_truth.txt         # 5 Sanskrit captions per image
│
├── VLM_Results/
│   ├── PaliGemma/
│   ├── Qwen2VL/
│   ├── Nalanda/
│   ├── Chitralekha/
│   ├── evaluation_all_models.json
│   └── plots/
│
├── CNN_Model_Results/
│   └── evaluation_cnn.json
│
├── notebooks/
│   ├── vlm_evaluation.ipynb
│   └── cnn_model_inference.ipynb
│
└── README.md
```

---

## 🔄 Pipeline

### 🧾 Input

* Image dataset (Flickr8k-based Sanskrit dataset)
* Each image has:

  * 🖼️ 1 image
  * 📝 5 ground-truth Sanskrit captions
  * 📝 1x5 = 5 caps

Dataset size:

* Train: 6,691 images
* Validation: 800 images
* Test: 600 images 

---

### ⚙️ Processing Flow

```
Image → Caption Generation → Save Predictions → Evaluation → Metrics → Plots
```

Two parallel pipelines:

#### 1️⃣ Custom Model Pipeline

* Input image
* Generate Sanskrit caption
* Save predictions

#### 2️⃣ VLM Pipeline (Few-shot)

* Select n examples (n = 5, 10, 15, 20)
* Provide:

  * Image + Sanskrit captions (few-shot context)
* Generate caption for unseen images

---

### 📤 Output

Each model produces:

```
model_name_n5.txt
model_name_n10.txt
model_name_n15.txt
model_name_n20.txt
```

Format:

```
image_name.jpg | generated_caption
```

Additional outputs:

* `evaluation_*.json` → all metric scores
* `plots/` → visual analysis
* `split_info.json` → few-shot sampling details

---

## 📊 Evaluation Metrics

All models are evaluated against **5 reference captions per image**.

### Metrics Used:

* **BLEU-1 / BLEU-2 / BLEU-3 / BLEU-4**
* **ROUGE-L**
* **METEOR**
* **CIDEr**

These are standard metrics used in image captioning research to measure:

* lexical similarity
* fluency
* semantic overlap

⚠️ Note: These metrics may not fully capture human-level quality but remain standard benchmarks.

---

## 📈 Experimental Setup

### Few-shot Evaluation

* n ∈ {5, 10, 15, 20}
* Random sampling with fixed seed
* Remaining images used for evaluation

### Evaluation Strategy

* Multi-reference scoring (5 GT captions per image)
* Invalid outputs filtered:

  * empty captions
  * errors
  * non-Sanskrit outputs

---

## 📊 Key Results

### 🧠 Custom CNN Model

* Achieved:

  * **BLEU-1 ≈ 77.8%**
* Outperformed all tested VLMs on Sanskrit captioning 

✔ Strong:

* grammatical correctness
* semantic alignment
* consistent Sanskrit output

---

### 🤖 VLM Performance Summary

| Model          | Output Quality | Issues Observed              |
| -------------- | -------------- | ---------------------------- |
| **LLaVA**      | Moderate       | Works in few-shot            |
| **PaliGemma**  | Partial        | Needs high n (n=20)          |
| **Qwen2-VL**   | Unstable       | Detection mode, Hindi output |
| **BLIP / GIT** | Failed         | English output only          |

---

### 🚨 Key Findings

* ❌ VLMs **fail in low-resource Sanskrit**

* ❌ Strong English models ≠ multilingual capability

* ❌ Few-shot prompting is not enough

* ✅ Task-specific training significantly improves performance

* ✅ Attention-based CNN models perform best

* ✅ Domain-specific datasets are critical

---

## 📉 Visualization Outputs

Generated plots include:

* 📊 Metric-wise bar charts
* 📈 BLEU score comparison
* 📉 ROUGE / METEOR / CIDEr trends
* 🔥 Heatmap of all metrics

---

## 🧪 Reproducibility

* Fixed random seed = 42
* Consistent dataset split
* Same evaluation metrics across models
* Standardized output format

---

## 💡 Key Takeaway

> ⚡ **Pre-trained VLMs do NOT replace domain-specific models in low-resource languages.**

Your custom Sanskrit captioning model:

* Learns linguistic structure
* Adapts to morphology
* Produces meaningful captions

While VLMs:

* Struggle with script consistency
* Fail to generalize to Sanskrit
* Often revert to English/Hindi

---

## 📌 Future Work

* Add CLIPScore / SPICE metrics
* Improve VLM prompting strategies
* Train multilingual VLMs for Sanskrit
* Expand dataset beyond Flickr8k

## 📎 Reference

This repository is based on the research paper:
📄 *Beyond Pre-trained VLMs: A CNN-RNN Framework for Low-Resource Sanskrit Image Captioning* 
