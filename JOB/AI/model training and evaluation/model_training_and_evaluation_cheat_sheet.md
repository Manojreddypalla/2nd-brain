# Model Training and Evaluation Cheat Sheet

## I. Model Training Steps

1.  **Data Collection:** Gather relevant data.
2.  **Data Preprocessing:** Clean, handle missing values, and perform feature engineering.
3.  **Data Splitting:**
    *   **Training Set:** For training the model.
    *   **Validation Set:** For hyperparameter tuning.
    *   **Test Set:** For final, unbiased evaluation.
4.  **Model Selection:** Choose an appropriate algorithm.
5.  **Model Training:** Fit the model to the training data.
6.  **Hyperparameter Tuning:** Optimize model settings using the validation set (e.g., Grid Search, Random Search).
7.  **Model Evaluation:** Assess performance on the validation set.
8.  **Final Model Training:** Retrain the best model on the combined training and validation data.
9.  **Model Testing:** Evaluate the final model on the unseen test set.

## II. Key Concepts in Training

*   **Overfitting:** Model learns training data too well, including noise, and performs poorly on new data.
*   **Underfitting:** Model is too simple and fails to capture underlying patterns.
*   **Bias-Variance Tradeoff:** The balance between a model's systematic error (bias) and its sensitivity to changes in the training data (variance).
*   **Regularization:** Techniques (e.g., L1, L2) to prevent overfitting.
*   **Cross-Validation (K-Fold):** A robust method for estimating model performance by splitting data into 'k' folds and training/validating 'k' times.

## III. Model Evaluation Metrics

### A. Classification Metrics

*   **Confusion Matrix:**
    *   **True Positives (TP):** Correctly predicted positives.
    *   **True Negatives (TN):** Correctly predicted negatives.
    *   **False Positives (FP):** Incorrectly predicted positives (Type I Error).
    *   **False Negatives (FN):** Incorrectly predicted negatives (Type II Error).
*   **Accuracy:** `(TP + TN) / (TP + TN + FP + FN)` - Good for balanced datasets.
*   **Precision:** `TP / (TP + FP)` - Important when the cost of false positives is high.
*   **Recall (Sensitivity):** `TP / (TP + FN)` - Important when the cost of false negatives is high.
*   **F1-Score:** `2 * (Precision * Recall) / (Precision + Recall)` - Harmonic mean of Precision and Recall, good for imbalanced datasets.
*   **ROC Curve & AUC:**
    *   **ROC Curve:** Plots True Positive Rate vs. False Positive Rate.
    *   **AUC (Area Under the Curve):** A single number summary of the ROC curve. Higher is better.

### B. Regression Metrics

*   **Mean Absolute Error (MAE):** Average of the absolute differences between predicted and actual values.
*   **Mean Squared Error (MSE):** Average of the squared differences. Penalizes larger errors more.
*   **Root Mean Squared Error (RMSE):** Square root of MSE. In the same units as the target variable.
*   **R-squared (R²):** Proportion of the variance in the dependent variable that is predictable from the independent variables.
