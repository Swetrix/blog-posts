---
title: "IP Tracking: What Websites Can See"
intro: "Learn what IP tracking can reveal, why geolocation is approximate, and how privacy-conscious analytics can use IPs without retaining raw addresses."
date: October 24, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/214e8523-a021-490a-8dd4-a1c2041a4158/0-5db9e3d55353.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "214e8523-a021-490a-8dd4-a1c2041a4158"
---

IP tracking is the process of using the network address associated with a web request to derive signals about a visitor, such as approximate geographic location, network context, or security status. Websites receive this address automatically to route content back to the browser, but receiving an IP address is distinct from logging, processing, or retaining it. Some privacy-conscious analytics platforms process IP addresses transiently to count sessions or estimate regions without retaining the raw address, as described in [Swetrix’s Events API documentation](https://swetrix.com/docs/events-api). You can use IP signals for measurement, traffic filtering, and troubleshooting rather than to pinpoint specific individuals.

## How Websites Track and Process IP Addresses

When a visitor types a URL or clicks a link, their browser sends a request through the network to the website’s server. The request reaches the server with a source IP address that the network uses to route a response back to the browser. The website infrastructure uses that address to return the HTML document, images, or stylesheets, but server configuration determines whether the address is also processed or retained. Different layers of the technology stack handle the address in distinct ways:

| Action | Technical Mechanism | Data Retention Result |
| :--- | :--- | :--- |
| Receiving | The server receives the network request with a source address used to return content. | Receiving alone does not determine whether the address is retained. |
| Processing | Software may use the address for a geolocation lookup or temporary identifier. | The raw address may be discarded after processing, depending on configuration. |
| Logging | The server writes the raw address to an access file or database. | Retention follows the configured rotation and deletion policy. |

A standard web server might write the raw address into a text log alongside the requested page path and timestamp. An analytics application may instead combine the address and User-Agent with a salt to derive a temporary session hash without storing the raw address, as described in [Swetrix’s Events API documentation](https://swetrix.com/docs/events-api).

Network architecture also affects which address the analytics system receives. If a visitor uses a VPN, the website generally sees the VPN’s public exit address rather than the visitor’s home IP. Requests passing through a load balancer or content delivery network can also appear to come from that intermediary unless the original address is forwarded. For events sent from a backend or proxy, the analytics endpoint may need a forwarded-IP header to receive the visitor’s address. The [Swetrix Events API documentation](https://swetrix.com/docs/events-api) says those requests should include the visitor’s IP in `X-Forwarded-For`; forwarding the server or CDN address instead can make visitor counts inaccurate and geolocation reflect the server’s location.

![Show a visitor at a laptop beside a map shaded with a broad, soft regional area rather than a pin on a home, making approximate geolocation clear.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/214e8523-a021-490a-8dd4-a1c2041a4158/1-44672626c5c3.webp)

## What an IP Address Can and Cannot Reveal

Website owners can overestimate the detail a network signal provides. An IP address provides broad network context and can support a location estimate, but it cannot pinpoint a person or a street address.

### Estimating Geographic Location

IP geolocation tools compare an address with provider databases to estimate its location. The result is an estimate, not a precise device location.

Accuracy varies by country and network type. For its GeoIP products, [MaxMind estimates 99.8% accuracy at the country level](https://support.maxmind.com/knowledge-base/articles/maxmind-geolocation-accuracy), while its estimates for U.S. IP addresses are around 80% at the state or region level and 66% at the city level within a 50-kilometer radius. Mobile networks, business VPNs, and consumer privacy networks can reduce precision, sometimes leaving only a broad region or no city or postal code. Country-level and regional reporting remains useful for understanding where your audience lives generally, but it cannot show the street where visitors are located.

### Identifying Individual Visitors

Using an IP address as a person-level identifier assumes that one address maps to one user and remains stable over time. Modern networks break both assumptions.

Consumer ISPs may assign dynamic IP addresses that change over time, and an address used by one household may later be reassigned to another subscriber. Shared networks complicate the picture further. Students using a university library or employees behind a corporate firewall can share a single outward-facing IP address. An analytics system that relies on the address alone may group those visits together rather than distinguish each visitor, which can distort returning-visitor counts on enterprise networks.

![Show several coworkers using different devices through one office router whose outward network signal appears as one path, illustrating how shared IPs can blur visitor counts.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/214e8523-a021-490a-8dd4-a1c2041a4158/2-6fb5eade3853.webp)

## Common Uses for IP-Based Signals

Network signals provide context that helps you analyze traffic patterns and maintain server health. You can treat the IP address as a supporting metric rather than a substitute for measuring direct business outcomes.

### Gathering Audience Insights and Excluding Internal Traffic

You can use broad geographic reports to understand your readership. Seeing that traffic clusters in a specific region helps you tailor content, adjust publication schedules, or decide which search topics to pursue, especially when you pair regional data with referrers and search performance metrics to form a fuller picture of content discovery.

You can also use IP addresses to clean your analytics data. Team members who repeatedly check the company website can skew conversion rates and pageview counts. By configuring analytics to ignore traffic from your office network using an IP address or CIDR range, you can remove internal visits from reports and make it easier to measure external activity. This provides a cleaner baseline for tracking leads and purchases through events and funnels.

### Security Filtering and Identifying B2B Organizations

Monitoring incoming network addresses helps protect your servers, because frequent requests from a single source might indicate scraping, a vulnerability scan, or a denial-of-service attempt. You can use IP-based filtering rules to block traffic from known malicious networks or restrict administrative access to a designated office IP range. An address alone does not prove malicious intent, but it can support traffic management.

For B2B software companies, an IP address can hint at which organizations are visiting a site. Corporate networks may have address ranges registered to an organization, and identifying a visiting organization can provide account-level insight, but the IP address does not identify a specific employee. This method becomes less reliable when employees work remotely, since a visitor connecting from home usually appears under a residential ISP rather than the target account’s network.

![Depict a web request reaching an analytics server, where a temporary visitor signal and broad region emerge while the raw address fades before the storage layer.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/214e8523-a021-490a-8dd4-a1c2041a4158/3-1073f5786862.webp)

## How IP Processing Differs From Cookie Tracking

Moving away from cookie-based tracking does not automatically eliminate privacy considerations. Processing a network address involves different mechanics and legal frameworks than setting a client-side tracker.

### Mechanical Differences Between Cookies and IPs

A cookie is a small text file that a website asks the browser to store on the visitor’s device. When the visitor returns to the same site, the browser can send the cookie back, allowing the site to recognize that browser across visits while the cookie remains available. This mechanism can support persistent recognition across changing networks, though a cookie’s scope, expiration, and availability depend on its settings and the browser.

An IP address is a network-level signal that accompanies a request rather than a value stored by the website in the browser. Changing from mobile data to home Wi-Fi often changes the public IP address, so the address alone cannot reliably connect those sessions. Learning how [cookieless tracking works](https://swetrix.com/blog/cookieless-tracking-how-it-works) requires understanding that platforms still receive the IP address to process a request. Choosing a cookieless tool means it does not rely on cookies for tracking, but it may still process network identifiers or other request data.

### Legal and Privacy Context

The legal status of an IP address depends on how a website uses it and what other information the operator holds. An IP address should not be assumed anonymous in every context.

The Court of Justice of the European Union’s [2016 Breyer judgment](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX%3A62014CJ0582) held that a dynamic IP address can constitute personal data for a website operator when the operator has legal means to identify the visitor using additional information held by the internet service provider. The holding is conditional, so it does not mean every IP address is personal data in every situation. Storing raw IP addresses alongside session data can make records easier to link with other information, so website operators should assess the purpose and retention of their own setup under applicable law.

## Processing Analytics Without Storing Raw Addresses

You can measure website traffic without keeping a database of raw visitor IP addresses. Choosing a [Google Analytics alternative](https://swetrix.com/google-analytics-alternative) does not automatically determine how raw IP addresses are handled, so check the provider’s retention documentation.

### Data Processing and Retention

For its Events API, [Swetrix’s documentation](https://swetrix.com/docs/events-api) describes using incoming IP and User-Agent information to derive a temporary salted session and generate geolocation reports, while not storing the raw IP address in its original form.

The documentation says the temporary session is removed from the database 30 minutes after the last interaction or at 00:00 UTC, whichever comes first. Because this identifier uses network and browser signals rather than a persistent device ID, it is not designed to identify the same person across devices. Shared networks can also cause multiple people with the same IP address and User-Agent to be counted together, which may lower unique-visitor totals in offices or schools. This trade-off limits persistent profiling while preserving session-level measurement.

### Measuring Outcomes Without Invasive Tracking

Teams can still measure more than pageviews without persistent identifiers, because events and funnels show how people move through key flows. A privacy-conscious setup can provide behavioral insights when configured for the data a team needs.

Teams can measure how people navigate a checkout flow with page-based and event-based funnels. Technical teams can also use tools to [monitor application errors](https://swetrix.com/error-tracking) and catch broken scripts without tying those failures to a person’s IP-based profile, depending on the tool’s configuration.

Session replay tools can offer visual context for user behavior, but what a recording captures depends on the tool and its settings. Before enabling one, review its legal basis, masking options, and retention controls, and avoid sending personal data through URLs or event metadata. Privacy-conscious analytics can help you observe site performance, while configuration determines what visitor data is collected and retained.

## Frequently Asked Questions About IP Tracking

Understanding network signals requires separating technical mechanics from privacy policies. Websites receive network information automatically, but software configuration dictates what happens next.

### Do websites automatically track IP addresses?

Websites receive the IP address associated with a request because the network needs it to route a response back to the browser. Whether a site logs or otherwise retains it depends on server and analytics configuration. Some systems discard it after processing, while others retain it under their log policies.

### Does an IP address reveal a physical location?

IP geolocation can estimate a country, region, or city, but precision varies by provider and network. For U.S. IP addresses, [MaxMind estimates 66% city-level accuracy within a 50-kilometer radius](https://support.maxmind.com/knowledge-base/articles/maxmind-geolocation-accuracy); some mobile or VPN addresses may receive only regional data. An IP address does not pinpoint a specific street address, building, or household.

### Does an IP address identify a specific person?

An IP address alone usually does not show who used a device. Shared networks can place multiple visitors behind one public address, and linking an address to a person may require additional information such as records held by an ISP, as the [Breyer judgment](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX%3A62014CJ0582) explains.

### How does IP tracking differ from cookie tracking?

A cookie is stored in a browser and can help a site recognize it across visits, even when the visitor changes networks. An IP address accompanies network requests and may change between networks, but it is not a reliable person-level identifier.

### Do cookieless analytics platforms use IP addresses?

Cookieless analytics describes a setup that does not rely on cookies; it does not mean the platform never receives IP addresses. Swetrix’s [Events API documentation](https://swetrix.com/docs/events-api) says it uses IP and User-Agent information for a temporary salted session and geolocation, without storing raw IP addresses.

### Does Google Analytics retain raw IP addresses?

For GA4 measurement, Google says IP addresses are used at collection time to derive location information, then discarded before data is logged on any data center or server, according to [Google’s regional data collection documentation](https://support.google.com/analytics/answer/11598602).

### Are websites legally allowed to log IP addresses?

Legality depends on jurisdiction, purpose, legal basis, and configuration. In the EU, the [Breyer judgment](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=CELEX%3A62014CJ0582) held that a dynamic IP can be personal data for a website operator when it has legal means to identify the visitor with additional information from an ISP.

### Does Swetrix retain raw visitor IP addresses?

Swetrix says it does not store raw visitor IP addresses in its analytics database. Its [Events API documentation](https://swetrix.com/docs/events-api) says the IP is used for a temporary salted session and geolocation reports, while the session hash is removed after 30 minutes without activity or at midnight UTC, whichever comes first.

---

Moving to a privacy-conscious analytics setup can reduce the amount of raw network data you retain while still giving your team useful product insights. Explore the platform at [Swetrix](https://swetrix.com).
