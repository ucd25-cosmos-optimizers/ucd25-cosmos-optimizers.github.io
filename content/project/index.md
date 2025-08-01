+++
title = "EEG and Epilepsy: Binary Classification"
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

Following a structured data science pipeline, our objective is to build a binary classification model to determine whether or not a patient is epileptic given EEG data.

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

We applied a bandpass filter to filter out unwanted noise, such as background electrical activity. We kept the frequencies 0.5 Hz to 10 Hz, which corresponds to the frequencies we are able to capture. See our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/bandpass%20filter.ipynb" target = "_blank">here</a>. 

![Bandpass Graph](/bandpass_graph.png "Sample of bandpass filter on one epoch of EEG data")

A visualization of sample EEG waves across channels found that for the same epoch, wave patterns remained consistent across channels. 

![Sample Wave Channels](/wave_channels.png "Sample waves of epochs across electrode channels")

#### Feature Extraction

Even with filtered data, the number of timepoints (28,500) is too many to fit into a model. Thus, we extracted statistical measurements (**features**) from the data, reducing computional costs of our models. The following features were extracted: 

**Basic statistical features** like mean, median, range, quartiles, variance, standard deviation, skew, and kurtosis (measures the relative number of outliers).

**Wave features** like signal intensity, signal trend, zero crossing rate (number of times the signal changes signs), and number of peaks.

**Spectral features (frequency domain features)** like spectral centroid (frequency-weighted average), spectral bandwidth, spectral rolloff (85% cutoff rate), and peak frequency.

We also separated each EEG wave into its composite brain waves. Because our data was downsampled to 20 Hz, as per the Nyquist-Shannon Sampling Theorem, we are only able to accurately capture frequencies from 0-10 Hz. This allows us to evaluate delta (0.5-4 Hz), theta (4-7 Hz), and parts of alpha (8-12 Hz) waves. We did not have the data to evaluate beta (12-30 Hz) and gamma (30-100 Hz) waves. The low-frequency brain waves we did evaluate are associated with subconscious or relaxed brains states. We extracted **basic statistical features** regarding the composite delta, theta, and low alpha waves. 

All together, we preliminarily extracted 41 features per electrode channel, 205 features in total.

Check out our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/feature%20extraction.ipynb" target = "_blank">here</a>.

### EDA

A correlation heatmap of all of the features we extracted for the electrode channel A1 reveals two main useful insights.

![Correlation Heatmap of Features A1](/correlation_heatmap.png "Initial correlation heatmap for extracted features on the A1 dataset")

1. The heatmap shows some features that might be more important to the final model. In particular, the **zero crossing rate**, **number of peaks**, **spectral centroid**, **spectral bandwidth**, and **spectral rolloff** have stronger correlations with the **label** (healthy or epileptic) compared to other features.
2. Many features have a perfect linear correlation, or very weak correlation. As such, we can save computing power by selecting only the most important features to feed into our models.

#### Feature Selection

We selected the top 15 features from each electrode channel using the **F-Test**, leaving us with a much more managable 75 features in total. The **F-Test** is a statistical test used to measure the significance of a feature compared to the label (healthy or epileptic).

Explore our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/feature%20selection.ipynb" target = "_blank">here</a>.

A correlation heatmap of these selected features show that most variables with 0 linear correlation to the labels have been removed. As expected, the **zero crossing rate**, **number of peaks**, **spectral centroid**, **spectral bandwidth**, and **spectral rolloff** are all kept. 

![New Correlation Heatmap of Features A1](/new_corr_heatmap.png "Correlation heatmap for selected features on the A1 dataset")

![Split Correlation Heatmap of Features A1](/split_corr_heatmap.png "Split correlation heatmap of the A1 dataset: healthy vs. epileptic")

![Difference Correlation Heatmap of Features A1](/diff_corr_heatmap.png "Difference in correlation heatmap of the A1 dataset: healthy vs. epileptic")

Some correlations between the **spectral mean** and **basic statistical features** are significantly larger for epileptic eeg samples compared to healthy samples. These correlations can be used to classify eeg signals in our models.

### Modeling

We compared multiple classes of models to see which would be the most effective. We tried 6 different models:

**Logistic Regression**: A parametric model that fits a "best-fit" line, then uses that line to calculate the probability of categorical outcomes (in this case healthy or epileptic). View our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/logistic%20regression.ipynb" target = "_blank">here</a>. 

![Logistic Regression](/log_reg.png "Logistic regression top 10 most important features")

**K-Nearest Neighbors (K-NN)**: A non-parametric model that classifies points based on the majority classification of their "k" nearest neighbors. In our model, "k" was optimized to  8. See our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/knn.ipynb" target = "_blank">here</a>.

**Decision Tree**: A non-parametric model that makes a tree-like series of decisions conditionally classifying each instance. In our model, the depth of the tree was optimized to 3. Check out our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/decision%20tree.ipynb" target = "_blank">here</a>.

![Decision Tree](/decision_tree.png "Decision tree conditionals")

