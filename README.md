
# FastAPI Interactive Brokers Integration

This project integrates FastAPI with the Interactive Brokers (IB) API to handle webhook requests and place buy/sell orders asynchronously.

## Prerequisites
- Python 3.7+
- `ib_insync` library
- `fastapi` library
- `uvicorn` server
- `ibapi` library (for direct IB API access)
- Ngrok (to expose your local server to the internet)
- An active IB Gateway or Trader Workstation (TWS) running and configured for API access.

## Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   # Activate the environment (Windows)
   venv\Scripts\activate
   # Activate the environment (macOS/Linux)
   source venv/bin/activate
   ```

3. Install the required dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   Ensure `requirements.txt` includes:
   ```
   fastapi
   uvicorn
   ib_insync
   ibapi
   ```

4. Install the `ibapi` library separately (if not included in your `requirements.txt`):
   ```bash
   pip install ibapi
   ```

5. Install Ngrok from the [official website](https://ngrok.com/download) and set it up.

## Setting up Trader Workstation (TWS)

Ensure that your IB Gateway or TWS is running and accessible on `localhost` at port `7497`:
- Download and install TWS from the [official website](https://www.interactivebrokers.com/en/index.php?f=16040).
- Open IB Gateway/TWS and configure the API settings:
  - Check "Enable ActiveX and Socket Clients"
  - Uncheck "Read-Only API"
  - Set the port to `7497` for paper trading.

## Running the Application

You **must** run the FastAPI server on port 80, and expose it to the internet using Ngrok.

### Start the FastAPI Server
```bash
uvicorn main:app --reload --port 80
```
- Replace `main` with your actual script name if it's different.

### Start Ngrok
In a separate terminal window, run:
```bash
ngrok http 80
```
- Ngrok will provide a URL (e.g., `http://abc123.ngrok.io`) that exposes your local server to the internet.

## Webhook Endpoint

The server exposes a `/webhook` endpoint that accepts POST requests with the following JSON payload:

### JSON Payload Example
```json
{
    "action": "buy",
    "ticker": "AAPL",
    "quantity": 10
}
```
- `action`: `buy` or `sell`
- `ticker`: The stock symbol (e.g., `AAPL` for Apple)
- `quantity`: The number of shares to trade

### Sample Request
You can test the endpoint using `curl` or tools like Postman:
```bash
curl -X POST "http://127.0.0.1:80/webhook" -H "Content-Type: application/json" -d '{"action": "buy", "ticker": "AAPL", "quantity": 10}'
```
Or using the Ngrok URL:
```bash
curl -X POST "http://abc123.ngrok.io/webhook" -H "Content-Type: application/json" -d '{"action": "buy", "ticker": "AAPL", "quantity": 10}'
```

## Debugging & Troubleshooting

- Check the console output for connection and order placement logs.
- Ensure that your IB Gateway/TWS is running and the port is accessible.
- Ensure Ngrok is running and you’re using the correct public URL for external requests.

## Shutting Down

To stop the server, press `CTRL + C` in the terminal where `uvicorn` and `ngrok` are running.


