# kidney_cancer_classification-using-ResNet50-with-multiscaling
About the Data Set:
## TCGA-KIDNEY-FINAL Dataset Description

The **TCGA-KIDNEY-FINAL** dataset is a curated collection of histopathological kidney cancer images derived from the The Cancer Genome Atlas. It is specifically designed to support research in medical image analysis, computational pathology, and deep learning-based cancer diagnosis.

This dataset primarily focuses on kidney cancer and includes tissue samples associated with major renal carcinoma subtypes such as kidney renal clear cell carcinoma (KIRC), kidney renal papillary carcinoma (KIRP), and kidney chromophobe (KICH). The images are obtained from whole-slide histopathology scans, capturing detailed cellular and tissue-level structures that are critical for cancer identification and grading.

The “FINAL” version of the dataset indicates that the data has undergone preprocessing and curation steps, including quality filtering, normalization, and structured organization. As a result, the dataset is typically free from corrupted samples and is arranged in a format suitable for machine learning workflows, such as training, validation, and testing splits.

Each image in the dataset represents microscopic views of kidney tissue, enabling the analysis of morphological patterns such as tumor architecture, cell density, nuclear abnormalities, and stromal characteristics. These features are essential for distinguishing between normal and cancerous tissues, as well as for identifying different cancer subtypes.

The dataset is widely used for various tasks, including:

* Multi-class classification of kidney cancer subtypes
* Binary classification (tumor vs. normal tissue)
* Feature extraction and representation learning
* Development and evaluation of deep learning models

Due to its origin from TCGA, the dataset benefits from high-quality medical imaging standards and is often associated with clinically validated annotations. This makes it a reliable resource for developing robust and generalizable AI models in healthcare.

However, working with this dataset may present challenges such as class imbalance, large image sizes, and variability in staining and imaging conditions. Proper preprocessing, augmentation, and normalization techniques are often required to achieve optimal performance.

In summary, the TCGA-KIDNEY-FINAL dataset serves as a valuable benchmark for kidney cancer research, providing rich histopathological data that supports advanced analysis and model development in the field of medical imaging.
Model Analysis:
## Deep Learning Pipeline and Model Architecture

This project implements a deep learning framework for kidney cancer classification using histopathological images from the TCGA-KIDNEY-FINAL dataset. The pipeline is designed to ensure robust feature extraction, efficient training, and reliable evaluation through a structured workflow.

### Data Preprocessing and Augmentation

The dataset is organized into training, validation, and test splits using a folder-based structure. Images are loaded using PyTorch’s `ImageFolder` utility. To improve generalization and reduce overfitting, several preprocessing and augmentation techniques are applied:

* Resizing all images to a fixed resolution of 224 × 224 pixels
* Random horizontal and vertical flipping
* Random rotation for spatial variability
* Normalization using ImageNet mean and standard deviation

Validation and test datasets are only resized and normalized to maintain consistency during evaluation.

---

### Data Loading

The preprocessed datasets are fed into `DataLoader` objects with a batch size of 16. Shuffling is enabled for the training set to ensure randomness, while parallel data loading (`num_workers=4`) improves performance.

---

### Model Architecture: Multi-Level Feature Fusion ResNet50

The model is based on a pre-trained ResNet50 architecture, leveraging transfer learning from ImageNet. The backbone network is frozen to retain learned low-level and mid-level visual features.

A key innovation in this model is **multi-level feature fusion**, where features are extracted from different depths of the network:

* Early-layer features (low-level textures and edges)
* Mid-layer features (intermediate patterns)
* Deep-layer features (high-level semantic information)

These features are obtained from specific layers, globally pooled, and flattened. The resulting feature vectors are concatenated to form a comprehensive representation of the input image.

The fused feature vector is then passed through a fully connected classifier consisting of:

* A dense layer with ReLU activation
* Dropout for regularization
* Final classification layer for multi-class prediction

---

### Handling Class Imbalance

To address class imbalance in the dataset, class weights are computed using scikit-learn and incorporated into the cross-entropy loss function. This ensures that underrepresented classes contribute more significantly during training.

---

### Training Strategy

The model is trained using the Adam optimizer with a learning rate of 0.0001 and weight decay for regularization. A learning rate scheduler (`ReduceLROnPlateau`) dynamically reduces the learning rate when validation loss stagnates.

The training process includes:

* Forward propagation and loss computation
* Backpropagation and weight updates
* Tracking of performance metrics per epoch

An early stopping mechanism is implemented to prevent overfitting by halting training when validation loss does not improve for a specified number of epochs.



### Evaluation Metrics

Model performance is evaluated using multiple metrics to provide a comprehensive assessment:

* Accuracy
* Precision (weighted)
* Recall (weighted)
* F1-score (weighted)
* Sensitivity (true positive rate)
* Specificity (true negative rate)

A confusion matrix is also generated to visualize classification performance across different classes.



### Testing and Visualization

After training, the best-performing model (based on validation loss) is loaded and evaluated on the test dataset. The evaluation includes:

* Final performance metrics
* Confusion matrix visualization
* Detailed classification report

Additionally, incorrectly classified samples are analyzed and visualized to better understand model limitations and failure cases.

### Loss Vs Epoch and Accuracy Vs Epoch(for both val and train):
<img width="1192" height="490" alt="image" src="https://github.com/user-attachments/assets/b08bef84-e807-44b0-9b12-60c2c3dca904" />
### Classification Matrics:
<img width="598" height="523" alt="image" src="https://github.com/user-attachments/assets/cdc421f8-f859-4dba-9ccc-a6ca4cf16b4d" />

### Classification Report:
<img width="533" height="247" alt="image" src="https://github.com/user-attachments/assets/79555112-70f2-4583-9ae5-7ca3d5f1e77b" />
### Test Accuracy:
* Accuracy= 0.8766
* Precision (weighted)=0.8786
* Recall (weighted)=0.8766
* F1-score (weighted)=0.8765
### Test Result:
<img width="1188" height="510" alt="image" src="https://github.com/user-attachments/assets/8e69b584-f91e-4f03-a584-fff6a9bd5685" />
<img width="1193" height="485" alt="image" src="https://github.com/user-attachments/assets/dee9e3a1-604f-45ec-b4fc-67c01d370c1a" />



### Summary

This pipeline integrates transfer learning, multi-level feature fusion, and robust evaluation techniques to effectively classify kidney cancer images. The combination of deep hierarchical features and careful training strategies enables improved performance in medical image classification tasks.

