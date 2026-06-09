---
title: "What is a User Agent? Definition for Browsers, Devices, and Analytics"
date: June 09, 2026
standalone: true
intro: "A user agent is a browser-provided string that identifies the client application, operating system, browser version, and device context. Learn how analytics tools use it."
---

A user agent is a string of information sent by a browser or client application when it requests a web page. It usually describes the browser, operating system, device type, rendering engine, and version information.

Web servers and analytics tools use user agent data to understand what kinds of browsers and devices are accessing a website.

## User agent example

A user agent string can look like this:

```txt
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36
```

This string can indicate that the visitor is using Chrome on macOS. Actual user agent strings can be long, inconsistent, and sometimes intentionally reduced for privacy.

## How analytics uses user agent data

Analytics platforms can parse user agent strings to report:

- Browser
- Browser version
- Operating system
- Device type
- Rendering engine
- Bot signals

This helps teams debug compatibility issues, prioritize browser support, and understand device trends.

## Privacy and accuracy

User agent data is not always accurate. Browsers may freeze or reduce user agent strings to limit fingerprinting. Some bots pretend to be normal browsers. Some privacy tools alter or mask user agent information.

Analytics teams should use user agent data for aggregate reporting and debugging, not invasive visitor identification.

Swetrix provides browser, OS, and device analytics in a privacy-conscious way, helping teams understand technical context without building cross-site profiles.

Related terms: [IP address](https://swetrix.com/glossary/ip-address), [personal data](https://swetrix.com/glossary/personal-data), [adblocker](https://swetrix.com/glossary/adblocker), and [page speed](https://swetrix.com/glossary/page-speed).

::CTA:TIME_TO_SWITCH::
