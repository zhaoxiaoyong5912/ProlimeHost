
# A Comprehensive Guide to Scraping Dynamic Websites with Scrapy

Web scraping has evolved significantly. While scraping static websites is straightforward, modern websites often employ dynamic content rendering, making traditional methods less effective. Popular e-commerce platforms like JD.com and Taobao load product lists dynamically using JavaScript and AJAX. This creates challenges for web scraping, as traditional methods may fail to retrieve asynchronously loaded content.

This article provides an in-depth guide on using `scrapy-splash` to scrape JavaScript-heavy pages effectively.

---

## Introduction to Scrapy-Splash

`scrapy-splash` integrates [Splash](https://github.com/scrapy-plugins/scrapy-splash), a JavaScript rendering service, into Scrapy, enabling seamless scraping of dynamic websites. Splash acts as a lightweight browser with an HTTP API, built on Twisted and QT, and written in Python. To use `scrapy-splash`, you need to install and configure Splash.

---

## Setting Up Splash with Docker

The recommended way to install Splash is by using Docker. Follow these steps to set it up:

### Step 1: Install Docker
Refer to the [official Docker installation guide](https://docs.docker.com/engine/installation/linux/ubuntulinux/). Below is an example setup for Ubuntu 12.04 LTS:

1. **Update Kernel** (Docker requires kernel version 3.13 or higher):
   ```bash
   $ sudo apt-get update
   $ sudo apt-get install linux-image-generic-lts-trusty
   $ sudo reboot
   ```

2. **Install CA Certificates**:
   ```bash
   $ sudo apt-get install apt-transport-https ca-certificates
   ```

3. **Add Docker GPG Key**:
   ```bash
   $ sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
   ```

4. **Configure Docker Sources**:
   Add the following line to `/etc/apt/sources.list.d/docker.list`:
   ```
   deb https://apt.dockerproject.org/repo ubuntu-trusty main
   ```

5. **Install Docker**:
   ```bash
   $ sudo apt-get update
   $ sudo apt-get install docker-engine
   $ sudo service docker start
   ```

6. **Verify Installation**:
   ```bash
   $ sudo docker run hello-world
   ```
   This command downloads a test image, runs it, and outputs a message.

---

## Configuring Scrapy-Splash

After installing Splash, you need to configure your Scrapy project:

1. Add the `SPLASH_URL` to your `settings.py`:
   ```python
   SPLASH_URL = '<your_splash_instance_url>'
   ```

2. Include Splash's downloader middleware in `settings.py`:
   ```python
   DOWNLOADER_MIDDLEWARES = {
       'scrapy_splash.SplashCookiesMiddleware': 723,
       'scrapy_splash.SplashMiddleware': 725,
       'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 810,
   }
   ```

3. Enable Splash's duplicate filter:
   ```python
   DUPEFILTER_CLASS = 'scrapy_splash.SplashAwareDupeFilter'
   ```

4. Set up HTTP cache storage for Splash:
   ```python
   HTTPCACHE_STORAGE = 'scrapy_splash.SplashAwareFSCacheStorage'
   ```

---

## Using Scrapy-Splash for JavaScript-Rendered Content

The most straightforward way to render dynamic pages is by using `scrapy_splash.SplashRequest`. Here's an example:

```python
yield SplashRequest(
    url,
    self.parse_result,
    args={
        'wait': 0.5,
    },
    endpoint='render.json',  # Default is render.html
    splash_url='<splash_instance_url>',
    slot_policy=scrapy_splash.SlotPolicy.PER_DOMAIN,
)
```

You can also add the `splash` meta key to a standard Scrapy request for the same effect:

```python
yield scrapy.Request(
    url,
    self.parse_result,
    meta={
        'splash': {
            'args': {
                'html': 1,
                'png': 1,
            },
            'endpoint': 'render.json',
        }
    }
)
```

### Key Arguments in Splash Requests:

- **`args`**: Parameters sent to Splash's HTTP API, such as wait times and rendering options.
- **`endpoint`**: Specifies which Splash endpoint to use (default: `render.html`).
- **`slot_policy`**: Controls request synchronization for Splash.

---

## Example: Scraping JD.com

JD.com’s homepage uses asynchronous loading for content like the “You May Like” section. Here's how you can scrape this section using `scrapy-splash`:

1. **Without Splash**:
   ```python
   2016-04-18 14:42:44 INFO find: None
   ```
   The "You May Like" section cannot be found because it's dynamically loaded.

2. **With Splash**:
   ```python
   2016-04-18 14:42:51 INFO find: You May Like
   2016-04-18 14:42:51 INFO success
   ```
   The content is successfully retrieved after rendering with Splash.

---

## Advanced Features

### Form Requests
Use `scrapy_splash.SplashFormRequest` to submit forms through Splash. It works similarly to `SplashRequest`.

### Custom Responses
`scrapy-splash` returns different response types based on the endpoint used:
- **`SplashResponse`**: Binary responses (e.g., images).
- **`SplashTextResponse`**: Text responses (e.g., HTML).
- **`SplashJsonResponse`**: JSON responses (e.g., API responses).

To use standard Scrapy responses, set:
```python
meta['splash']['dont_process_response'] = True
```

---

## Final Thoughts

Scraping dynamic websites is no longer a daunting task with tools like `scrapy-splash`. By integrating Splash into Scrapy, you can easily handle JavaScript-rendered content and enhance your web scraping capabilities.

Stop wasting time on proxies and CAPTCHAs! ScraperAPI's simple API handles millions of web scraping requests, so you can focus on the data. Get structured data from Amazon, Google, Walmart, and more. 👉 [Start your free trial today!](https://bit.ly/Scraperapi)
