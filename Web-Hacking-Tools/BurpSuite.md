# Introduction to Burp Suite

Burp Suite is a professional toolkit for web application security testing. It primarily functions as an intercepting proxy, sitting between a user's browser and the target web server. This allows an analyst to inspect, modify, and replay web traffic to find security vulnerabilities.

---

## Core Components

-   **Proxy:** The heart of Burp, used to intercept and view HTTP/S traffic.
-   **Repeater:** Allows for manual modification and re-sending of individual requests to analyze server responses.
-   **Intruder:** Automates sending customized requests to test for a variety of vulnerabilities.

---

## Basic Setup: Intercepting Traffic

My first exercise was to intercept the traffic to my own GitHub Pages site to understand the flow.

### 1. Browser Proxy Configuration
The first step is to configure the browser's proxy settings to point to Burp's default listener. This redirects all browser traffic through Burp.

-   **Host/IP:** `127.0.0.1`
-   **Port:** `8080`

### 2. Installing the CA Certificate
To intercept and decrypt HTTPS traffic without browser errors, Burp's CA certificate must be installed and trusted by the browser.

1.  With the proxy configured, navigate to `http://burpsuite` in the browser.
2.  Download the `cacert.der` file.
3.  In the browser's certificate manager, import and set the certificate to be trusted for identifying websites.

### 3. A Captured Request
With the setup complete, I enabled 'Intercept is on' and navigated to my website. Burp successfully captured the `GET` request. This allows for a deep inspection of all request headers, such as `Host`, `User-Agent`, and `Cookies`. By clicking "Forward," the request is sent to the server, and the subsequent response is also captured before being sent to the browser.



## Lessons Learned

Burp Suite provides incredible visibility into the normally invisible communication between a browser and a server. Mastering the Proxy tool is the first and most critical step in web application penetration testing.
