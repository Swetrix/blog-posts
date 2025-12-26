---
title: Swetrix Data Policy and Privacy Law Compliance
date: May 31, 2025
hidden: true
standalone: true
---

Swetrix is a privacy-focused, open-source analytics platform that helps you understand your website traffic and performance. We are committed to protecting your privacy and complying with all relevant privacy laws and regulations. This data policy explains how we collect, use, and share your data.

Swetrix Analytics does not rely on collecting any personal data or personally identifiable information (PII). We also do not use any cookies for tracking your visitors.

::TABLE_OF_CONTENTS::

> Disclaimer: The content included in this article is based on our understanding of data protection law at the time of publication. It may be subject to change. You are responsible for complying with data protection law and should seek independent advice if you require further information about the content included in this article.

## What data we collect and what do we use it for?

One of the crucial features of Swetrix is that we are completely cookieless. No cookies (or any other client side identifiers like local storage) are used for tracking. Also we do not permanently store any personal identifiable information, like IP address.

We don't track users across devices or websites - we don't know when same person visits your site from 2 different devices.

### Sessions tracking

One of the crucial requirements of any analytics platform is to track visits, not just pageviews. Tracking pageviews alone is usually not enough, since one person can visit same page dozens of times in a single session, and you will miss the insights of how many visitors you actually had on that day.

Because we don't use cookies or cookie-like identifiers, we had to come up with a way to differentiate between pageviews, while respecting your visitors privacy.

When our API receives an event from your website, we will create a temporary session identifier, if it doesn't exist yet. Every HTTP request always contains information like User-Agent or an IP address. The IP address is used transiently in memory to help generate a unique but anonymized session identifier.

To do this, we combine the following data and generate an irreversible hash:

1. Daily rotating, random salt
2. Your website (project) ID
3. Request IP address (processed in memory, not stored)
4. User-Agent string (processed in memory, not stored)

This irreversible hash (random string) is used to identify the session. The raw IP address or User-Agent string are never stored in our database or anywhere else.

Then we generate a random number (64 bit unsigned integer) and store it together with the session identifier in RAM.

So, every time a new request is received, we check if the session identifier is already in our RAM, or generate a new one.

The data stored in RAM looks approximately like the following:

```
ab6a9d9be879724fb0a64ff67181d1521e465885b287704dc5ba6ddeba506f3b -> 123456789000102

56baf823e62d01c9c6eb1347104af6899892df4bb37d03e01b3bb86d8a876c15 -> 443949128

c43ee3c347c82d2493a8089d05609c0113301b7f7e3e4776d2969c23940dc878 -> 11119592020000
```

Only the randomly generated number is stored in the database to link pageviews to the same session. It is impossible to reverse that number to get the session identifier, the IP address or identify an individual, since it was generated randomly. Not even the hash itself is stored in the database.

The salt to generate the hash is generated randomly every day. This means a visitor on Monday and Tuesday will appear as two distinct visitors, preventing long-term tracking. We cannot show user retention statistics because they rely on a persistent session identifier, like cookies.

### User Profiles

We also support User Profiles tracking. Profiles work similarly to sessions and use the same irreversible hash mechanism. The main difference is that while the sessions salt is reset daily, the profiles salt is reset monthly.

You can also pass your own `profileIds` to the API if you want to identify your users (e.g. by using your internal user IDs or storing this data in cookies). However, if you choose to do this, you are required to obtain consent from your users.

### Page views

For pageviews, we store the following data:

1. **Randomly generated number** - to help count pageviews for the same visitor
2. **Your website (project) ID** - to understand which website the pageview belongs to
3. **Host and page URL** - to understand which page was viewed. For example, if you visit `https://example.com/page1` and Swetrix is set up on this website, we will store the host (example.com), the page URL (/page1). We will also store UTM parameters (parameters like **ref**, **source**, **utm_source**, **utm_campaign**, **utm_medium**, **utm_term**, **utm_content**).

Optionally, if you can also enable hash-based tracking (so we will store the hash of the page URL visited, like `/#page1`) or search-based tracking (so we will store the query parameters, like `/?search`) if your website uses such kind of routing.

