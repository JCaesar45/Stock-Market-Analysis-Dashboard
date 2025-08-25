```markdown
# Stock Market Analysis Dashboard - Wall Street & WTC Themed

## Overview
This is a simple, interactive web-based stock market analysis prototype themed around the iconic symbols of Wall Street and the World Trade Center. It visually and functionally demonstrates how technical indicators, sentiment analysis, and support/resistance levels can be calculated and displayed for educational or prototyping purposes.

**Note:** This project is a front-end simulation with mock data and simplified calculations. It does **not** fetch real-time data or perform actual machine learning or sentiment analysis.

---

## Features
- Themed visual design inspired by Wall Street and the World Trade Center
- Calculates key technical indicators:
  - Simple Moving Averages (SMA)
  - Relative Strength Index (RSI)
  - Moving Average Convergence Divergence (MACD)
  - Bollinger Bands
  - Support and Resistance levels
- Mocks sentiment analysis with random scores
- Random detection of basic candlestick patterns
- Interactive button to generate analysis output

---

## Technologies Used
- **HTML** for structure
- **CSS** for styling and theme
- **JavaScript** for calculations and interactivity

---

## How to Run Locally

1. **Download or clone this repository** (or copy the code snippets below into your local files).

2. **Create three files:**
   
   - `index.html`
   - `styles.css`
   - `script.js`

3. **Copy the respective code snippets into these files:**

### `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Stock Market Analysis - Wall Street & WTC Theme</title>
<link rel="stylesheet" href="styles.css" />
</head>
<body>

<header>
  <h1>Work of Wall Street & WTC</h1>
</header>

<main>
  <button onclick="analyzeStock()">Run Advanced Analysis</button>
  <div id="output"></div>
</main>

<script src="script.js"></script>

</body>
</html>
``

### `styles.css`
```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
  background: linear-gradient(to bottom, #0f2027, #203a43, #2c5364);
  color: #fff;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

header h1 {
  font-family: 'Georgia', serif;
  font-size: 2.8em;
  text-align: center;
  margin-bottom: 20px;
  color: #ffd700;
  text-shadow: 2px 2px 4px #000;
  padding: 10px;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 8px;
  width: 90%;
}

button {
  padding: 12px 24px;
  font-size: 1.2em;
  background-color: #ffd700;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  transition: background-color 0.3s, transform 0.2s;
}
button:hover {
  background-color: #e6c200;
  transform: scale(1.05);
}

#output {
  margin-top: 20px;
  padding: 15px;
  width: 90%;
  max-width: 800px;
  background-color: rgba(255,255,255,0.1);
  border-radius: 10px;
  font-family: 'Courier New', monospace;
  font-size: 1.2em;
  line-height: 1.5;
  color: #fff;
  backdrop-filter: blur(4px);
  border: 1px solid #fff;
  overflow-wrap: break-word;
}
``

### `script.js`
```js
const prices = [100, 102, 105, 103, 107, 110, 108, 112, 115, 117, 120];

function calculateSMA(data, period) {
  if (data.length < period) return null;
  const sum = data.slice(-period).reduce((a, b) => a + b, 0);
  return sum / period;
}

function calculateRSI(data, period=14) {
  if (data.length < period + 1) return null;
  let gains = 0, losses = 0;
  for (let i = data.length - period; i < data.length; i++) {
    const change = data[i] - data[i - 1];
    if (change > 0) gains += change;
    else losses -= change;
  }
  const rs = gains / (losses || 1);
  const rsi = 100 - (100 / (1 + rs));
  return rsi.toFixed(2);
}

function calculateEMA(data, period) {
  if (data.length < period) return null;
  const k = 2 / (period + 1);
  let ema = data.slice(0, period).reduce((a, b) => a + b, 0) / period;
  for (let i = period; i < data.length; i++) {
    ema = data[i] * k + ema * (1 - k);
  }
  return ema;
}

function calculateMACD(data) {
  const ema12 = calculateEMA(data, 12);
  const ema26 = calculateEMA(data, 26);
  if (ema12 && ema26) {
    return ema12 - ema26;
  }
  return null;
}

function calculateBollingerBands(data, period=20, stdDevMultiplier=2) {
  if (data.length < period) return null;
  const recentData = data.slice(-period);
  const mean = recentData.reduce((a,b) => a+b, 0) / period;
  const variance = recentData.reduce((a,b) => a + Math.pow(b - mean, 2), 0) / period;
  const stdDev = Math.sqrt(variance);
  return {
    upper: mean + stdDevMultiplier * stdDev,
    middle: mean,
    lower: mean - stdDev
  };
}

function getSentiment() {
  return (Math.random() * 2 - 1).toFixed(2);
}

function detectCandlestickPattern() {
  const patterns = ['Hammer', 'Shooting Star', 'Doji', 'Engulfing', 'None'];
  return patterns[Math.floor(Math.random() * patterns.length)];
}

function analyzeStock() {
  const output = document.getElementById('output');
  let result = '';

  const sma3 = calculateSMA(prices, 3);
  const sma5 = calculateSMA(prices, 5);
  const rsi = calculateRSI(prices);
  const macd = calculateMACD(prices);
  const bollinger = calculateBollingerBands(prices);
  const sentiment = getSentiment();
  const candlestick = detectCandlestickPattern();

  result += `SMA 3: ${sma3.toFixed(2)}\n`;
  result += `SMA 5: ${sma5.toFixed(2)}\n`;
  result += `RSI: ${rsi}\n`;
  result += `MACD: ${macd ? macd.toFixed(2) : 'N/A'}\n`;
  if (bollinger) {
    result += `Bollinger Bands: Upper=${bollinger.upper.toFixed(2)}, Middle=${bollinger.middle.toFixed(2)}, Lower=${bollinger.lower.toFixed(2)}\n`;
  }
  result += `Sentiment Score: ${sentiment}\n`;
  result += `Candlestick Pattern: ${candlestick}\n`;

  const support = Math.min(...prices);
  const resistance = Math.max(...prices);
  result += `Support Level: ${support}\n`;
  result += `Resistance Level: ${resistance}\n`;

  document.getElementById('output').textContent = result;
}
``

---

## Usage
- Save the HTML, CSS, and JS files in the same directory.
- Open `index.html` in your browser.
- Click **"Run Advanced Analysis"** to generate and see the indicators.

---

## Customization
You can extend this project by:
- Integrating real data APIs (Yahoo Finance, Alpha Vantage).
- Adding more technical indicators.
- Enhancing sentiment analysis with actual NLP models.
- Visualizing data with charts (e.g., Chart.js).

---

## License
This project is for educational and prototyping purposes. Feel free to modify and expand!

---
