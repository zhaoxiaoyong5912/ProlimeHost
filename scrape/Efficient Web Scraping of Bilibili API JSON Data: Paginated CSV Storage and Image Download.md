
# Efficient Web Scraping of Bilibili API JSON Data: Paginated CSV Storage and Image Download

## Introduction

When working with APIs that return JSON data, you can skip the tedious task of parsing HTML and directly extract and save the required data. In this article, we’ll explore a step-by-step guide to scrape data from the Bilibili API, store it in a CSV file, and download associated images.

---

### API Request Overview

The Bilibili API returns JSON data with a variety of information. Below is an example request for fetching video data:

**Request URL:**
`https://api.bilibili.com/x/space/arc/search?mid=390461123&ps=30&tid=0&pn=17&keyword=&order=pubdate&jsonp=jsonp`

**Request Details:**
```plaintext
Request Method: GET
Status Code: 200
Content-Type: application/json; charset=utf-8
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
```

Below is the structure of the JSON response:
- **`data.list.vlist`**: Contains the list of videos.
- Fields of interest: `aid` (video ID), `pic` (thumbnail URL), and `title`.

---

## Code Implementation

### Core Libraries and Setup

To extract, process, and store data efficiently, the following Python libraries are used:
- `requests` for sending HTTP requests.
- `csv` and `pandas` for handling CSV data.
- `os` for file handling.
- `time` to add delays and prevent API rate limiting.

Below is the Python script for scraping Bilibili data:

```python
import requests
import csv
import os
import time

# Define the API URL
API_URL = 'https://api.bilibili.com/x/space/arc/search?mid=390461123&ps=30&tid=0&pn={page}&keyword=&order=pubdate&jsonp=jsonp'

# Custom headers to mimic browser behavior
HEADERS = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36'
}

# Function to fetch data from API
def fetch_data(page):
    url = API_URL.format(page=page)
    response = requests.get(url, headers=HEADERS)
    return response.json()

# Function to write data to CSV
def write_to_csv(item_list, file_name="bilibili.csv"):
    if not os.path.exists(file_name):
        with open(file_name, 'w', encoding='utf-8', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(["Video ID", "Thumbnail URL", "Title"])
    with open(file_name, 'a', encoding='utf-8', newline='') as file:
        writer = csv.writer(file)
        for item in item_list:
            writer.writerow([item['aid'], item['pic'], item['title']])

# Function to download images
def download_images(item_list, directory="images"):
    if not os.path.exists(directory):
        os.makedirs(directory)
    for item in item_list:
        img_url = item['pic']
        img_name = os.path.join(directory, img_url.split('/')[-1])
        response = requests.get(img_url)
        with open(img_name, 'wb') as img_file:
            img_file.write(response.content)

# Main scraping function
def scrape_bilibili_data(total_pages=28):
    for page in range(1, total_pages + 1):
        print(f"Scraping page {page}...")
        data = fetch_data(page)
        video_list = data.get('data', {}).get('list', {}).get('vlist', [])
        if video_list:
            write_to_csv(video_list)
            download_images(video_list)
        time.sleep(1)  # Avoid API rate limiting

if __name__ == "__main__":
    scrape_bilibili_data()
```

---

### Key Observations

1. **API Pagination:**
   The API supports pagination through the `pn` parameter:
   ```
   https://api.bilibili.com/x/space/arc/search?mid=390461123&ps=30&tid=0&pn=<PAGE_NUMBER>&keyword=&order=pubdate&jsonp=jsonp
   ```

2. **CSV Storage:**
   - Saves video `aid`, `pic`, and `title` into a structured format.

3. **Image Downloading:**
   - Saves all thumbnails to a local directory named `images`.

---

## Stop Wasting Time on Proxies and CAPTCHAs

Scraping data shouldn’t be a struggle. [ScraperAPI](https://bit.ly/Scraperapi) simplifies the process with a powerful API that handles millions of requests effortlessly.

- Get structured data from platforms like Amazon, Google, and more.
- Forget about dealing with proxies, CAPTCHAs, and rate-limiting errors.

👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

### Advanced Case: Paginated Scraping and Image Storage

In cases where multiple pages are needed, the pagination function ensures every page is processed. Below is an advanced implementation combining CSV and image downloads:

```python
def scrape_and_store_data(total_pages=10):
    for page in range(1, total_pages + 1):
        print(f"Processing page {page}...")
        response = fetch_data(page)
        data = response.get('data', {})
        video_list = data.get('list', {}).get('vlist', [])
        if video_list:
            write_to_csv(video_list)
            download_images(video_list)
        time.sleep(1)  # Pause to avoid hitting rate limits
```

This method ensures the data integrity of large-scale scraping.

---

## Conclusion

The Bilibili API makes it simple to fetch structured JSON data. With a combination of Python libraries like `requests` and `csv`, you can effectively process, store, and utilize this data. The provided script can be extended further for analytics or other use cases.

---

### Reminder: Simplify Scraping with ScraperAPI

Don’t let proxies and CAPTCHAs slow you down. ScraperAPI takes care of these challenges so you can focus on what matters—data.

👉 [Try ScraperAPI for free now!](https://bit.ly/Scraperapi)
