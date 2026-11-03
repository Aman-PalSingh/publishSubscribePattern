# Run and Test the Publish–Subscribe App

These commands run the Node.js Publish–Subscribe demo locally.

## 1. Install dependencies

Run once from the project root:

```bash
npm init -y
npm install express express-ws axios ws
```

## 2. Terminal 1 — Start the server

```bash
node server.js
```

Expected output:

```text
Listening on port 3001!
```

Keep this terminal running.

## 3. Terminal 2 — Subscribe to stock prices

```bash
TOPIC_ID=stock_prices node subscriber.js
```

## 4. Terminal 3 — Subscribe to news alerts

```bash
TOPIC_ID=news_alerts node subscriber.js
```

## 5. Terminal 4 — Publish stock price messages

This sends `New Stock Price` approximately once per second:

```bash
(for i in $(seq 1 10000); do sleep 1; echo New Stock Price; done) | NAME=STOCK_BROKER TOPIC_ID=stock_prices node publisher.js
```

## 6. Test news publishing

Stop the stock publisher with `Ctrl+C`, then run:

```bash
(for i in $(seq 1 10000); do sleep 1; echo Breaking News; done) | NAME=NEWS_STATION TOPIC_ID=news_alerts node publisher.js
```

## Expected behavior

The stock subscriber should display messages like:

```text
> STOCK_BROKER: New Stock Price
```

The news subscriber should display messages like:

```text
> NEWS_STATION: Breaking News
```

Messages are delivered only to subscribers connected to the matching topic.

## Notes

- Run each command in a separate terminal tab/window, except the publisher commands which use Terminal 4 one at a time.
- Stop a running publisher with `Ctrl+C`.
- The loop uses `$(seq 1 10000)` to generate valid Bash sequence values.
- The original `server.js` POST route does not send an HTTP response. Broadcasting may happen, but the publisher's Axios request can remain pending. This note does not change the original source code.