**Random Forest**: A non-parametric model that takes the average of many decision trees, each trained in parallel on a random subset of data, reducing overfitting. In our model, the maximum depth of each tree is optimized to 6 and the number of decision trees is optimized to 200. View our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/random%20forest.ipynb" target = "_blank">here</a>. 

![Random Forest](/rand_forest.png "Random forest top 10 most important features")

**Gradient Boosted Classification Tree**: A non-parametric model like random forest, but each tree is trained sequentially off of the errors of the last tree. In our model, the maximum depth of each tree is optimized to 3, the number of decision trees is optimized to 300, and the learning rate of each tree is optimized to 0.5. See our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/gradient%20boosted%20classification%20tree.ipynb" target = "_blank">here</a>.

**Multilayer Perceptron (MLP)**: A machine learning model and type of neural network. Our model consists of three hidden layers, bringing the feature dimensions from 75 to 64 to 32 to 16 to finally 1. Each hidden layer uses the ReLu activation function, and the output passes through a Sigmoid function to format it for binary classification. In our model, the dropout rate is optimized to 0.3, and the number of training epochs is optimized to 51. See our code <a href = "https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/neural%20network.ipynb" target = "_blank">here</a>.

![Multilayer Perceptron (MLP)](/multi_layer_percep.png "Multilayer perceptron (MLP) diagram")

--------
## Results

We used a few different metrics to compare how well our models perform:

**Accuracy**: A measure of how often the model predicts the label correctly (correct predictions/total number of predictions).

**Precision**: A measure of the accuracy of positive predictions made by the model (correct positive predictions/all positive predictions). In our case, precision measures the chance that someone actually has epilepsy if our model tells them they have epilepsy.

**Recall**: A measure of how well a model is of finding all positive (epileptic) cases (correct positive predictions/all positive cases). In our case, recall measures how many actually epileptic cases the model can diagnose as epileptic.

To us, recall is the most important metric, since undiagnosed and therefore untreated epilepsy can be very dangerous, sometimes even leading to death.

<div align="center">
  <table>
    <tr>
      <th style="width:60px; text-align:center;">Model</th>
      <th style="width:100px; text-align:center;">Logistic Regression</th>
      <th style="width:100px; text-align:center;">K-NN</th>
      <th style="width:100px; text-align:center;">Decision Tree</th>
     <th style="width:100px; text-align:center;">Random Forest</th>
     <th style="width:100px; text-align:center;">Gradient Boosted Regression Tree</th>
     <th style="width:100px; text-align:center;">MLP</th>
    </tr>
    <tr>
      <td style="text-align:center;">Accuracy</td>
      <td style="text-align:center;">0.82</td>
      <td style="text-align:center;">0.83</td>
      <td style="text-align:center;">0.75</td>
     <td style="text-align:center;">0.86</td>
     <td style="text-align:center;">0.87</td>
     <td style="text-align:center;">0.88</td>
    </tr>
    <tr>
      <td style="text-align:center;">Precision</td>
      <td style="text-align:center;">0.87</td>
      <td style="text-align:center;">0.81</td>
      <td style="text-align:center;">0.70</td>
     <td style="text-align:center;">0.86</td>
     <td style="text-align:center;">0.86</td>
     <td style="text-align:center;">0.88</td>
    </tr>
    <tr>
      <td style="text-align:center;">Recall</td>
      <td style="text-align:center;">0.77</td>
      <td style="text-align:center;">0.88</td>
      <td style="text-align:center;">0.88</td>
     <td style="text-align:center;">0.86</td>
     <td style="text-align:center;">0.88</td>
     <td style="text-align:center;">0.88</td>
    </tr>
  </table>
</div>

Across all metrics, the **Multilayer Perceptron** performed the best, identifying cases correctly **88%** of the time, ensuring that the people it diagnoses as epileptic are actually epileptic **88%** of the time, and catching **88%** of actual epilepsy cases.

![Confusion Matrix](/confusion_matrix.png "Confusion matrix for MLP")

--------

## Discussion

While neural nets like Multilayer Perceptrons are more like "black boxes," we can look to some other models like **Logistic Regression** (82% accuracy) and **Random Forest** (87% accuracy) to see which features are most important in classification.

![Comparison of Feature Importance](/discussion.png "Comparing feature importance between logistic regression and random forest models")

Spectral (frequency) features are the most considered across both models. C4 is the most important electrode channel, perhaps because it is the only electrode channel in our data that is placed on the right side of the brain.

In this project, we explored how to build an effective model with very limited (downsampled) data, which could have practical implications in areas that do not have the technical capibilities to collect the full range of EEG data. In the future, we hope to future explore data from other electrode channels as well as higher frequencies to create more accurate predictions that properly mirror how EEG data will be found in the real world.

-----------

See our entire workflow <a href="https://github.com/ucd25-cosmos-optimizers/optimizers/blob/chloe/notebooks/eeg.ipynb" target = "_blank">here</a>.

