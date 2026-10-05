# Automation Templates for n8n

A collection of reusable n8n workflows and automation templates. This repository serves as a catalog and deployment source for streamlined automation flows.

## Workflows

| Name | File | Description |
| :--- | :--- | :--- |
| Market Advice | [workflows/market-advice.json](workflows/market-advice.json) | AI-driven day trading advisor using Gemini and TwelveData. |

## Installation

1.  Clone this repository or download the JSON file for the desired workflow.
2.  Open your [n8n](https://n8n.io/) instance.
3.  Click "Import from file" and select the downloaded JSON.
4.  Configure the nodes with your own API credentials as specified in the original workflow documentation.

## Contribution

Contributions are welcome! If you have a template, please submit a pull request with the workflow JSON file placed in the `workflows/` directory.

---

## Original Workflow Documentation (Market Advice)

This n8n workflow provides AI-driven day trading advice by analyzing real-time market data and news sentiment. When a user provides a stock ticker symbol via a chat interface, the workflow fetches technical data across multiple timeframes and the latest news, processes it through Google's Gemini AI, and returns a concise, actionable trading recommendation.

### ⚙️ How It Works
The workflow operates in two parallel branches that eventually merge for a final analysis:
1.  **Data Collection:** Fetches candlestick data (1m, 15m, 1h) and news articles.
2.  **Sentiment Analysis:** Analyzes news with Google Gemini.
3.  **Technical Analysis:** Normalizes candlestick data.
4.  **Synthesis:** AI agent synthesizes technicals and sentiment to generate actionable advice.

*(See the `workflows/market-advice.json` file for specific configuration.)*
