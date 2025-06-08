#  Audio Deepfake Detection

This repository contains my solution for the audio deepfake detection, aimed at detecting AI-generated human speech, suitable for real-time applications, and analyzing realistic conversational data.

[COLAB NOTEBOOK](https://colab.research.google.com/drive/1xF8--sYdszkFjkczM7W9ENWIq6IJGJgJ?usp=sharing)
---

## Project Overview

The goal of this project was to:

- Research existing audio deepfake detection approaches.
- Select promising models aligned with needs (real-time, conversational).
- Implement and fine-tune one chosen approach.
- Document and critically analyze the approach, challenges, and future considerations.

---

## Part 1: Research & Selection

Based on resources from the [Audio Deepfake Detection GitHub](https://github.com/media-sec-lab/Audio-Deepfake-Detection), the following three approaches were selected:

###  1. **AASIST (Audio Anti-Spoofing using Integrated Spectro-Temporal Graph Attention Networks)**

- **Technical Innovation:** 
  - Spectro-temporal Graph Attention Network combining spectral and temporal context.
- **Reported Performance:**
  - EER: **0.83%**, min t-DCF: **0.0275** (ASVspoof 2019 LA)
- **Promising because:**
  - Lightweight, suitable for real-time scenarios.
  - Robust detection capability proven on ASVspoof datasets.
- **Limitations:**
  - Potential performance drop in noisy, real-world environments.

###  2. **Dual-Branch Multi-Task CNN**

- **Technical Innovation:** 
  - Separate CNN branches for time-domain and spectral-domain feature extraction with multi-task learning.
- **Reported Performance:**
  - EER: ~**1.27%** (ASVspoof 2019 LA)
- **Promising because:**
  - Strong feature extraction capabilities, potentially robust for conversational audio.
- **Limitations:**
  - Computationally heavier, challenging to implement in real-time.

###  3. **Self-Supervised Learning (SSL) Models (e.g., Wav2Vec2.0, WavLM)**

- **Technical Innovation:** 
  - Pre-trained SSL embeddings capturing rich contextual audio representations.
- **Reported Performance:**
  - EER: **~1.5–2.5%**, varies based on setup.
- **Promising because:**
  - Excellent generalization, suitable for complex conversational audio.
- **Limitations:**
  - High computational resources required, real-time deployment challenging.

---

##  Part 2: Implementation

###  Chosen Approach: **AASIST-L**

**Reason for selection:**  
- **Efficiency & Simplicity:**
- AASIST is a lightweight model that uses a graph attention mechanism to capture both temporal and spectral features in one unified architecture.
- Its design offers a good balance between high detection accuracy and low computational overhead—ideal for near real-time processing.

- **Comparison with Other Approaches:**
- Dual-Branch Multi-Task Network: While very accurate, it requires managing two separate feature extractors (LFCC & CQT) and additional multi-task training, which increases complexity.
- SSL Pretrained Models (wav2vec 2.0/WavLM): These models are state-of-the-art but tend to be large and computationally heavy, making them less ideal for rapid prototyping or deployment on limited hardware.
- AASIST, on the other hand, offers a straightforward implementation path while meeting the key requirements of detecting AI-generated speech in real conversations.


###  Implementation Details

- **Dataset:** [ASVspoof 2019 Logical Access (LA)](https://datashare.ed.ac.uk/handle/10283/3336)
- **Framework:** [ClovaAI AASIST Official Repository](https://github.com/clovaai/aasist)
- **Fine-tuning configuration:**
  - Epochs: `10`
  - Batch size: `48` (trained on NVIDIA A100 GPU)
  - Learning Rate: `5e-5`
  - Configuration adapted to `.json` for ease of use in Google Colab.

### 📊 Performance Results

| Metric       | Achieved Result |
|--------------|-----------------|
| **EER**      | `0.992%`        |
| **min-tDCF** | `0.0308`        |

---
#### Equal Error Rate (EER) (0.992%):

Measures the point at which the false acceptance rate (FAR) (falsely labeling fake audio as real) and the false rejection rate (FRR) (falsely labeling real audio as fake) are equal.

Lower EER indicates better performance—a model with 0% EER would perfectly distinguish between real and fake speech.

An EER of 0.992% means the model is highly accurate, rarely misclassifying fake audio as real or vice versa.

#### Minimum Tandem Detection Cost Function (min-tDCF) (0.0308):

A composite metric that evaluates the overall cost of errors considering both the audio deepfake detection system and an automatic speaker verification system (ASV).

Considers the practical impacts of misclassification in realistic scenarios, assigning costs to false positives and negatives based on real-world consequences.

Lower min-tDCF indicates better overall effectiveness in practical use-cases.

A min-tDCF of 0.0308 indicates very strong performance and low risk of significant real-world errors.


##  Part 3: Documentation & Analysis

###  Implementation Process
- Cloned and adapted the original AASIST repo.
- Addressed compatibility issues (`np.float` deprecation).
- Fine-tuned model successfully with dataset preparation and config adaptations.

###  Challenges and Solutions
- **Challenge:** NumPy deprecated attributes causing evaluation errors.
- **Solution:** Updated codebase (`np.float` replaced by `float`).

###  Model Explanation
- Utilizes Spectro-temporal Graph Attention Networks (GAT) on log-mel spectrograms.
- Leverages attention to capture both temporal and frequency domain features effectively.

###  Strengths and Weaknesses
- **Strengths:** Lightweight, accurate, suitable for real-time usage.
- **Weaknesses:** Possible lower performance in noisy, varied real-world audio conditions.

###  Suggestions for Future Improvements
- Incorporate data augmentation (noise, reverberation).
- Evaluating on newer datasets (e.g., ASVspoof 2021).
- Considering domain adaptation and transfer learning with SSL embeddings.

---

##  Reflection Questions

**1. Significant challenges during implementation?**  
- Managing NumPy compatibility issues.
- Handling large dataset efficiently in Colab environment.

**2. Real-world performance vs research datasets?**  
- Performance is strong on lab datasets but likely would degrade slightly in real-world noise, different accents, or compression scenarios.

**3. Additional data/resources to improve performance?**  
- More diverse conversational datasets with varied accents, languages, and noise conditions.

**4. Deployment approach for production?**  
- Deploy as lightweight REST API (FastAPI/Flask).
- Convert to optimized inference model (TorchScript/ONNX).
- Continuously retrain and monitor performance for evolving threats.

---

##  Repository Structure

```
aasist-deepfake/
├── config/
│   └── AASIST-L.json              # Configuration file for AASIST-L model
├── evaluation.py                  # Evaluation script (patched version)
├── main.py                        # Main training/inference script
├── requirements.txt               # Python dependencies
├── AASIST_L_FineTuning_Momenta.ipynb ✅ # Fine-tuning notebook
├── README.md ✅                    # Project documentation
├── analysis.md (optional)         # Optional analysis or part of README
├── models/
│   └── weights/
│       └── AASIST-L.pth           # Pretrained model weights (or external link)
├── exp_result/
│   └── LA_AASIST-L_ep10_bs48/
│       └── metrics/
│           ├── dev_score.txt      # Development set metrics
│           └── eval_score.txt     # Evaluation set metrics
```


## Download dataset:

python download_dataset.py

## Training & Evaluation:

python main.py --config ./config/AASIST-L.json
python main.py --eval --config ./config/AASIST-L.json

## Credits & References
AASIST original repository: [ClovaAI AASIST](https://github.com/clovaai/aasist?tab=readme-ov-file)

ASVspoof 2019 LA dataset: University of Edinburgh
