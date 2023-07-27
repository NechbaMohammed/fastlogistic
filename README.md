# HPC Logistic Package

## Overview

This Python package provides efficient implementations of logistic regression using high-performance computing techniques, with support for both CPU and GPU architectures. The algorithms are implemented in Python 3.8, and the GPU implementation leverages CUDA programming to accelerate the training process.

## Features

- `logistic_cpu`: Implementation of logistic regression using multi-core parallelism on the CPU. It is designed to efficiently handle large datasets and perform binary classification tasks.

- `logistic_gpu`: Implementation of logistic regression using CUDA programming to harness the power of modern GPUs. The GPU implementation aims to expedite the training process, particularly for big data scenarios.

## Authors and Contributors

- [Mohammed Nechba](https://www.github.com/NechbaMohammed), Applied Mathematics & AI engineering student at ENSIAS
- [Mohamed Mouhajir](https://github.com/mohamedmohamed2021), Applied Mathematics & AI engineering student at ENSIAS
- [Yassine Sedjari](https://github.com/Heyyassinesedjari), Applied Mathematics & AI engineering student at ENSIAS

## Requirements

- Python 3.8
- CUDA-enabled GPU (for using `logistic_gpu`)

## Installation

To install the package, you can use the following command :

```bash
  git clone git@github.com:NechbaMohammed/fastlogistic.git
```
# Comparison between the Two Versions

## Data Description
The [HIGGS dataset](http://archive.ics.uci.edu/dataset/280/higgs) was originally introduced in a research paper titled "Discovering the Higgs boson in the noise" by Baldi et al. (Nature Communications, 2014). The authors of the paper are Pierre Baldi, Peter Sadowski, and Daniel Whiteson. The dataset is used for searching for exotic particles in high-energy physics with deep learning.
![logo](fig/fig1.png)

### Reference:
- Paper: [Baldi, Pierre, Peter Sadowski, and Daniel Whiteson. "Searching for exotic particles in high-energy physics with deep learning." Nature communications 5, no. 1 (2014): 4308](https://scholar.google.com/scholar_lookup?arxiv_id=1402.4735)
- Dataset: [HIGGS dataset](http://archive.ics.uci.edu/dataset/280/higgs) 

## Load data:
```python	
import numpy as np
import pandas as pd

df  =  pd.read_csv("./data/HIGGS_2M_Row.csv")

y= df['label']
X = df.drop('label',axis=1)
X = X.to_numpy()
y = y.to_numpy().reshape(1,y.shape[0])
```
## Logistic Regression GPU-version
```python
from lib.logistic_gpu import logistic_regression, predict 
from sklearn.metrics import accuracy_score, f1_score
import time

# Measure the execution time of the logistic_regression function
start_time = time.time()
w = logistic_regression(X, y, learning_rate=0.2, epsilon=2e-2)
end_time = time.time()

# Print the execution time
print("Execution time:", end_time - start_time, "seconds")

# Use the trained model to make predictions on the training data
y_pred = predict(X, w)

# Calculate and print the accuracy score and F1 score
score = accuracy_score(y_pred[0], y[0])
f1Score = f1_score(y_pred[0], y[0])
print("accuracy score is", score)
print("f1_score is", f1Score)
```
### Results:
```Bash
Execution time: 9.497852802276611 seconds
accuracy score is 0.5553597223201389
f1_score is 0.6741570975215551
```

## Logistic Regression CPU-version

```python
from lib.logistic_cpu import LogisticRegression, predict 
from sklearn.metrics import accuracy_score, f1_score
import time

# Measure the execution time of the logistic_regression function
start_time = time.time()
w = LogisticRegression(X, y, lr=0.2, epsilon=2e-2)
end_time = time.time()

# Print the execution time
print("Execution time:", end_time - start_time, "seconds")

# Use the trained model to make predictions on the training data
y_pred = predict(X, w)
y_pred = y_pred.reshape(1,y_pred.shape[0])
# Calculate and print the accuracy score and F1 score
score = accuracy_score(y_pred[0], y[0])
f1Score = f1_score(y_pred[0], y[0])
print("accuracy score is", score)
print("f1_score is", f1Score)
```
### Results:
```Bash
Execution time: 19.423811674118042 seconds
accuracy score is 0.6151406924296537
f1_score is 0.6689305175558841
```
## Logistic Regression Sklearn-version

```python
import time
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, f1_score

# Create a LogisticRegression classifier with specified parameters
clf = LogisticRegression(l1_ratio=0.2, tol=2e-2)

# Start timer for model training
t1 = time.time()

# Fit the classifier to the data
clf.fit(X, y[0])

# End timer for model training
t2 = time.time()
print("The execution time: ", t2 - t1)

# Make predictions on the data
y_pred = clf.predict(X)

# Calculate accuracy score and f1 score
score = accuracy_score(y_pred, y[0])
f1Score = f1_score(y_pred, y[0])

# Print the results
print("Accuracy score: ", score)
print("F1 score: ", f1Score)
```
### Results:

```Bash
The execution time:  22.600391149520874
Accuracy score:  0.6417916791041605
F1 score:  0.6869399278895209
```