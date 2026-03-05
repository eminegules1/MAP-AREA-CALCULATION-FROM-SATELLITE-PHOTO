# Map Area Calculation from Satellite Photo

This project implements an automated semantic segmentation pipeline to identify **water bodies** in satellite imagery and calculate their physical surface area in square kilometers. Using a Deep Learning approach combined with traditional computer vision techniques, the system achieves high-precision geographic measurements.

---

## 🚀 Project Overview

The automated analysis of Earth's surface is vital for environmental monitoring and urban planning. While this pipeline can theoretically be applied to various topographical features, this implementation specifically targets **Water Bodies** as a robust proof-of-concept for automated area calculation.

### Key Achievements

* 
**High Precision:** Achieved a Mean Intersection over Union (**mIoU**) of **0.8441** and a **Pixel Accuracy** of **94.51%**.


* 
**Stable Training:** Effective convergence over 50 epochs with a final Validation Loss of 0.1628.


* 
**Automated Measurement:** Integrated algorithm to convert segmented pixel coordinates into physical area ($km^2$) based on Ground Sample Distance (GSD).



---

## 🛠️ Methodology & Tech Stack

### 1. Image Processing (OpenCV)

Raw Sentinel-2 satellite data is preprocessed to align with the neural network's requirements:

* 
**Color Space Transformation:** Converts default BGR matrices to **RGB** to match the model's expected input distribution.


* 
**Dimensionality Reduction:** Ground truth masks are processed as grayscale to simplify the task into a binary classification (Land vs. Water).


* 
**Geometric Transformation:** Inputs are normalized to a **256 x 256** vector using bilinear interpolation.



### 2. Deep Learning Architecture

The project utilizes a **U-Net** architecture, the industry standard for semantic segmentation:

* 
**Encoder (Backbone):** ResNet34 (pretrained on ImageNet) for feature extraction.


* 
**Decoder:** Upsamples features to generate a pixel-perfect mask matching the original resolution.



### 3. Area Calculation Logic

The physical area is derived from the predicted binary mask using a **10m Ground Sample Distance (GSD)**:

1. 
**Thresholding:** The probability map is converted into a binary mask.


2. 
**Pixel Counting:** `np.count_nonzero()` sums the water pixels.


3. **Conversion:**


$$\text{Area}(km^2) = \frac{\text{Water Pixels} \times (10m \times 10m)}{1,000,000}$$






---

## 📊 Experimental Results

The model was trained on a P100 GPU for 50 epochs.

| Metric | Value | Significance |
| --- | --- | --- |
| **mIoU** | 0.8441 | Considered state-of-the-art for satellite segmentation (>0.80) 

 
| **Pixel Accuracy** | 94.51% | Correctly classified nearly 95% of all pixels 

 
| **Validation Loss** | 0.1628 | Indicates stable convergence and low error 



**Qualitative Note:** While standard thresholding (0.5) occasionally missed faint water boundaries, adjusting the detection threshold to **0.3** significantly improved edge detection and area calculation accuracy for difficult samples.

---

## 📂 Dataset & References

* 
**Dataset:** [Satellite images of water bodies](https://www.kaggle.com/datasets/franciscoescobar/satellite-images-of-water-bodies) by Escobar, F. (2020).


* 
**Model Library:** [Segmentation Models PyTorch](https://github.com/qubvel/segmentation_models.pytorch) by Yakubovskiy, P. (2020).


