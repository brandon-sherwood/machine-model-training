
# Machine Model Training: CGM Meal Classification

## Overview

This repository documents a supervised machine learning project completed for CSE 572: Data Mining as part of my Master of Computer Science program at Arizona State University.

The goal was to classify continuous glucose monitor (CGM) time-series data as Meal or No-Meal periods. I used Python to preprocess CGM and insulin pump records, extract numerical features from glucose readings, and train a Decision Tree classification model.

## Dataset and Preprocessing

I worked with CGM and insulin pump data from two patients. The insulin pump records provided meal events through nonzero carbohydrate entries, while the CGM records contained glucose measurements collected at five-minute intervals.

I combined date and time values into timestamps, sorted the records chronologically, and identified meal events with sufficient separation from subsequent meals. I then extracted 30-reading Meal windows beginning 30 minutes before each meal and extending approximately two hours afterward. No-Meal windows contained 24 readings from two-hour periods beginning at least two hours after a meal.

Windows with missing glucose values or an incorrect number of readings were excluded to keep the input data consistent.

## Feature Engineering

Rather than training the classifier on the raw glucose measurements, I represented each observation window using numerical features that described the shape of the glucose response.

### Time-Domain Features

- Time to peak glucose concentration
- Normalized difference between the starting and peak glucose values
- Maximum first difference between consecutive measurements
- Maximum second difference, representing changes in successive glucose differences

### Frequency-Domain Features

I used NumPy's Fast Fourier Transform (`np.fft.fft`) to extract information about glucose fluctuations in the frequency domain. After excluding the DC component and negative frequencies, I identified the two strongest positive-frequency components and calculated their magnitudes and frequencies.

This produced eight candidate features in total. For the final classifier, I excluded the two FFT frequency values and retained six features.

## Supervised Machine Learning

### Decision Tree Classification

I trained a `DecisionTreeClassifier` using scikit-learn to distinguish Meal (`1`) and No-Meal (`0`) windows. I split the labeled feature dataset into 80% training and 20% testing data, using stratified sampling to preserve the class proportions and `random_state=42` for reproducibility.

I evaluated the model on the held-out test set using accuracy and F1 score. After evaluation, I retrained the Decision Tree using the complete labeled feature dataset and saved it with Python's `pickle` module.

A separate testing script loaded the saved classifier, applied the same feature extraction and selection steps to new CGM windows, and exported the predicted labels to a CSV file.

### Evaluation and Results

Accuracy measures the proportion of correctly classified observations, while F1 score balances precision and recall for the Meal class.

*Numerical performance results and visualizations may be added later.*

## Technologies Used

- **Python:** Data processing and model development
- **pandas:** Timestamp handling and preprocessing
- **NumPy:** Numerical feature extraction and FFT
- **scikit-learn:** Data splitting, Decision Tree classification, and evaluation
- **pickle:** Saving and loading the trained model

## Academic Integrity

This repository documents a project completed for CSE 572: Data Mining at Arizona State University. Original assignment solutions, course datasets, trained models, and submission files are not publicly distributed. The purpose of this repository is to summarize the methodology and skills demonstrated by the project.
