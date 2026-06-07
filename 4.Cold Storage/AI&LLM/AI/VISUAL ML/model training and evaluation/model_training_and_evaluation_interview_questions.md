# Model Training and Evaluation Interview Questions

### 1. What is the difference between a training, validation, and testing set?
**Answer:**
*   **Training set:** Used to train the model.
*   **Validation set:** Used to tune the model's hyperparameters and prevent overfitting.
*   **Testing set:** Used for the final, unbiased evaluation of the model's performance on unseen data.

### 2. What is overfitting and how can you avoid it?
**Answer:** Overfitting is when a model learns the training data too well, including the noise, and performs poorly on new, unseen data. You can avoid it with techniques like:
*   **Cross-validation:** To get a more robust estimate of model performance.
*   **Regularization (L1/L2):** To penalize complex models.
*   **Early stopping:** To stop training when performance on the validation set starts to degrade.
*   **Getting more data:** To help the model generalize better.

### 3. What is the bias-variance tradeoff?
**Answer:** It's a fundamental concept in machine learning.
*   **Bias** is the error from erroneous assumptions in the learning algorithm. High bias can cause an algorithm to miss the relevant relations between features and target outputs (underfitting).
*   **Variance** is the error from sensitivity to small fluctuations in the training set. High variance can cause an algorithm to model the random noise in the training data, rather than the intended outputs (overfitting).
The tradeoff is that decreasing one tends to increase the other. The goal is to find a balance between the two.

### 4. Why is accuracy not always a good metric for classification problems?
**Answer:** Accuracy can be misleading, especially for imbalanced datasets. For example, if you have a dataset with 95% of one class and 5% of another, a model that always predicts the majority class will have 95% accuracy but will be useless for predicting the minority class. In such cases, metrics like Precision, Recall, and F1-score are more informative.

### 5. What is the difference between Precision and Recall?
**Answer:**
*   **Precision:** Of all the positive predictions, how many were actually positive? `TP / (TP + FP)`. Use when the cost of false positives is high (e.g., spam detection).
*   **Recall:** Of all the actual positives, how many did we correctly predict? `TP / (TP + FN)`. Use when the cost of false negatives is high (e.g., medical diagnosis).

### 6. What is the F1-score?
**Answer:** The F1-score is the harmonic mean of Precision and Recall. It's a good metric to use when you want to find a balance between Precision and Recall, especially when you have an imbalanced dataset.

### 7. What is a confusion matrix?
**Answer:** A confusion matrix is a table that is used to evaluate the performance of a classification model. It shows the number of true positives, true negatives, false positives, and false negatives.

### 8. What are some common evaluation metrics for regression problems?
**Answer:**
*   **Mean Absolute Error (MAE):** The average of the absolute errors.
*   **Mean Squared Error (MSE):** The average of the squared errors. It penalizes larger errors more than MAE.
*   **Root Mean Squared Error (RMSE):** The square root of the MSE. It's in the same units as the target variable.
*   **R-squared (R²):** The proportion of the variance in the dependent variable that is predictable from the independent variables.

### 9. What is cross-validation?
**Answer:** Cross-validation is a technique for assessing how the results of a statistical analysis will generalize to an independent data set. A common method is k-fold cross-validation, where the data is split into k subsets, and the model is trained and evaluated k times, using a different subset for evaluation each time. This gives a more robust estimate of the model's performance.
