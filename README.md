# TCGPlayer_Price_Monitor_Project

# TCGplayer Dynamic Price Monitor

A Python-based price monitoring tool built in Jupyter Notebook that tracks selected products on TCGplayer and sends Discord notifications when a listing falls below a dynamically calculated target price.

Instead of using a fixed threshold, the monitor retrieves the current **TCGplayer Market Price** and calculates the target as a percentage of that value. By default, an alert is triggered when the lowest available listing, including shipping, falls below **70% of the current Market Price**.

## Features

* Monitors multiple TCGplayer products
* Retrieves the current TCGplayer Market Price
* Dynamically calculates a target price based on market value
* Analyzes current marketplace listings
* Includes shipping in the total price calculation
* Uses Playwright to handle dynamically rendered content
* Captures product images for alerts
* Sends Discord notifications when the target price is reached
* Displays price, shipping, seller, condition, and market information
* Automatically repeats checks at a configurable interval
* Includes fallback methods for Market Price extraction

## How It Works

For each configured product, the notebook:

1. Opens the TCGplayer product page using Playwright.
2. Waits for dynamically rendered marketplace data.
3. Extracts the current TCGplayer Market Price.
4. Calculates the target price:

```text
Target Price = Market Price × 70%
```

5. Analyzes the available marketplace listings.
6. Calculates the total cost:

```text
Total Cost = Listing Price + Shipping
```

7. Identifies the lowest total cost.
8. Sends a Discord alert when:

```text
Lowest Total Cost < Target Price
```

The monitor then waits for the configured interval before checking all tracked products again.

## Example

If the current Market Price is:

```text
$30.00
```

the 70% dynamic target is:

```text
$30.00 × 0.70 = $21.00
```

If a listing is available for:

```text
Card Price: $18.50
Shipping:    $1.25
Total:       $19.75
```

the monitor detects:

```text
$19.75 < $21.00
```

and sends an alert to Discord.

## Technologies

* Python
* Jupyter Notebook
* Playwright
* Asyncio
* Requests
* Regular Expressions (Regex)
* Discord Webhooks

## Installation

Clone the repository:

```bash
git clone <your-repository-url>
cd <your-repository-name>
```

Install the dependencies:

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

Open:

```text
TCGplayer_Price_Monitor.ipynb
```

and run the notebook cells.

Because the project runs in Jupyter, the asynchronous monitoring loop is started with:

```python
await main_loop()
```

## Configuration

Products can be added to `TRACKED_PRODUCTS`:

```python
TRACKED_PRODUCTS = [
    {
        "product_id": 707591,
        "name": "Lacus EX Resource"
    },
    {
        "product_id": 707586,
        "name": "Sayla Mass EX Resource"
    }
]
```

The monitoring interval and alert threshold can also be adjusted:

```python
CHECK_INTERVAL_SECONDS = 300
DYNAMIC_PERCENTAGE = 0.70
```

The default configuration checks prices every five minutes and alerts when the lowest total listing price falls below 70% of the current Market Price.

## Discord Webhook Setup

For security, the Discord webhook URL should **not** be stored directly in the notebook.

Store it as an environment variable:

```bash
export DISCORD_WEBHOOK_URL="your_webhook_url"
```

Then retrieve it in the notebook:

```python
DISCORD_WEBHOOK_URL = os.getenv("DISCORD_WEBHOOK_URL")
```

Never commit your actual Discord webhook URL to a public GitHub repository.

## Discord Alert

When the target threshold is reached, the notification includes:

* Product name
* Lowest total cost
* Dynamic target price
* TCGplayer Market Price
* Base listing price
* Shipping cost
* Card condition
* Seller information
* Product image
* Direct link to the TCGplayer product page

## Repository Structure

```text
tcgplayer-price-monitor/
│
├── TCGplayer_Price_Monitor.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Disclaimer

This project was developed for personal and educational purposes. It is not affiliated with, endorsed by, or sponsored by TCGplayer or Discord.

Website structure and dynamically rendered content may change over time and may require updates to the scraping logic. Users are responsible for ensuring their use complies with applicable website terms and policies.

## Future Improvements

Future development could include:

* Duplicate-alert prevention
* Historical price tracking
* Price trend visualization
* Configurable card-condition filters
* Seller-rating filters
* Persistent price storage
* Additional error handling
* Automated cloud deployment
* Expanded support for additional products