4. **HTTP referrer** - to understand where the visitor came from (for example, _https://google.com_).
5. **Browser** - to understand which browser the visitor used (for example, _Firefox 139.0_).
6. **Operating system** - to understand which operating system the visitor used (for example, _macOS 10.15_).
7. **Device type** - to understand what kind of device the visitor used (for example, _desktop_ or _mobile_).
8. **Country, region, city** - we approximate this information based on the IP address of the visitor. We do not track anything more specific than the city-level data and do not store IP addresses in our database or logs.
9. **Language / locale** - to understand which language the visitor used (for example, _en-US_).
10. **(Optional) Metadata** - you can also supply additional metadata to the API. For example, if you use Swetrix for your blog, you can pass the author or the category of the post to help you better understand your content. Make sure to never pass any personal data to the API.

You can also optionally supply custom events (like _sign up_ or _purchase_) to the API.

It is crucial that you, as the website operator, ensure that no personal data, PII or PHI is passed to Swetrix through URLs, custom events, metadata, or any other data fields you configure or transmit. You are responsible for the data you send to our service.

### Website performance

When collecting pageviews information, we also store data of how fast the page was loaded to help you identify any performance issues.

This is calculated based on the [Web Performance API](https://developer.mozilla.org/en-US/docs/Web/API/Performance) and includes the following metrics:

- **DNS resolution time** - how long it took to resolve the domain name to an IP address.
- **TLS setup** - how long it took to establish a secure connection to the server.
- **Connection time** - how long it took to establish a connection with the server.
- **Response time** - how long it took to download the content from the server after the connection was established.
- **Browser rendering time** - how long it took to render the HTML content of the page.
- **DOM content load** - how long it took to load the DOM content of the page.
- **Page load** - how long it took to load the entire page.
- **Time to first byte** - how long it took to receive the first byte of the response from the server.

These metrics are then aggregated to help you understand overall performance of your website, frontend or backend.

### Error tracking

Optionally, you can also enable error tracking to help you identify any technical issues with your website.

When enabled, and an error occurs on your website, we will store the following data:

1. **Randomly generated number** - to help count errors for the same visitor
2. **Your website (project) ID** - to understand which website the error belongs to
3. **Host and page URL**, **Device type**, **Country, region, city**, **Language / locale**, **Browser**, **Operating system**.
4. **Error name**, **Error message**, **Error stack trace**, **Line number**, **Column number**, **Error type**.
5. **(Optional) Metadata** - that you can supply to the API when tracking errors manually.

## GDPR compliance

Swetrix Ltd. is a company [registered in the United Kingdom](/imprint), which is recognised by the EU as a country with an [adequate level](https://commission.europa.eu/law/law-topic/data-protection/international-dimension-data-protection/adequacy-decisions_en) of data protection. All the analytics data collected is stored on Hetzner servers in the EU (Germany).

Under GDPR, "personal data" is any information relating to an identified or identifiable natural person. For example, an email address, home address, or a raw IP address can likely be linked back to a real person. While an IP address can be considered personal data and is used to help generate an irreversible session identifier, it is not stored on disk or in our database and such identifier cannot be reversed to get the IP address or identify an individual.

Swetrix does not store any personal data from end users (your website visitors) and does not use cookies or any other client side identifiers for tracking.

It is also possible that GDPR might not even apply to our use case (see [Recital 26 - Not Applicable to Anonymous Data](https://gdpr-info.eu/recitals/no-26/)) since the data stored is anonymous. Even if it does apply, we believe that using Swetrix for website analytics or error tracking is a legitimate interest of you, as a website operator, under the GDPR, to understand how your website performs or identify any technical issues.

## PECR compliance

PECR are UK-based regulations that work in conjunction with the GDPR. PECR specifically covers electronic marketing communications, the use of cookies and similar technologies, and the security of public electronic communications services.

Swetrix is designed to be compliant with PECR by default:

- **No Cookies or Similar Technologies:** As Swetrix does not use cookies or any client-side identifiers (like local storage) for tracking visitors, the PECR rules regarding consent for cookies do not apply.
- **No Electronic Marketing Data:** Swetrix does not collect data for electronic marketing purposes from your website visitors.
- **Data Security:** We are committed to the security of the data we process, as detailed in other sections of this policy.

## CCPA compliance

Swetrix is designed to be compliant with the California Consumer Privacy Act (CCPA) by default. Swetrix does not collect personal information as defined by the CCPA. We do not gather data that can directly identify an individual, such as names, email addresses, or precise location data.

The data processed by Swetrix is anonymised and does not constitute "personal information" in a way that would typically trigger the core obligations of CCPA regarding user rights to access, delete, or opt-out of the sale of personal information, as we do not engage in such activities with the data we process.

## HIPAA compliance

Swetrix does not collect any personally identifiable information (PII) from end users. We are not a subject to HIPAA because we do not collect, store, or transmit Protected Health Information (PHI), neither do we use cookies or any other client side identifiers for tracking.

Swetrix focuses on general website traffic and performance metrics, such as page views, browser types, operating systems. This type of data is not considered PHI under HIPAA. Consequently, a Business Associate Agreement (BAA) is not necessary when using Swetrix, as we do not handle PHI on your behalf.

## Data ownership

When using Swetrix, you own your data. When using our Cloud offering, even though your data is stored on our servers, you are in full control of it. Our business model is based on selling software, not your data.

- We do not sell or share your data with any third parties.
- We don't use your data for any other purpose than to provide you with the service.
- We do not monetise your data.

You can delete your website data or your whole account at any time. The data is deleted permanently and irreversibly.
