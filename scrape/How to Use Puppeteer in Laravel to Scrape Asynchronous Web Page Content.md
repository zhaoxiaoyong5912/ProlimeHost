
# How to Use Puppeteer in Laravel to Scrape Asynchronous Web Page Content

## Summary

Scraping web page content is a common requirement in many projects. For traditional static pages, simple tools like `curl` are sufficient. However, when dealing with dynamically loaded content, such as articles loaded via AJAX or pages that undergo additional processing (e.g., replacing image URLs), traditional tools may fall short. Enter Puppeteer—a robust, modern tool built by the Chrome team that leverages Chrome's Headless feature.

In this article, we will explore how to use Puppeteer in a Laravel environment by utilizing the `spatie/browsershot` package.

---

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)

---

## Why Puppeteer?

While tools like PhantomJS were once the gold standard for scraping dynamic content, Puppeteer has risen to prominence due to its reliability and modern architecture. Puppeteer is a JavaScript library developed and maintained by the official Chrome team, making it highly dependable for handling tasks like dynamic content scraping, browser automation, and testing.

To use Puppeteer in a Laravel project, we need the help of another powerful tool: `spatie/browsershot`.

---

## Installation Guide

### Step 1: Install `spatie/browsershot`

`Browsershot` is a Composer package developed by the renowned Spatie team.

```bash
$ composer require spatie/browsershot
```

### Step 2: Install Puppeteer

Puppeteer is an NPM package. It can be installed as a project dependency for better control and compatibility.

```bash
$ npm i puppeteer --save
```

> **Tip:** While Puppeteer can be installed globally, it's recommended to install it within your project. This ensures that different projects remain isolated from global dependencies, and updates can be managed without impacting live projects. Keep in mind that installing Puppeteer may take some time, as it includes the Chromium browser.

### Special Note for Chinese Users

During Puppeteer's installation, Chromium may fail to download due to network restrictions. In such cases, you might need to employ a local mirror or other workarounds.

---

## How to Use Puppeteer in Laravel

### Example: Scraping Article Content from a Dynamic Page

The following example demonstrates how to scrape the content of a mobile version of a news article using Puppeteer and `spatie/browsershot`.

```php
use Spatie\Browsershot\Browsershot;

public function getBodyHtml()
{
    $newsUrl = "https://m.toutiao.com/i6546884151050502660/";

    $html = Browsershot::url($newsUrl)
        ->windowSize(480, 800)
        ->userAgent("Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Mobile Safari/537.36")
        ->mobile()
        ->touch()
        ->bodyHtml();

    Log::info($html);
}
```

### Saving as an Image or PDF

In addition to scraping content, you can use Puppeteer to save the web page as an image or PDF. Here's how:

```php
use Spatie\Browsershot\Browsershot;

public function savePageAsImage()
{
    $newsUrl = "https://m.toutiao.com/i6546884151050502660/";

    Browsershot::url($newsUrl)
        ->windowSize(480, 800)
        ->userAgent("Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Mobile Safari/537.36")
        ->mobile()
        ->touch()
        ->setDelay(1000) // Wait for content to load
        ->save(public_path("images/toutiao.jpg"));
}
```

> **Note:** The `setDelay()` method ensures that content has time to load fully before taking a screenshot. While effective, it may not always be the most efficient approach.

---

## Potential Issues and Solutions

### Chromium Browser Compatibility

Puppeteer relies on Chromium, which is supported on most systems. If Chromium is not available, consider using PhantomJS as a fallback.

### File Permission Issues

After installing Puppeteer, you may encounter permission issues when trying to execute it. To resolve this, ensure appropriate permissions are set for the `/node_modules/puppeteer` directory.

---

## Conclusion

Puppeteer is a powerful tool for scenarios involving dynamic content scraping, browser automation, and testing. When combined with Laravel and the `spatie/browsershot` package, it becomes a potent solution for small-scale scraping tasks like the examples covered in this article. However, for high-volume scraping or advanced use cases, consider tools like Python-based frameworks.

---

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free 
