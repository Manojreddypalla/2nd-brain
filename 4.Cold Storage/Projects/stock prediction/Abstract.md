This project focuses on predicting stock market movement by integrating historical stock
data with sentiment analysis of financial news. Stock data is collected using the Yahoo
Finance API, while financial news headlines are preprocessed and analyzed using Natural
Language Processing (NLP) techniques such as VADER and FinBERT to derive sentiment
scores. These sentiment features are combined with technical indicators like daily returns
and moving averages to build predictive models. Both traditional machine learning
classifiers (Logistic Regression, Random Forest) and deep learning models (LSTM) are
implemented and compared. Experimental results indicate that sentiment-enriched models
achieve higher prediction accuracy than those relying solely on price-based features. This
demonstrates the importance of combining quantitative and qualitative data for effective
financial forecasting.