
# 

1. **`pandas`**
    - **What it is:** The most popular data manipulation and analysis library in Python. It provides a powerful data structure called a DataFrame, which is essentially a smart table or spreadsheet.
    - **Its Role in Your Project:** You will use it for everything data-related: reading the CSV files from `yfinance` and `newsapi-python`, cleaning the data, calculating technical indicators (like moving averages), and merging the stock and news data together before training.
2. **`yfinance`**
    - **What it is:** A reliable and straightforward library for downloading historical market data from Yahoo! Finance.
    - **Its Role in Your Project:** This is your source for all quantitative stock data—opening prices, closing prices, volume, etc.
3. **`newsapi-python`**
    - **What it is:** A Python wrapper for [NewsAPI.org](http://newsapi.org/), making it easy to search for and retrieve live news articles and headlines from sources across the web.
    - **Its Role in Your Project:** This is your source for the qualitative data. You'll use it to fetch the financial news headlines that will be fed into the LLM.

### Natural Language Processing (NLP) and Machine Learning (ML)

1. **`sentence-transformers`**
    - **What it is:** A library built on top of major transformer frameworks (like PyTorch and TensorFlow) that simplifies the process of creating high-quality embeddings (numerical representations) for sentences and text.
    - **Its Role in Your Project:** This is the core of your LLM integration. You will use a pre-trained model from this library to convert each news headline into a meaningful vector of numbers. This is a crucial step that turns unstructured text into something a machine learning model can understand.
2. **`scikit-learn`**
    - **What it is:** The fundamental library for traditional machine learning in Python. It provides simple and efficient tools for data mining and data analysis.
    - **Its Role in Your Project:** You will use it to:
        - Split your final dataset into training and testing sets (`train_test_split`).
        - Train your predictive model (`RandomForestClassifier`).
        - Evaluate your model's performance (`accuracy_score`).
3. **`joblib`**
    - **What it is:** A set of tools to provide lightweight pipelining in Python. It's particularly efficient for saving and loading Python objects that have large NumPy arrays, which is exactly what `scikit-learn` models are.
    - **Its Role in Your Project:** After you train your `RandomForestClassifier` model, you don't want to retrain it every time your app starts. `joblib` allows you to save the trained model to a file and load it back into your Flask app instantly.

### Web Backend (API)

1. **`Flask`**
    - **What it is:** A lightweight "microframework" for building web applications and APIs in Python. It's simple to get started with but powerful enough for complex applications.
    - **Its Role in Your Project:** It acts as the brain of your web application. You'll use Flask to create a simple API server that listens for requests from your frontend website, runs your prediction logic using the saved model, and sends the result back.
2. **`Flask-CORS`**
    - **What it is:** An extension for Flask that handles Cross-Origin Resource Sharing (CORS).
    - **Its Role in Your Project:** For security reasons, web browsers block web pages from making requests to a different domain than the one they were served from. Since your HTML/JS frontend and your Python backend will be running in different places (even on your local machine), you need this library to tell the browser, "It's okay, my frontend is allowed to talk to my backend."