# TCGPlayer_Price_Monitor_Project


A Python-based price monitoring tool that tracks selected products on TCGplayer and sends a Discord notification when a listing falls below a dynamically calculated target price.

Instead of using a fixed price threshold, the monitor retrieves the current **TCGplayer Market Price** and automatically calculates the target price as a percentage of the market value. By default, an alert is triggered when the lowest available listing, including shipping, falls below **70% of the current Market Price**.

## Features

* Monitors multiple TCGplayer products
* Retrieves the current TCGplayer Market Price
* Dynamically calculates a target price based on market value
* Analyzes current marketplace listings
* Includes shipping when calculating the lowest total price
* Captures product images using Playwright
* Sends Discord alerts when the target price is reached
* Displays price, shipping, seller, condition, and market information
* Automatically repeats checks at a configurable interval
* Includes fallback methods for dynamically rendered page content

## How It Works

For each configured product, the program:

1. Opens the TCGplayer product page using Playwright.
2. Waits for the dynamically rendered marketplace data.
3. Extracts the current TCGplayer Market Price.
4. Calculates the target price:

```text
Target Price = Market Price × 70%
```

5. Examines the available marketplace listings.
6. Calculates the total cost of each listing:

```text
Total Cost = Listing Price + Shipping
```

7. Identifies the lowest total cost.
8. Sends a Discord alert if:

```text
Lowest Total Cost < Target Price
```

The monitoring process then waits for the configured interval before checking the products again.

## Example

If a product has a TCGplayer Market Price of:

```text
$30.00
```

the monitor calculates:

```text
$30.00 × 0.70 = $21.00
```

If an available listing costs:

```text
Card Price: $18.50
Shipping:    $1.25
Total:       $19.75
```

the program detects that:

```text
$19.75 < $21.00
```

and sends an alert to Discord.

## Technologies

* Python
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

Install the required Python packages:

```bash
pip install playwright requests
```

Install the Playwright Chromium browser:

```bash
playwright install chromium
```

## Configuration

Products can be added or removed from `TRACKED_PRODUCTS`:

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

The alert threshold and monitoring interval can also be customized:

```python
CHECK_INTERVAL_SECONDS = 300
DYNAMIC_PERCENTAGE = 0.70
```

This configuration checks prices every five minutes and triggers an alert below 70% of the current Market Price.

## Discord Webhook

For security, do **not** hard-code your Discord webhook URL into the source code.

Store it as an environment variable:

```bash
export DISCORD_WEBHOOK_URL="your_webhook_url"
```

Then retrieve it in Python:

```python
DISCORD_WEBHOOK_URL = os.getenv("DISCORD_WEBHOOK_URL")
```

If using a `.env` file, make sure `.env` is included in `.gitignore`.

## Running the Monitor

For a standard Python script:

```python
if __name__ == "__main__":
    asyncio.run(main_loop())
```

Then run:

```bash
python monitor.py
```

If running inside Jupyter Notebook, use:

```python
await main_loop()
```

## Discord Alerts

When the target threshold is reached, the Discord notification includes:

* Product name
* Lowest total cost
* Dynamic target price
* TCGplayer Market Price
* Listing price
* Shipping cost
* Card condition
* Seller information
* Product image
* Link to the TCGplayer product page

## Project Structure

```text
tcgplayer-price-monitor/
│
├── monitor.py
├── README.md
├── requirements.txt
└── .gitignore
```

## Disclaimer

This project is intended for personal and educational use. It is not affiliated with, endorsed by, or sponsored by TCGplayer or Discord.

Website structure and dynamically rendered content may change over time, which can require updates to the scraping logic. Users should also ensure their use of the project complies with the applicable website terms and policies.

## Future Improvements

Potential improvements include duplicate-alert prevention, persistent price history, additional listing filters, configurable product conditions, improved error handling, automated deployment, and historical price analysis.
