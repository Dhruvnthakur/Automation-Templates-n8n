# AI-Powered Day Trading Advisor (n8n Workflow)

This n8n workflow provides AI-driven day trading advice by analyzing real-time market data and news sentiment. When a user provides a stock ticker symbol via a chat interface, the workflow fetches technical data across multiple timeframes and the latest news, processes it through Google's Gemini AI, and returns a concise, actionable trading recommendation.



---

## ✨ Features

* **Multi-Timeframe Analysis:** Fetches and processes candlestick data for 1-minute, 15-minute, and 1-hour intervals to capture both short-term momentum and broader trends.
* **News Sentiment Analysis:** Gathers the latest news related to the stock and uses a Gemini model to perform a detailed sentiment analysis, generating a score and rationale.
* **AI-Powered Synthesis:** An AI agent, acting as an expert day trader, synthesizes the technical data and news sentiment to produce a unified recommendation.
* **Actionable Output:** Delivers a clear "Buy," "Sell," or "Hold" recommendation, complete with suggested **entry price**, **stop-loss**, and **target/exit price**.
* **Chat-Based Interface:** Triggered by a simple message, making it easy to integrate with various chat platforms.

---

## ⚙️ How It Works

The workflow operates in two parallel branches that eventually merge for a final analysis.

### 1. Data Collection
When a user sends a chat message with a stock ticker (e.g., `AAPL`), the workflow triggers and runs two main data-gathering processes simultaneously:

* **Technical Data:** Three `HTTP Request` nodes call the **Twelve Data API** to get time-series candlestick data for the ticker at `1min`, `15min`, and `1h` intervals.
* **News Data:** A separate `HTTP Request` node calls the **Newsdata.io API** to retrieve recent news articles related to the ticker.

### 2. Sentiment Analysis Branch
* The news articles are passed to a **Google Gemini** model.
* The model is prompted to act as a financial sentiment analyzer, evaluating the news to determine if the short-term sentiment is "Positive," "Neutral," or "Negative" and assigning a sentiment score from -1.0 to +1.0.
* The output is a structured JSON object containing the sentiment analysis.

### 3. Technical Analysis Branch
* The three sets of candlestick data (`1m`, `15m`, `1h`) are combined using a `Merge` node.
* A `Code` node then runs JavaScript to normalize the data: it cleans up the format, converts data types (e.g., string to float), and sorts the candles by timestamp. This ensures the data is clean and structured for the AI agent.

### 4. Final Synthesis & Recommendation
* The cleaned technical data and the JSON sentiment analysis are merged.
* This combined dataset is passed to a final **AI Agent** powered by **Google Gemini 2.5 Pro**.
* The agent is given a specific prompt to act as an expert day trader. It analyzes all the provided information—price action across all timeframes and news sentiment—to generate a final, unified trading recommendation in a precise format.

---

## 🔧 Setup & Configuration

To use this workflow, you'll need to configure the following:

1.  **API Credentials:** The current workflow has hardcoded API keys, which is **not recommended for production**. You should replace them by setting up credentials within n8n:
    * **Twelve Data API Key:** Used in the `HTTP 1min`, `HTTP 15min`, and `HTTP 1hr` nodes. Get your key from [twelvedata.com](https://twelvedata.com/).
    * **Newsdata.io API Key:** Used in the `HTTP Request` node for news. Get your key from [newsdata.io](https://newsdata.io/).
    * **Google Gemini API Key:** Create credentials in n8n for the `Google Gemini` nodes. Get your key from [Google AI Studio](https://aistudio.google.com/app/apikey).

2.  **Chat Trigger:** The `When chat message received` node is a LangChain trigger. You need to connect it to your desired chat front-end (e.g., the n8n chat, a web application, Discord, etc.) to send and receive messages.

---

## 🚀 Usage

1.  Activate the workflow in your n8n instance.
2.  Open the chat interface you configured for the Chat Trigger.
3.  Send a message containing only the stock ticker symbol you want to analyze (e.g., `TSLA`, `GOOGL`, `MSFT`).
4.  The workflow will execute and return the AI-generated trading advice directly in the chat.

### Example Interaction

**User Input:**

NVDA

**Workflow Output:**

Technical Recommendation: BUY

Entry Price: 125.50

Stop-Loss: 123.00

Target/Exit Price: 129.00


