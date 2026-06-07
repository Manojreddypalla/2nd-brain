# Full Project Guide: AI Stock Predictor with Streamlit

This guide contains all the steps, commands, and code needed to build your application from start to finish. Follow the days in order.

### **Day 1: Setup and Data Collection**

**Goal:** Create the project structure and scripts to download stock and news data.

**Step 1: Project Setup (Terminal)**
Open your terminal, create a project folder, and set up a Python virtual environment.

```
mkdir stock_pr
edictor
cd stock_predictor
python -m venv venv
# On Windows: venv\Scripts\activate
# On macOS/Linux: source venv/bin/activate
pip install pandas yfinance newsapi-python

```

**Step 2: Get a News API Key**
You need a free API key to fetch news headlines.

1. Go to [newsapi.org](https://newsapi.org/) and click "Get API Key".
2. Sign up for a free developer account.
3. Copy your API key. You will need it in the next step.

**Step 3: Create the Data Collection Script**
Create a new file named `collect_data.py` and paste the following code into it.

```
# collect_data.py
import yfinance as yf
import pandas as pd
from newsapi import NewsApiClient
import os
from datetime import datetime, timedelta

# --- CONFIGURATION ---
TICKER = 'AAPL' # Example: Apple Inc.
STOCK_FILENAME = f'{TICKER}_stock_data.csv'
NEWS_FILENAME = f'{TICKER}_news_headlines.csv'
# IMPORTANT: Replace with your actual NewsAPI key
NEWS_API_KEY = 'YOUR_NEWS_API_KEY_HERE'

def get_stock_data(ticker, start_date, end_date):
    """Downloads historical stock data and saves it to a CSV."""
    print(f"Downloading {ticker} stock data...")
    stock = yf.Ticker(ticker)
    df = stock.history(start=start_date, end=end_date)
    df.to_csv(STOCK_FILENAME)
    print(f"Stock data saved to {STOCK_FILENAME}")

def get_news_data(query, start_date, end_date):
    """Fetches news headlines and saves them to a CSV."""
    if NEWS_API_KEY == 'YOUR_NEWS_API_KEY_HERE':
        print("Error: Please replace 'YOUR_NEWS_API_KEY_HERE' with your actual NewsAPI key.")
        return

    print(f"Fetching news for '{query}'...")
    newsapi = NewsApiClient(api_key=NEWS_API_KEY)

    all_articles = newsapi.get_everything(
        q=query,
        language='en',
        sort_by='publishedAt',
        from_param=start_date,
        to=end_date
    )

    # Process and save headlines
    articles_data = []
    for article in all_articles['articles']:
        articles_data.append({
            'publishedAt': article['publishedAt'],
            'title': article['title']
        })

    df = pd.DataFrame(articles_data)
    # Ensure 'publishedAt' is just the date part for easier merging later
    df['publishedAt'] = pd.to_datetime(df['publishedAt']).dt.date
    df.to_csv(NEWS_FILENAME, index=False)
    print(f"News headlines saved to {NEWS_FILENAME}")

if __name__ == '__main__':
    # We'll download data for the past year
    end = datetime.now()
    start = end - timedelta(days=365)
    start_str = start.strftime('%Y-%m-%d')
    end_str = end.strftime('%Y-%m-%d')

    get_stock_data(TICKER, start_str, end_str)
    # For news, we can use a broader query like the company name
    get_news_data('Apple', start_str, end_str)

```

**Step 4: Run the Script (Terminal)**
Before running, make sure you've added your API key to the file.

```
python collect_data.py

```

You should now have two CSV files in your folder: `AAPL_stock_data.csv` and `AAPL_news_headlines.csv`.

### **Day 2 & 3: Feature Engineering and Model Training**

**Goal:** Process the raw data into features, merge them, and train a predictive model.

**Step 1: Install Libraries (Terminal)**

```
pip install sentence-transformers scikit-learn joblib

```

**Step 2: Create the Training Script**
Create a new file named `train_model.py` and paste the following code. This single script handles feature creation, data merging, and model training.

```
# train_model.py
import pandas as pd
from sentence_transformers import SentenceTransformer
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import joblib
import numpy as np

# --- CONFIGURATION ---
TICKER = 'AAPL'
STOCK_FILENAME = f'{TICKER}_stock_data.csv'
NEWS_FILENAME = f'{TICKER}_news_headlines.csv'
MODEL_FILENAME = 'stock_predictor_model.joblib'

def train():
    """Main function to run the training pipeline."""
    # 1. Load Data
    print("Loading data...")
    stock_df = pd.read_csv(STOCK_FILENAME, parse_dates=['Date'])
    news_df = pd.read_csv(NEWS_FILENAME, parse_dates=['publishedAt'])

    # 2. Feature Engineering - Technical Indicators
    print("Calculating technical indicators...")
    stock_df.set_index('Date', inplace=True)
    stock_df['daily_return'] = stock_df['Close'].pct_change()
    stock_df['ma_7'] = stock_df['Close'].rolling(window=7).mean()
    stock_df['ma_21'] = stock_df['Close'].rolling(window=21).mean()
    stock_df.dropna(inplace=True)

    # 3. Feature Engineering - LLM Embeddings for News
    print("Generating LLM embeddings for news headlines...")
    model = SentenceTransformer('all-MiniLM-L6-v2')

    # Group headlines by date and average their embeddings
    news_df['embedding'] = list(model.encode(news_df['title'].astype(str).tolist(), show_progress_bar=True))
    daily_embeddings = news_df.groupby('publishedAt')['embedding'].apply(np.mean).reset_index()
    daily_embeddings.rename(columns={'publishedAt': 'Date'}, inplace=True)

    # 4. Merge Data
    print("Merging stock data and news embeddings...")
    final_df = pd.merge(stock_df, daily_embeddings, on='Date', how='inner')

    # 5. Create Target Variable
    # Predict if the next day's closing price will be higher (1) or lower (0)
    final_df['target'] = (final_df['Close'].shift(-1) > final_df['Close']).astype(int)
    final_df.dropna(inplace=True) # Drop the last row with NaN target

    # 6. Prepare Data for Model
    print("Preparing data for training...")
    # The embeddings are lists; we need to split them into separate columns
    embedding_features = pd.DataFrame(final_df['embedding'].tolist(), index=final_df.index)

    features = final_df[['daily_return', 'ma_7', 'ma_21']]
    X = pd.concat([features, embedding_features], axis=1)
    y = final_df['target']

    # Align X and y after potential drops
    y = y.loc[X.index]

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42, shuffle=False)

    # 7. Train Model
    print("Training RandomForestClassifier...")
    rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
    rf_model.fit(X_train, y_train)

    # 8. Evaluate Model
    predictions = rf_model.predict(X_test)
    accuracy = accuracy_score(y_test, predictions)
    print(f"Model Accuracy on Test Set: {accuracy:.2f}")

    # 9. Save Model
    joblib.dump(rf_model, MODEL_FILENAME)
    print(f"Model saved to {MODEL_FILENAME}")

if __name__ == '__main__':
    train()

```

**Step 3: Run the Training Script (Terminal)**
This might take a few minutes, especially the first time it downloads the sentence transformer model.

```
python train_model.py

```

After it finishes, you will have a `stock_predictor_model.joblib` file in your folder.

### **Day 4, 5, & 6: Build the Streamlit Web App**

**Goal:** Create a user-friendly web interface for your model.

**Step 1: Install Streamlit (Terminal)**

```
pip install streamlit

```

**Step 2: Create the Streamlit App Script**
Create a new file named `app.py`. This is your main application file. Paste the following code into it.

```
# app.py
import streamlit as st
import pandas as pd
import joblib
import yfinance as yf
from newsapi import NewsApiClient
from sentence_transformers import SentenceTransformer
import numpy as np
from datetime import datetime, timedelta

# --- CONFIGURATION & MODEL LOADING ---
TICKER = 'AAPL' # Default ticker
NEWS_API_KEY = 'YOUR_NEWS_API_KEY_HERE' # IMPORTANT: Replace with your key

# Load the pre-trained model and the sentence transformer
# Use st.cache_resource to load these only once
@st.cache_resource
def load_models():
    predictor_model = joblib.load('stock_predictor_model.joblib')
    embedding_model = SentenceTransformer('all-MiniLM-L6-v2')
    return predictor_model, embedding_model

predictor_model, embedding_model = load_models()

# --- HELPER FUNCTIONS ---
def get_live_data(ticker):
    """Fetches the latest stock data and news headlines for a given ticker."""
    # Stock Data (get last 60 days to calculate indicators)
    end_date = datetime.now()
    start_date = end_date - timedelta(days=60)
    stock_df = yf.Ticker(ticker).history(start=start_date, end=end_date)

    # News Data
    if NEWS_API_KEY == 'YOUR_NEWS_API_KEY_HERE':
        st.error("NewsAPI key not configured. Please add it to the script.")
        return None, None

    newsapi = NewsApiClient(api_key=NEWS_API_KEY)
    # A broad query is often better, e.g., company name
    company_info = yf.Ticker(ticker).info
    company_name = company_info.get('longName', ticker)

    end_date_str = end_date.strftime('%Y-%m-%d')
    start_date_str = (end_date - timedelta(days=2)).strftime('%Y-%m-%d') # Look at news from last 2 days

    headlines = newsapi.get_everything(
        q=company_name,
        language='en',
        sort_by='publishedAt',
        from_param=start_date_str,
        to=end_date_str,
        page_size=10 # Get top 10 recent headlines
    )

    headline_titles = [article['title'] for article in headlines['articles']]

    return stock_df.tail(1), headline_titles # Return latest stock data and headlines

def create_feature_vector(latest_stock_data, headlines):
    """Creates a feature vector from live data for prediction."""
    # 1. Technical Indicators
    daily_return = latest_stock_data['Close'].pct_change().iloc[-1]
    ma_7 = latest_stock_data['Close'].rolling(window=7).mean().iloc[-1]
    ma_21 = latest_stock_data['Close'].rolling(window=21).mean().iloc[-1]

    # 2. News Embeddings
    if headlines:
        headline_embeddings = embedding_model.encode(headlines)
        avg_embedding = np.mean(headline_embeddings, axis=0)
    else:
        # If no news, use a zero vector of the correct shape (384 for all-MiniLM-L6-v2)
        avg_embedding = np.zeros(384)

    # 3. Combine features
    technical_features = np.array([daily_return, ma_7, ma_21])
    feature_vector = np.concatenate([technical_features, avg_embedding]).reshape(1, -1)

    # Handle NaNs from indicators if there's not enough data
    feature_vector = np.nan_to_num(feature_vector)

    return feature_vector

# --- STREAMLIT UI ---
st.set_page_config(page_title="AI Stock Predictor", layout="wide")

st.title("📈 AI-Powered Stock Movement Predictor")
st.write("This app uses a machine learning model to predict the next day's stock movement (Up or Down) based on historical price data and recent financial news sentiment.")

# --- User Input ---
ticker_input = st.text_input("Enter a Stock Ticker (e.g., AAPL, GOOGL, MSFT)", "AAPL").upper()

if st.button("Predict"):
    if not ticker_input:
        st.warning("Please enter a stock ticker.")
    else:
        with st.spinner(f"Analyzing {ticker_input}... This may take a moment."):
            try:
                # 1. Fetch Data
                latest_stock_data, headlines = get_live_data(ticker_input)

                if latest_stock_data is None:
                    # Error handled inside the function
                    pass
                elif latest_stock_data.empty:
                    st.error(f"Could not fetch stock data for {ticker_input}. Please check the ticker.")
                else:
                    # 2. Create Feature Vector
                    # We need more than 1 day of data to calculate indicators, so let's refetch a larger chunk
                    full_stock_history = yf.Ticker(ticker_input).history(period="2mo")
                    feature_vector = create_feature_vector(full_stock_history, headlines)

                    # 3. Make Prediction
                    prediction = predictor_model.predict(feature_vector)
                    prediction_proba = predictor_model.predict_proba(feature_vector)

                    # 4. Display Results
                    st.subheader(f"Prediction for {ticker_input}")
                    if prediction[0] == 1:
                        st.success("Prediction: Movement UP 🟢")
                    else:
                        st.error("Prediction: Movement DOWN 🔴")

                    st.write(f"Confidence: {prediction_proba[0][prediction[0]]*100:.2f}%")

                    # Display recent news headlines used for the prediction
                    st.subheader("Recent News Headlines Considered:")
                    if headlines:
                        for h in headlines:
                            st.write(f"- {h}")
                    else:
                        st.write("No recent news headlines found.")

                    # Display price chart
                    st.subheader("Recent Stock Performance")
                    st.line_chart(full_stock_history['Close'])

            except Exception as e:
                st.error(f"An error occurred: {e}")

st.sidebar.header("About")
st.sidebar.info("This project integrates historical stock data with LLM-based sentiment analysis of financial news to predict market movements. It demonstrates the value of combining quantitative and qualitative data for financial forecasting.")

```

**Step 3: Run the Streamlit App (Terminal)**
Make sure you have added your NewsAPI key to `app.py`.

```
streamlit run app.py

```

Your web browser should open with your running application!

### **Day 7: Documentation and Deployment**

**Goal:** Finalize your project with a good description and prepare it for sharing.

**Step 1: Create a `README.md` file**
Create a file named `README.md` in your project folder and add the following content.

```
# AI Stock Predictor

This project uses a Random Forest machine learning model to predict the next day's stock price movement (Up or Down). It enriches traditional technical indicators (like moving averages) with sentiment analysis from financial news headlines, processed into embeddings by a small Language Model.

## Features
- **Technical Analysis:** Uses daily returns and moving averages.
- **Sentiment Analysis:** Converts news headlines into numerical embeddings using `sentence-transformers`.
- **Web Interface:** A user-friendly UI built with Streamlit to get live predictions.

## How to Run
1. **Clone the repository:**
   ```bash
   git clone <your-repo-url>
   cd stock_predictor

```

1. **Set up the environment:**
    
    ```
    python -m venv venv
    source venv/bin/activate  # macOS/Linux
    # venv\Scripts\activate  # Windows
    pip install -r requirements.txt
    
    ```
    
2. **Add your API Key:**
    - Get a free API key from [newsapi.org](https://newsapi.org/).
    - Open `collect_data.py` and `app.py` and replace `'YOUR_NEWS_API_KEY_HERE'` with your key.
3. **Train the model:**
    
    ```
    python train_model.py
    
    ```
    
4. **Run the Streamlit app:**
    
    ```
    streamlit run app.py
    
    ```
    

```

**Step 2: Generate `requirements.txt` (Terminal)**
This file lists all the dependencies needed to run your project.

```bash
pip freeze > requirements.txt

```

**Step 3: (Optional) Deploy to the Web**
The easiest way to share your app is with Streamlit Community Cloud.

1. Create a GitHub account if you don't have one.
2. Create a new repository and upload your project files (`app.py`, `train_model.py`, `collect_data.py`, `requirements.txt`, and your trained `.joblib` model).
3. Go to [share.streamlit.io](https://share.streamlit.io/), sign up with your GitHub account.
4. Click "New app", select your repository, and deploy.

Your project is now complete and live on the web!