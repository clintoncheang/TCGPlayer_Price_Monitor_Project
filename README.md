# 🎴 TCGPlayer_Price_Monitor_Project


A Python-based price monitoring tool built in **Jupyter Notebook** that tracks selected products on TCGplayer and sends **Discord notifications** when a listing falls below a dynamically calculated target price.

Instead of using a fixed price threshold, the monitor retrieves the current **TCGplayer Market Price** and calculates the target as a percentage of that value. By default, an alert is triggered when the lowest available listing, including shipping, falls below **70% of the current Market Price**.

> **Project Status:** 🚧 This is an ongoing personal project and will continue to be improved as I experiment with new features and monitoring strategies. For true 24/7 monitoring, the program needs to run on a device or environment that stays online, such as a cloud server, Raspberry Pi, or an older laptop dedicated to running the monitor.

---

## 💡 Why I Built This

Trading card prices can change quickly, and manually checking the same cards throughout the day is not exactly an efficient use of time.

I built this project to automate that process. Rather than simply looking for cards below a fixed dollar amount, the monitor compares current listings against the card's **Market Price**, allowing the alert threshold to adjust as the market changes.

It also gave me an opportunity to work with **web automation, asynchronous Python, data extraction, regular expressions, and webhook-based notifications** in a practical project.

---

## ✨ Features

* Tracks multiple TCGplayer products
* Retrieves the current TCGplayer Market Price
* Calculates a dynamic target based on Market Price
* Evaluates current marketplace listings
* Includes shipping when calculating total cost
* Identifies the lowest available total price
* Uses Playwright to handle dynamically rendered web content
* Sends Discord webhook notifications
* Includes product information and images in alerts
* Supports both one-time and continuous monitoring
* Includes fallback extraction methods if webpage structure changes

---

## ⚙️ How It Works

For each tracked product, the monitor:

1. Opens the TCGplayer product page using Playwright.
2. Waits for marketplace listings and pricing information to load.
3. Extracts the current **TCG Market Price**.
4. Calculates a dynamic target price:

```text
Target Price = Market Price × 70%
```

5. Examines the available marketplace listings.
6. Calculates the total cost of each listing:

```text
Total Cost = Listing Price + Shipping
```

7. Identifies the lowest total cost.
8. Sends a Discord notification when:

```text
Lowest Total Cost < Target Price
```

For example, if the Market Price is **$30.00**:

```text
Dynamic Target = $30.00 × 70%
               = $21.00
```

If a listing is available for:

```text
Card Price: $18.50
Shipping:    $1.25
------------------
Total:      $19.75
```

Since `$19.75 < $21.00`, the monitor triggers a Discord alert.

---

## 🛠️ Technologies

* **Python**
* **Jupyter Notebook**
* **Playwright**
* **Asyncio**
* **Requests**
* **Regular Expressions (Regex)**
* **Discord Webhooks**

---

## 📦 Installation

Clone the repository:

```bash
git clone <your-repository-url>
cd <your-repository-name>
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Install Chromium for Playwright:

```bash
playwright install chromium
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open the project's `.ipynb` file.

---

## 🔐 Discord Webhook Setup

For security, the Discord webhook URL is **not stored directly in the notebook**.

Set your Discord webhook as an environment variable before running the monitor:

```bash
export DISCORD_WEBHOOK_URL="your_discord_webhook_url"
```

The notebook retrieves it with:

```python
DISCORD_WEBHOOK_URL = os.getenv("DISCORD_WEBHOOK_URL")
```

> **Important:** Never commit your actual Discord webhook URL, API keys, tokens, or other credentials to a public repository.

---

## 🎯 Configuration

Products can be added or removed from the configuration:

```python
TRACKED_PRODUCTS = [
    {
        "product_id": 707591,
        "name": "Lacus EX Resource"
    },
    {
        "product_id": 707586,
        "name": "Sayla Mass EX Resource"
    },
    {
        "product_id": 707585,
        "name": "Miorine EX Resource"
    }
]
```

The alert percentage and monitoring interval can also be changed:

```python
DYNAMIC_PERCENTAGE = 0.70
CHECK_INTERVAL_SECONDS = 300
```

The default configuration checks every **5 minutes** and alerts when a listing falls below **70% of Market Price**.

---

## ▶️ Running the Monitor

### Run Once

For testing or demonstration:

```python
await run_once()
```

This checks every configured product once and then stops.

### Continuous Monitoring

For continuous monitoring:

```python
await main_loop()
```

The monitor will check all configured products, wait for the configured interval, and repeat until stopped.

---

## 🔔 Discord Alerts

When the target price is triggered, the Discord notification can include:

* Product name
* Lowest total cost
* Dynamic target price
* TCG Market Price
* Base listing price
* Shipping cost
* Card condition
* Seller information
* Product image
* Link to the TCGplayer product page

---

## 📁 Repository Structure

```text
TCGplayer-Price-Monitor/
│
├── TCGplayer_Price_Monitor.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

### `requirements.txt`

```text
playwright
requests
jupyter
```

### `.gitignore`

```text
.env
.ipynb_checkpoints/
__pycache__/
*.pyc
card_art_*.png
.DS_Store
venv/
.venv/
env/
```

---

## 🚀 Future Improvements

This project is still under development. Some improvements I plan to explore include:

* Duplicate-alert prevention and alert cooldowns
* Historical price tracking
* Price trend visualization
* Seller rating filters
* Card condition filters
* Persistent storage for historical listings
* Better handling of changes to TCGplayer's webpage structure
* Additional error handling and logging
* Cloud or Raspberry Pi deployment for 24/7 monitoring
* Support for additional products and configurable alert thresholds

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## ⚠️ Disclaimer

This project was created for **personal and educational purposes**.

It is not affiliated with, endorsed by, or sponsored by TCGplayer or Discord. TCGplayer's website structure may change over time, which may require updates to the data extraction logic.

Users are responsible for ensuring that their use of this project complies with applicable website terms, policies, and rate limits.
