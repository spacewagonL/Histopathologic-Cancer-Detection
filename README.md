# Histopathologic Cancer Detection - Kaggle Competition  

This repository contains my solution for the **Histopathologic Cancer Detection** Kaggle competition. The goal of this project is to build a **Convolutional Neural Network (CNN) model** to detect metastatic tissue in histopathologic scans of lymph node sections.

---

## Dataset Overview  
- The dataset consists of histopathology images labeled as **0 (benign) or 1 (malignant)**.
- The train set contains labeled images, while the test set (**57,458 images**) is unlabeled.
- Each image is **96x96 pixels** with RGB channels.

Competition Link: [Histopathologic Cancer Detection](https://www.kaggle.com/competitions/histopathologic-cancer-detection/)

---

## Approach  

### 1. Data Preprocessing
- Used `ImageDataGenerator` to efficiently load and augment images.
- Applied real-time data augmentation: rotation, flipping, and zooming to improve generalization.
- Normalized images to scale pixel values between `[0,1]`.
- The dataset was split into **80% training / 20% validation**.

### 2. Model Architecture (CNN)
- **Model**
  - `Conv2D(32) → MaxPooling`
  - `Conv2D(64) → MaxPooling`
  - `Flatten() → Dense(128) → Dropout(0.5) → Output`
  - Activation: `ReLU` (hidden layers), `Sigmoid` (output)
  - Optimizer: Adam (`learning_rate=0.0005`)
  - Loss Function: Binary Crossentropy

### 3. Training Strategy
- Used early stopping to halt training if validation loss stopped improving.
- Implemented ReduceLROnPlateau to adjust learning rate dynamically.
- Trained the model for 10-15 epochs.

### 4. Handling Large Test Set (57.5k Images)
- Instead of loading all images into memory, used:
  - `flow_from_dataframe()` for batch inference.  
  - Batch size of 32 to optimize speed and memory usage.  

### 5. Submission
- Ensured test image filenames matched Kaggle’s expected format (`id.tif → id`).
- Converted predictions into binary labels (0 or 1).
- Generated `submission.csv` and uploaded it to Kaggle.

---

## Results and Score  

| Metric | Score |
|------------|----------|
| Public Leaderboard Score | 0.7593 |
| Private Score | 0.7161 |

- The model achieved **75.93% AUC (public)** and **71.61% AUC (private)**.


---

## Challenges and Learnings  
- Memory issues with large test set were fixed by using `flow_from_dataframe()`.  
- Overfitting in initial models was improved with Dropout, BatchNorm, and Regularization.  
- Filename mismatches in submission were fixed by ensuring `.tif` was removed from `submission.csv`.  
- Optimizing for Kaggle score was achieved by tuning learning rate and data augmentation.

---

## How to Run the Model  
1. Clone this repository:  
```sh
git clone https://github.com/yourusername/histopathologic-cancer-detection.git
cd histopathologic-cancer-detection
