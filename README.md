**Murmur Classification from Phonocardiogram Recordings Using Scattering Transform and 1D-CNN**

**1. Introduction**

This project implements an automated pipeline to detect and classify heart murmurs from phonocardiogram (PCG) recordings using wavelet scattering transform features and a 1D convolutional neural network (CNN). Leveraging the CirCor DigiScope Phonocardiogram Dataset (v1.0.3).
The pipeline is designed to process raw .wav recordings and corresponding segmentation .tsv files, extract complete heart cycles, transform them into scattering coefficients, and perform multi-class classification into:

Absent (no murmur detected)

Present (murmur detected)

Unknown (uncertain cases)

This work is intended for research in biomedical signal processing and can be used as a baseline for automatic heart sound analysis in clinical decision support systems.

**2. Dataset**

For each subject and auscultation location (e.g., PV, AV, TV, MV, Phc), the dataset includes:

.wav file: raw heart sound recording

.hea: WFDB header file

.tsv: segmentation for S1/S2 annotations

ABCDE.txt: subject metadata—including murmur labels and demographic details

**3. Methodology**

**3.1 Preprocessing**

Filtering: A 5th-order Butterworth low-pass filter (cutoff = 500 Hz) is applied to remove high-frequency noise.

Cycle Extraction: Full cardiac cycles are identified from segmentation labels in the order [1, 2, 3, 4] (S1 → systole → S2 → diastole).

Normalization: Zero-mean and unit-variance scaling is applied to each cycle.

Length Normalization: Signals are either padded or truncated to a fixed length of MAX_LEN = 3000 samples.

**3.2 Feature Extraction**

Scattering Transform: Wavelet scattering (kymatio.Scattering1D) is applied to each cycle to capture time-frequency characteristics robust to small deformations.

**3.3 Data Balancing**

The Absent class is downsampled to match the size of the Present class.

The Unknown class is preserved entirely.

**3.4 Model Architecture**

The CNN consists of:

Two Conv1D layers (128 and 64 filters) with LeakyReLU activation and MaxPooling

Batch Normalization layers

Fully connected layers with Dropout

Softmax output for 3-class classification

**4. Training**

Cross-validation: 5-fold StratifiedKFold CV to estimate model performance

Final Training: Early stopping and model checkpointing on the validation set

Optimizer: Adam (learning rate = 0.001)

Loss: Sparse categorical crossentropy

Metrics: Accuracy

**5. Results**

Cross-validation performance:

**Mean accuracy: 0.7606**

**Standard deviation: 0.0061**
