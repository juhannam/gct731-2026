# Homework #2: Beat Tracking and Chord Recognition

**Beat tracking** and **chord recognition** are fundamental tasks in Music Information Retrieval (MIR). In this assignment, you will implement and analyze classic approaches to both tasks, focusing on **dynamic programming** for beat tracking and **Hidden Markov Models (HMMs)** for chord recognition. You will also investigate how preprocessing techniques and model configurations affect performance.

---

## Audio and Labels
We will primarily use the Beatles' song **"Let It Be"** for this homework.
* **Audio:** You can obtain the audio files (downsampled to 22.05 kHz) from the [FMP website](https://www.audiolabs-erlangen.de/resources/MIR/FMP/C5/C5S3_ChordRec_Beatles.html).
* **Labels:** Beat and chord annotations are available via the [Isophonics Website](https://isophonics.net/content/reference-annotations-beatles).

## Starter Code
We provide a Python notebook, [GCT731-HW2.ipynb](https://colab.research.google.com/drive/1H_DpcVIXaO7xdChK7NlMotxiEBeDZ6dr?usp=sharing), to facilitate your implementation. You will extend this basic implementation by following the instructions in the sections below.

---

## Question 1: Beat Tracking via Dynamic Programming (15 pts)

Run the beat tracking method using dynamic programming on the **intro section** of the song (0–11 seconds). The code will return a tempo value, a visualization comparing ground truth vs. predicted beat times, and evaluation scores (F-measure, CMLc, CMLt, AMLc, AMLt). You should observe near 100% accuracy.

Next, run the same code on the **verse section** (15–26 seconds), where vocals are mixed with the piano. **Analyze the results:** you will likely notice a drop in performance. Your task is to improve the method using the following techniques:

### 1.1 Harmonic-Percussive Source Separation (HPSS)
The primary difference between the intro and the verse is the presence of vocals. While high-quality source separation often requires complex deep learning models, we can use **HPSS** as a computationally efficient alternative to isolate the percussive or harmonic components.

* **Study:** Read the [HPSS paper](https://arrow.tudublin.ie/cgi/viewcontent.cgi?article=1078&context=argcon) and review the [Librosa documentation](https://librosa.org/doc/main/auto_examples/plot_hprss.html).
* **Implementation:** Apply HPSS to the input audio and experiment with the parameters (e.g., kernel size). How much does this improve the tracking accuracy on the verse?

### 1.2 Tempo Prior
You may observe "octave errors," where the estimated tempo is exactly double or half the true tempo. To mitigate this, we use a **tempo prior** (typically a log-normal distribution) to favor common human-centric tempos.

* **Study:** Read the [original paper by Dan Ellis](https://www.ee.columbia.edu/~dpwe/pubs/Ellis07-beattrack.pdf) to understand the role of the tempo prior in the DP objective function.
* **Implementation:** Refer to the [Librosa PLP documentation](https://librosa.org/doc/latest/generated/librosa.beat.plp.html) for practical settings.



**Submission Requirements for Q1:**
Write a detailed report answering the following:
* Did HPSS improve results? Which parameters were most effective? Visualize the spectrograms and describe the auditory characteristics of the separated components.
* Did the tempo prior solve octave errors? What was your optimal parameter setting?
* Why do we need tempo-specific metrics like CMLc or AMLt in addition to the standard F-measure?
* Improve the beat tracking accuracy further with your own ideas. 

---

## Question 2: Chord Recognition via HMM (15 pts)

Run the **template-based** chord recognition on the verse section (15–26 seconds). It will return evaluation scores and a visualized comparison of the ground truth vs. predicted chords.

### 2.1 HMM-based Chord Recognition
You will notice that the template-based approach yields **highly jittery** predictions with significant **temporal instability** (over-segmentation). Your task is to implement an HMM to obtain a smooth, musically plausible sequence of chords.



* **Study:** Read Section 5.3 of the FMP book, ["Chapter 5: Chord Recognition"](https://www.audiolabs-erlangen.de/resources/MIR/FMP/C5/C5S3_ChordRec_HMM.html).
* **Evaluation:** Review the [MIREX Audio Chord Estimation task](https://music-ir.org/mirex/wiki/2025:Audio_Chord_Estimation) to understand the difference between CSR and chord segmentation accuracy.

**Submission Requirements for Q2:**
Write a detailed report answering the following:
* How much did the HMM improve the CSR and the **Over-segmentation** (temporal continuity) score?
* Compare three types of transition probability matrices: **uniform**, **estimated** (data-driven), and **transpose-invariant**. Use the pre-computed Beatles matrices provided in the [HMM-based Chord Recognition Section](https://www.audiolabs-erlangen.de/resources/MIR/FMP/C5/C5S3_ChordRec_HMM.html). Note the the data-driven transition probability matrix is not necessarily working best for this song. 
* Summarize your understanding of CSR versus segmentation accuracy. How does the HMM trade off between the two?
* **Integration (Bonus):** Explore how using the **beat-aligned chroma** (syncing chroma to the beats found in Q1 by averaging chroma vectors within one beat) affects the chord recognition results. This does not necessariliy improve the performance but it is worth trying. 

---

## Deliverables
Submit the following items to **KLMS**:
1. Your modified Python code (`.ipynb` or `.py` files).
2. A comprehensive homework report in **PDF format**.