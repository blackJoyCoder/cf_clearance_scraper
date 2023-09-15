# CF-Clearance-Scraper

## undetected-chromedriver Version
A simple program for scraping Cloudflare clearance (cf_clearance) cookies from websites issuing Cloudflare challenges to visitors. This program works on all Cloudflare challenge types (JavaScript, managed, and interactive).

## Clearance Cookie Usage
In order to bypass Cloudflare challenges with the clearance cookies, you must make sure of two things:

- The user agent used to fetch the clearance cookie must match the user agent being used within the requests that use the clearance cookie
    > **Note**
    > The default user agent used by the scraper is `Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36`.
- The IP address used to fetch the clearance cookie must match the IP address being used to make the requests that use the clearance cookie

```mermaid
flowchart
	N14e["cf_clearance"]
	N14f["IP Address"]
	N150["User Agent"]
	N14e --> N14f
	N14e --> N150
```

## Installation
    $ pip install -r requirements.txt

Next, you'll need to install the CA certificate for Selenium Wire. First, download the certificate:

    $ python -m seleniumwire extractcert

Then, navigate to Chrome's settings. From there, go to `Privacy and security` > `Security` > `Manage device certificates`. The next steps will vary depending on your operating system.

### Windows
Once the `Certificates` menu opens, navigate to the `Trusted Root Certification Authorities` tab and click `Import`. Select the certificate you downloaded and click `Next`, `Next`, and `Finish`.

### MacOS
Once the `Keychain Access` app opens, go to `File` > `Import Items`. Select the certificate you downloaded and click `Open`. Navigate to the `Certificates` tab in the `Keychain Access` app. Right click the Selenium Wire CA certificate and select `Get Info`. Expand the `Trust` section and set `When using this certificate` to `Always Trust`.

## Usage
> **Note**
> Depending on the user agent used, it may affect your ability to solve the Cloudflare challenge.

```
usage: main.py [-h] [-f FILE] [-t TIMEOUT] [-p PROXY] [-ua USER_AGENT] [--disable-http2] [--disable-http3] [-d] [-v] URL

A simple program for scraping Cloudflare clearance (cf_clearance) cookies from websites issuing Cloudflare challenges to visitors

positional arguments:
  URL                   The URL to scrape the Cloudflare clearance cookie from

options:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  The file to write the Cloudflare clearance cookie information to, in JSON format
  -t TIMEOUT, --timeout TIMEOUT
                        The timeout in seconds for browser actions and solving challenges
  -p PROXY, --proxy PROXY
                        The proxy server URL to use for the browser requests
  -ua USER_AGENT, --user-agent USER_AGENT
                        The user agent to use for the browser requests
  --disable-http2       Disable the usage of HTTP/2 for the browser requests
  --disable-http3       Disable the usage of HTTP/3 for the browser requests
  -d, --debug           Run the browser in headed mode
  -v, --verbose         Increase the output verbosity
```

## Example
    $ python main.py -v -f cookies.json https://nowsecure.nl
    [11:33:32] [INFO] Launching headless browser...
    [11:33:34] [INFO] Going to https://nowsecure.nl...
    [11:33:34] [INFO] Solving Cloudflare challenge [Managed]...
    [11:33:38] [INFO] Cookie: cf_clearance=SNMwlsKbfROOWr3FU0jgPn0WY3.z1sn5_b3W6aSRwh8-1690648414-0-160.0.0
    [11:33:38] [INFO] User agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36
    [11:33:38] [INFO] Writing Cloudflare clearance cookie information to cookies.json...
