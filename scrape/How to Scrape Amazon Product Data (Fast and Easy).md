# How to Scrape Amazon Product Data (Fast and Easy)

Extracting product data from Amazon has never been easier. This guide walks you through the process of setting up an Amazon scraper that pulls product information directly into a Google Sheet. Whether you're a beginner or an experienced user, this method offers a simple and efficient way to automate data collection.

---

## Step 1: Create Your Google Sheet

Start by creating a new Google Sheet. You can do this quickly in your Chrome browser by typing `sheet.new` (if you're logged into your Google account). Once created:

1. Name your sheet something like **Amazon Scraper**.
2. Create two tabs:
   - **Amazon Product Links**: For pasting the links of the Amazon products you want to scrape.
   - **Data**: Where the extracted product information will be stored.

---

## Step 2: Install the Amazon Scraper Template

To begin, you’ll need the Amazon scraper template from Axiom.ai. Follow these steps:

1. **Install the Template**: Click 'Install Template'. If you're new to Axiom.ai, you'll need to:
   - Install the Chrome extension.
   - Create a free Axiom.ai account.

2. **Start the Scraper**: Once installed, click 'Start' to configure the scraper.

---

### Advertising Content
Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more.  
👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Step 3: Configure the Amazon Scraper

Axiom.ai will guide you through the configuration steps. Here's what to do:

### Step 3.1: Read Data from a Google Sheet
- **Spreadsheet**: Select the Google Sheet you created earlier.
- **Sheet Name**: Choose the tab called **Amazon Product Links**.

### Step 3.2: Loop Through Data
- **Go to Page**: Use the 'Insert Data' option to select the column with Amazon product links.
- **Current URL**: No changes are needed here.

### Step 3.3: Extract Data
- Use the **Selector Tool** to point and click on the data you want to scrape (e.g., price, title, ratings).
- Set the **Max Results** to match the number of columns of data you are scraping.

### Step 3.4: Write Data to a Google Sheet
- **Spreadsheet**: Select your Google Sheet.
- **Sheet Name**: Choose the tab called **Data**.
- **Data**: Ensure this is set to `[scrape-data]`.
- **Clear Data Before Writing**: Set this to **Add to Existing Data**.

### Step 3.5: Delete Processed Links
- **Spreadsheet**: Select your Google Sheet.
- **Sheet Name**: Choose the **Amazon Product Links** tab.
- **First Row & Last Row**: Set both to 1 to delete the processed link after each loop.

---

## Step 4: Run Your Amazon Scraper

We recommend running a small test before processing the entire list. Stop the scraper after a few loops and review the data to ensure everything is being captured correctly.

---

## Step 5: Customize Your Template

Axiom.ai offers a no-code bot builder, allowing you to customize the scraper template to suit your needs. You can adjust the bot's functionality to extract additional data points or tweak the process for different requirements.

---

## Tips for Efficient Scraping

- **Set Loop Limits**: To scrape a specific number of rows, set a 'Last Cell' (e.g., `AE100` for 100 rows).
- **Selectors Not Working?**: Re-select or use custom selectors to refine your data extraction.
- **Optimize Scraper Speed**: Adjust the 'Max Results' to match the number of data columns you're extracting.
- **Avoid Overwriting Data**: Ensure the **Add to Existing Data** option is selected to prevent overwriting previous results.

---

## Troubleshooting Common Issues

- **No Data Written?**: Check that the **Data** setting is `[scrape-data]`.
- **Data Overwriting**: Ensure the **Add to Existing Data** option is enabled.
- **Scraper Running Slowly**: Toggle **Configure Scraper** and set 'Number of Retries' to 1 for faster performance.

---

With this Amazon scraper template, you can automate your data collection process in minutes. Pair it with a tool like ScraperAPI to ensure seamless and efficient scraping across large datasets.  
👉 [Try ScraperAPI now for free!](https://bit.ly/Scraperapi)
