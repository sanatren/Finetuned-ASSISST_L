# Model Research & Selection
Below are three promising approaches for detecting audio deepfakes, selected from the curated Audio Deepfake Detection GitHub repository. These were chosen based on their performance, innovation, and suitability for real-time or conversational audio detection.

## AASIST (Audio Anti-Spoofing using Integrated Spectro-Temporal Graph Attention Networks)
Key Technical Innovation:

Combines log-mel spectrogram features with a Spectro-Temporal Graph Attention Network (STGAT).

Captures both time and frequency context using GAT for more robust spoof detection.

Reported Performance Metrics:

EER: 0.83%

min t-DCF: 0.0275 (ASVspoof 2019 LA)

Why It's Promising for Our Use Case:

Lightweight model (AASIST-L has ~85k parameters), suitable for real-time applications.

Open-source, reproducible with pretrained models.

Strong performance across diverse spoofing attacks.

Potential Limitations:

Designed for clean lab datasets — real-world generalization may require further training.

Relies on handcrafted log-mel features (less end-to-end than others).

## Dual-Branch Multi-Task CNN + Classifier
Key Technical Innovation:

Uses a dual-branch CNN: one for spectral features and one for temporal features.

Incorporates multi-task learning (spoof detection + auxiliary tasks like speaker ID).

Reported Performance Metrics:

EER: ~1.27% (ASVspoof 2019 LA, from reference paper)

Why It's Promising for Our Use Case:

Learns richer representations from both frequency and time domains.

Multi-task setup could generalize well to conversational scenarios or noisy data.

Potential Limitations:

More computationally heavy than AASIST-L.

Less well-supported in open-source repositories — may require reimplementation.

## Self-Supervised Learning (Wav2Vec 2.0 / WavLM)
Key Technical Innovation:

Leverages pre-trained SSL models like Wav2Vec2.0 or WavLM, trained on massive unlabeled audio corpora.

Extracts rich, context-aware embeddings from raw audio for downstream classification.

Reported Performance Metrics:

EER: ~1.5% – 2.5% depending on fine-tuning and setup (based on multiple studies)

Why It's Promising for Our Use Case:

Excellent transfer learning capabilities, especially in low-data scenarios.

Robust to variations in speech content and speaker identity.

Potential Limitations:

Requires more computational resources (especially during inference).

May not be real-time friendly without quantization or distillation.

Fine-tuning needs care to avoid overfitting to spoof-specific cues.
