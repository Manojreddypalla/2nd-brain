## ## Project: AI Stock Predictor - Revision Notes

You have successfully completed the data collection and model training phases of the project. This involved creating two core Python scripts.

---

### ### 1. `collect_data.py` - The Data Collector

- **Goal:** To automatically download historical stock prices and relevant news headlines, then save them as clean CSV files.
- **Key Libraries Used:**
    - **`yfinance`**: Connects to Yahoo! Finance to get historical stock data.
    - **`newsapi-python`**: Connects to the NewsAPI service to get news articles.
    - **`pandas`**: Used to organize the downloaded data into tables (DataFrames) and save them as CSVs.
    - **`python-dotenv`**: Loads secret keys (like your `NEWS_API_KEY`) from a `.env` file to keep them out of your code.
- **Workflow Summary:**
    1. Loads configuration (API key, ticker) from the `.env` file.
    2. Calculates a date range for the past **29 days** (to respect the NewsAPI free plan limit).
    3. Calls a function (`get_stock_data`) to download price history for the specified ticker.
    4. Calls a second function (`get_news_data`) to search for news headlines related to the company.
    5. Saves both datasets into two separate files: `TICKER_stock_data.csv` and `TICKER_news_headlines.csv`.
- **Key Concept:** You configured **`.gitignore`** to prevent your secret `.env` file and your generated `.csv` data files from being committed to your repository. This is a crucial professional practice.

---

### ### 2. `train_model.py` - The AI Brain

- **Goal:** To load the collected data, transform it into features the model can understand, train a predictive model, and save the trained "brain" to a file.
- **Key Libraries Used:**
    - **`pandas`**: Loads and manipulates the data from your CSV files.
    - **`sentence-transformers`**: Provides the "mini LLM" (`all-MiniLM-L6-v2`) to convert news headlines into numerical **embeddings**.
    - **`scikit-learn`**: The main machine learning toolbox. We used it for:
        - `RandomForestClassifier`: The model itself, which acts like a "committee of experts."
        - `train_test_split`: To separate data for training and testing.
        - `accuracy_score`: To measure the model's performance.
    - **`joblib`**: A simple tool to save your final, trained model object to a single file.
- **Workflow Summary:**
    1. Loads the two CSV files using **pandas**.
    2. **Performs Feature Engineering:**
        - Calculates technical indicators from the stock data (daily return, moving averages).
        - Uses the **SentenceTransformer** to convert every news headline into a numerical embedding.
        - Averages the embeddings for each day to get a single daily "news sentiment" vector.
    3. **Merges** the stock and news data into a single master DataFrame.
    4. **Creates the Target:** Defines what to predict—whether the next day's price will be higher (1) or lower (0).
    5. **Trains the Model:** Splits the data chronologically (**`shuffle=False`** is critical) and trains the `RandomForestClassifier`.
    6. **Saves the Model:** The final trained model is saved as `stock_predictor_model.joblib`.
- **Key Concept:** You used a **RandomForestClassifier**, which is powerful because it combines many decision-makers to make a final prediction. You also learned to split time-series data without shuffling to prevent the model from "cheating" by looking into the future.