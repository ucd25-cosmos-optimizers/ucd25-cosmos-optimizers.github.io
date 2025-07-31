+++
title = "Project"
showReadingTime = false
showDate = false
showPagination = false
+++

## Introduction

### Overview

Epilepsy, also known as seizure disorder, is a medical condition characterized by recurring seizures due to abnormal electrical activity in the brain. In the United States, epilepsy affects almost 3 million adults. There are many ways to detect epileptic behavior in the brain, including electroencephalogram (**EEG**), magnetic resonance imaging (**MRI**), and computed tomography (**CT**) scans. 

However, what makes EEG such a popular method to measure brain activity is its excellent temporal resolution, relatively low cost, and noninvasive nature. It is able to measure brain activity by recording electrical activity through a patient's skull and scalp. Despite this, symptoms of epilepsy are not guaranteed to be present at all times of data collection. Thus, this process can take long periods of monitoring, generating large amounts of data.

![Process of EEG Detection](/eeg_image.png "EEG signals capturing brain activity to detect epilepsy")

### Motivation

Our motivation for this project is to be able to automate the process of identifying abnormality within brain activity patterns of EEG data, making the process to diagnosis of epilepsy faster for patients. 

Automation with identification of EEG is especially important because of some difficulties when dealing with EEG, burdening experts and reducing efficiency: 

 1. There are small amounts of epilepsy data available simply due to the infrequency of seizure       occurences.
 2. Presence of noise and artifacts in the data disturbs learning brain patterns during ictal cases.
 3. Inconsistency in seizure activitation among different patients also hinders pattern learning.  

Following a structed data science pipeline, our objective is to build a binary classification model to determine whether or not a patient is epileptic given EEG data.

---------
## Data

 Our data is sourced from hospital EEG recordings of healthy and epileptic patients, recorded across five electrode channels. The exact nodes are as follows:
 
 - A1: Placed on the ear to measure the average of all electrodes
 - C3: Placed on the left primary motor cortex, measures motor movement of the right hand
 - C4: Placed on the right primary motor cortex, measures motor movement of the left hand
 - CZ: Placed on the center top of the head, measures possible temporal lobe epilepsy
 - F3: Placed on the left frontal lobe, measures motor control and imagined movement

![EEG Nodes for Dataset](/eeg_nodes.png "Electrode channels collected in the EEG dataset")
  
The data contains 411 recordings of people (**epochs**) per channel over 25 minutes at a sampling frequency of 200 Hz. Labels of healthy (visualized in blue) and epileptic (visualized in red) were given. 

--------
## Methodology

### Data Cleaning

We applied a bandpass filter to filter out unwanted noise, such as background electrical activity. We kept the frequencies 0.5 Hz to 45 Hz, which corresponds to the range of alpha, beta, gamma, and delta waves. See our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/bandpass%20filter.ipynb" target = "_blank">here</a>. 

![Bandpass Graph](/bandpass_graph.png "Sample of bandpass filter on one epoch of EEG data")

A visualization of sample EEG waves across channels found that for the same epoch, wave patterns remained consistent across channels. 

![Sample Wave Channels](/wave_channels.png "Sample waves of epochs across electrode channels")

#### Feature Extraction

Even with filtered data, the number of timepoints (28,500) is too many to fit into a model. Thus, we extracted statistical measurements (**features**) from the data, reducing computional costs of our models. The following features were extracted: 

**Basic statistical features** like mean, median, range, quartiles, variance, standard deviation, skew, and kurtosis (measures the relative number of outliers).

**Wave features** like signal intensity, signal trend, zero crossing rate (number of times the signal changes signs), and number of peaks.

**Spectral features (frequency domain features)** like spectral centroid (frequency-weighted average), spectral bandwidth, spectral rolloff (85% cutoff rate), and peak frequency.

We also separated each eeg wave into its composite brain waves. Because our data was downsampled to 20 Hz, as per the Nyquist-Shannon Sampling Theorem, we are only able to accurately capture frequencies from 0-10 Hz. This allows us to evaluate delta (0.5-4 Hz), theta (4-7 Hz), and parts of alpha (8-12 Hz) waves. We did not have the data to evaluate beta (12-30 Hz) and gamma (30-100 Hz) waves. The low-frequency brain waves we did evaluate are associated with subconscious or relaxed brains states. We extracted **basic statistical features** regarding the composite delta, theta, and low alpha waves. 

All together, we preliminarily extracted 41 features per electrode channel, 205 features in total.

Check out our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/feature%20extraction.ipynb" target = "_blank">here</a>.

### EDA

A correlation heatmap of all of the features we extracted for the electrode channel A1 reveals two main useful insights.

![Correlation Heatmap of Features A1](/correlation_heatmap.png "Initial Correlation Heatmap for Extracted Features on the A1 dataset")

1. The heatmap shows some features that might be more important to the final model. In particular, the **zero crossing rate**, **number of peaks**, **spectral centroid**, **spectral bandwidth**, and **spectral rolloff** have stronger correlations with the **label** (healthy or epileptic) compared to other features.
2. Many features have a perfect linear correlation, or very weak correlation. As such, we can save computing power by selecting only the most important features to feed into our models.

#### Feature Selection

We selected the top 15 features from each electrode channel using the **F-Test**, leaving us with a much more managable 75 features in total. The **F-Test** is a statistical test used to measure the significance of a feature compared to the label (healthy or epileptic).

Explore our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/feature%20selection.ipynb" target = "_blank">here</a>.

A correlation heatmap the these selected features show that most variables with 0 linear correlation to the labels have been removed. As expected, the **zero crossing rate**, **number of peaks**, **spectral centroid**, **spectral bandwidth**, and **spectral rolloff** are all kept. 

![New Correlation Heatmap of Features A1](/new_corr_heatmap.png "Correlation Heatmap for Selected Features on the A1 dataset")

![Split Correlation Heatmap of Features A1](/split_corr_heatmap.png "Split Correlation Heatmap of the A1 dataset: Healthy vs. Epileptic")

![Difference Correlation Heatmap of Features A1](/diff_corr_heatmap.png "Difference in Correlation Heatmap of the A1 dataset: Healthy vs. Epileptic")

Some correlations between the **spectral mean** and **basic statistical features** are significantly larger for epileptic eeg samples compared to healthy samples. These correlations can be used to classify eeg signals in our models.

### Modeling

We compared multiple classes of models to see which would be the most effective. Below are some that we tried:

**Logistic Regression**: fits a "best-fit" line, then uses that line to calculate the probability of categorical outcomes (in this case healthy or epileptic). View our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/logistic%20regression.ipynb", target = "_blank">here</a>. 

--------
## Results

|           | Logistic Regression | Model 2 | Model 3 |
| :--------:  | :-------: | :-------: | :-------: | 
| Accuracy  |   0.82    |   hi    |    hi   | 
| Precision |   0.87    |   hi    |    hi   |
| Recall    |   0.77    |   hi    |    hi   |

--------

## Discussion
