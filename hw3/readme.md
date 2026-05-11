# Homework #3: Music Tagging and Retrieval 

Music tagging is a fundamental task in music information retrieval (MIR) that powers search engines and recommendation systems. The goal of this assignment is to understand the full lifecycle of a music tagging system—from raw audio processing to evaluating state-of-the-art pre-trained models. Specifically, 

* Implement a complete pipeline for music tagging: data preparation, feature extraction, model training, and performance evaluation.
* Analyze the structural differences between 1D and 2D CNN architectures for audio spectrograms.
* Explore performance optimization through data augmentation and modern SOTA architectures.
* Conduct linear or MLP probing on self-supervised pre-trained models to understand the power of representation learning.

---


### The Dataset
We use the [magnatagatune](https://mirg.city.ac.uk/codeapps/the-magnatagatune-dataset) dataset which has been widely used in the music tagging task.  The MagnaTagATune dataset consists of 25k music clips from 6,622 unique songs. The dataset contains 30-second audio files including 189 different tags. For this assignment, we will use a subset consisting of 9074 samples with 8 seconds of audio and only 50 genres.


## Starter Code
A Python notebook template is provided here, [GCT731-HW3.ipynb](https://colab.research.google.com/drive/1UoZ-UhbF5xEPDD2KDke_-7p7UIZ8kSOQ?usp=sharing). You are expected to extend this notebook to complete the tasks below.

---


### Question 1: Baseline 1D CNN (5 pts)
The baseline model utilizes a 1D CNN architecture that treats the mel-spectrogram as a 1D signal with multiple channels (frequency bins).

* Task: Run the baseline training code provided in the starter notebook.
* Report: Document the final training/validation loss, the evaluation metrics (ROC-AUC and PR-AUC), and discuss your observations from the experiment.


### Question 2: Implementing a 2D CNN (10 pts)
Standard 1D CNNs often fail to capture  time-frequency variation in the large filter size. In this section, you will implement a 2D CNN which uses small filters.  

* Task: Complete the to-do section. 
* Requirement: Ensure your convolutional layers, batch normalization, and pooling layers are correctly configured to handle 2D input $(Batch, Channel, Freq, Time)$.


### Question 3: Performance Optimization (15 pts)
Apply advanced techniques to improve the performance of the baseline 2D CNN. You must implement at least one of the following strategies and explain your reasoning:

* Architectural Search: Implement a "Short-Chunk CNN" or other CNN architecture such as ResNets. 
* Data Augmentation: Implement SpecAugment (time/frequency masking) or time-domain digital audio effects. For more detail about data augmentation using digital audio effects, see this [tutorial](https://music-classification.github.io/tutorial/part3_supervised/data-augmentation.html). 
* Hyperparameter Tuning: Systematically test different optimizers (e.g., AdamW), learning rate schedulers, or weight decay settings.

### Question 4: Probing Pre-trained Models (MERT) (10 pts)
In this task, you will evaluate the quality of representations from [MERT](https://arxiv.org/abs/2306.00107), a state-of-the-art transformer-based self-supervised learning model. 

* Feature Extraction: Use a pre-trained MERT model (e.g., [m-a-p/MERT-v1-95M via Hugging Face](https://huggingface.co/m-a-p/MERT-v1-95M)) as a frozen backbone. Do not update the weights of the MERT model itself. Also, check the [Githib link](https://github.com/yizhilll/MERT).  

* Linear Probe: Implement a simple linear layer on top of the extracted MERT features to perform the tagging task. 

* MLP Probe: Implement a Multi-Layer Perceptron (MLP) with one hidden layer (e.g., 512 units) and ReLU activation to act as the classifier.

* Comparison: Compare the performance of the Linear Probe vs. the MLP Probe. Discuss whether the added depth in the classifier significantly improves the tagging accuracy for pre-trained features.


## Deliverables
Submit the following items to **KLMS**:
1. Your modified Python code (`.ipynb` or `.py` files).
2. A comprehensive homework report in **PDF format**.


## Evaluation Criteria
* Technical Correctness: Correct model implementation
* Performance & Analysis: Ability to improve upon the baseline and provide a data-driven explanation for the results.
* Quantitative Reporting: Clear presentation of metrics (AUC, PR-AUC) across all experimental setups.
* Critical Discussion: Insightful discussion on the strengths/weaknesses of compared models.


