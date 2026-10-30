---
title: "Referrer Policy: What It Shares and Why"
intro: "Learn what a referrer policy sends, how it shapes referral analytics, which setting to choose, and how to configure and test it."
date: October 30, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/7f57e683-65d0-4f2a-9751-c5cbf7e1a64c/0-4edeccc68f1c.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "7f57e683-65d0-4f2a-9751-c5cbf7e1a64c"
---

A referrer policy controls how much information a browser includes in the `Referer` header on requests from a web page, including link navigations and resource requests. A website can set a policy with an HTTP response header, while HTML can set document-wide or element-specific rules. The system relies on two related components: the browser uses an HTTP request header called `Referer` (historically misspelled) to report the source URL, while the source website uses an HTTP response header called `Referrer-Policy` to control what the browser sends with that request. Because this mechanism governs outbound data sharing, a destination website cannot use its own policy to demand more information from the site that sent the visitor. The browser applies the policy associated with the source page and request, subject to browser settings and more specific request controls.

Google Analytics 4 uses `page_referrer` to identify which referral source brought traffic to a page, while campaign fields can also identify traffic sources. The field defaults to the user's [`document.referrer` value](https://developers.google.com/analytics/devguides/collection/ga4/reference/config).

The referrer policy controls what the browser shares, while an analytics platform works with the signals that arrive. Swetrix bridges simple, privacy-compliant tracking with advanced product analytics, so moving from Google Analytics to a cookieless solution doesn't mean giving up essential growth data such as conversion funnels, session replays, or technical SEO insights.

## Data Shared by the Default Referrer Policy

When no policy is otherwise specified, the W3C specification sets `strict-origin-when-cross-origin` as the default and permits user agents to reduce referrer details to minimize data leakage. Under this policy, same-origin requests send the full sanitized URL, while cross-origin requests send only the source origin unless a potentially trustworthy source is sent to a non-potentially-trustworthy destination, in which case the `Referer` header is omitted, as in an HTTPS-to-HTTP request. The [W3C Referrer Policy specification](https://w3c.github.io/webappsec-referrer-policy/) defines these behaviors.

![A left-to-right browser request diagram showing a full URL kept for a same-origin request, reduced to an origin for cross-origin HTTPS, and omitted for an HTTP destination; label the routes "Same origin," "Cross-origin HTTPS," and "HTTPS to HTTP."](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/7f57e683-65d0-4f2a-9751-c5cbf7e1a64c/1-1c404f8600dd.webp)

First, a same-origin request receives the most detail. Under this default, when a user navigates between pages on the same HTTPS origin, the browser sends the full sanitized source URL. For example, if someone clicks from `https://blog.example/guide?utm_source=newsletter` to a homepage on the same origin, the `Referer` value includes the `/guide` path and query parameters. The browser strips URL fragments such as `#section-two` and embedded credentials before sending the referrer, while the path and query remain available to same-origin analytics.

Second, a cross-origin HTTPS request receives only the source origin. When that same visitor clicks a link to another HTTPS site, the browser sends the source scheme, host, and port, but leaves out the page path and query. The destination can receive `https://blog.example/` as the `Referer` value. Because the source origin remains visible, the destination can attribute traffic to the referring site when the browser sends the header and its analytics platform records it.

Third, a request from an HTTPS page to an HTTP destination carries no `Referer` header under this default. This avoids sending a secure page's URL over an unencrypted connection, where a path or query string could disclose sensitive context.

## Selecting a Referrer Policy

Policies differ in how much outbound detail they share with third parties. A stricter policy reduces the referrer detail available to destination sites, while a looser policy can provide more granular referral data at the risk of exposing sensitive information.

Explicitly declaring `strict-origin-when-cross-origin` as your site-wide baseline is a practical starting point. This behavior withholds detailed paths from cross-origin HTTPS destinations while retaining source-origin visibility. Selecting a different directive requires understanding exactly how it handles same-origin routing and connection downgrades. Mozilla's [implementation guidance](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/Referrer_policy) outlines these privacy trade-offs and notes that recent Firefox and Safari versions treat `no-referrer-when-downgrade`, `origin-when-cross-origin`, and `unsafe-url` like `strict-origin-when-cross-origin`, so the table below describes specified policy behavior rather than a guarantee for every browser. The W3C [specification lists eight named policy values](https://w3c.github.io/webappsec-referrer-policy/).

| Policy | Cross-origin HTTPS destination receives | HTTPS page to HTTP destination | Practical reading |
|---|---|---|---|
| `strict-origin-when-cross-origin` | Origin only | No referrer | Current default. Origin-level referral remains, while path and query are withheld from cross-origin HTTPS requests. |
| `no-referrer` | Nothing | Nothing | Omits the `Referer` header from every request made under this policy. |
| `same-origin` | Nothing | Nothing | Sends a full referrer on same-origin requests, none to other origins. |
| `strict-origin` | Origin only | No referrer | Sends only the source origin except on HTTPS-to-HTTP downgrades. |
| `no-referrer-when-downgrade` | Full sanitized URL | No referrer | Older default. Sends the full sanitized URL to HTTPS destinations, omitting it on HTTPS-to-HTTP downgrades. |
| `origin-when-cross-origin` | Origin only | Origin | Can send an HTTPS origin over an unencrypted HTTP connection. |
| `origin` | Origin only | Origin | Always sends only the origin, including over unencrypted HTTP. |
| `unsafe-url` | Full sanitized URL | Full sanitized URL | Sends paths and query strings to any destination, including HTTP, so avoid it for general use. |

Using `no-referrer` offers the most restrictive referrer setting by omitting the `Referer` header from requests made by the page. A site publishing sensitive medical or legal content might choose it to prevent external sites from receiving the source URL in that header. As a result, partner sites won't receive referrer data from those requests, so their referrer-based reports won't attribute the visits to your domain unless you provide campaign tags or another signal.

Applying `same-origin` sends the full source URL with same-origin requests and no referrer data to other origins. You can use those same-origin referrer values to track navigation within your application, but external requests carry no `Referer` information. This can suit complex web applications that rely on query strings for internal routing while needing to withhold referrer details from outside sites.

The `strict-origin` policy sends only the source origin when the request does not involve a downgrade from HTTPS to HTTP, including on same-origin links. As a result, internal analytics can receive the origin but not the source page's path or query string. You might use this option if internal URLs contain sensitive tokens you don't want exposed through the `Referer` header.

The older `no-referrer-when-downgrade` policy sends the full sanitized URL to HTTPS destinations, omitting it on a downgrade from HTTPS to HTTP. That can reveal the referring page's path and query to a secure third-party site, so use it only when that level of disclosure is intentional.

Policies like `origin-when-cross-origin`, `origin`, and `unsafe-url` can send some or all referrer information to HTTP destinations, depending on the directive. The `unsafe-url` directive sends the full sanitized URL to any destination, including unencrypted HTTP. Avoid it for general use. If a source URL contains a password-reset token, email address, or API key in its query string, this policy can send that value in the `Referer` header to an HTTP destination, where it travels without encryption.

## Handling Missing Referral Data

Destination websites operate downstream of the referrer policy, processing the data the browser sends when a visitor arrives. A server configuration cannot reach back and request a complete source path from a browser that withheld it. So when referral reports look empty, the cause can be the source site's sharing rules, browser privacy settings, or the environment where the click originated.

### Visits Classified as Direct or None

In Google Analytics, a [session is processed as direct traffic when no information about the referral source is available or when the referring source or search term is configured to be ignored](https://support.google.com/analytics/answer/11242841?hl=en), so a Direct classification doesn't prove that a visitor typed the domain into the address bar. A click can therefore appear as direct when Analytics receives no referral-source information.

A user clicking a link from an email client, desktop application, or encrypted messaging service may arrive without a web referrer. If that source app has no web URL to report, the browser can send the request without a `Referer` header. Similarly, if the originating website applies a `no-referrer` policy, the destination receives no referrer value. Some browsers or privacy extensions may also reduce or omit referrer details independently of the site's policy.

Swetrix handles these scenarios by processing the [available signals](https://swetrix.com/docs/traffic-sources) rather than attempting to guess origin paths. Running a privacy-first [cookieless Google Analytics alternative](https://swetrix.com/google-analytics-alternative) means relying on factual browser metadata. Reviewing post-arrival funnel behavior provides better growth insights than chasing unrecoverable acquisition data, letting you measure what users do once they arrive even if the browser concealed their exact route. This complements the broader growth picture, including conversion funnels, session replays, and technical SEO insights, without relying on referrer paths the browser never sent.

![A content marketer preparing a partner or newsletter link with campaign tags while reviewing a generic referral and conversion report, with no readable interface text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/7f57e683-65d0-4f2a-9751-c5cbf7e1a64c/2-03e3e2668146.webp)

### Tag Planned Campaigns with UTM Parameters

You can label planned promotional efforts with UTM parameters attached to the destination URL. Google Analytics reads [custom campaign parameters embedded in the destination URL](https://support.google.com/analytics/answer/11242841?hl=en) from the document location field and maps them directly to campaign dimensions, separately from the document referrer field, so those parameters can supply campaign labels when Analytics receives and processes them.

For a newsletter sponsorship, guest post, or partner placement, you can set explicit source, medium, and campaign values in the link:

`https://example.com/landing?utm_source=partner&utm_medium=referral&utm_campaign=review`

The destination analytics system can read these query parameters on arrival if they reach the page where tracking runs. To preserve them, make sure redirects carry the UTM query string to the final URL. If a redirect strips that string, the final page won't have the campaign tags to record. Swetrix maintains separate breakdowns for Referrers, Source / Medium, and Campaigns, allowing you to distinguish organically reported domains from deliberately tagged promotional links. That keeps planned campaigns measurable when source domains restrict referrer data, provided redirects preserve the tags and the destination analytics records them.

## Setting and Testing a Referrer Policy

Implementing a clear rule requires choosing the right scope. You can set a response-header policy on the site's HTML documents, use a document-wide HTML policy, or add a more specific rule to an individual element. Explicitly declaring `strict-origin-when-cross-origin` in your HTML responses makes the intended baseline clear rather than relying on a browser default, though browser settings and element-level policies can still affect individual requests.

### Configuration Scopes

The most robust method is configuring your web server or CDN to send a `Referrer-Policy` HTTP response header with the HTML documents whose outbound requests you want to govern. A server or CDN can add that header consistently across the documents it serves, and the policy establishes the baseline for requests made from each document. A standard server configuration block outputs this exact text:

`Referrer-Policy: strict-origin-when-cross-origin`

When you lack server access, you can apply a policy at the document level with an HTML meta tag inside the `<head>` section. It applies to requests originating from that document unless an individual element or request sets a more specific policy, and it does not affect requests started outside the document, such as opening an image URL directly.

`<meta name="referrer" content="strict-origin-when-cross-origin">`

You can also override the document-level policy for individual links using the `referrerpolicy` attribute. This helps when a site generally uses the default policy but one link points to a sensitive resource and should not send the page URL to that destination.

`<a href="https://partner.example/" referrerpolicy="no-referrer">Visit partner</a>`

This element-level control also applies to iframes and images. If you embed a third-party dashboard or an admin panel, applying a suitable policy directly to its iframe can prevent the parent page's full URL from appearing in the iframe request's `Referer` header.

### Verifying Request Headers

Testing requires checking both the published configuration and the resulting network traffic. Browser developer tools show the request and response headers, letting you confirm that the selected policy works as intended.

![A website developer checking published response headers and network requests in browser tools at a desk, with interface details but no legible text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/7f57e683-65d0-4f2a-9751-c5cbf7e1a64c/3-5cd0d9c79803.webp)

1. Open the Network tab in your browser's developer tools and load your website.
2. Select the main HTML document request and inspect the Response Headers for `Referrer-Policy`. If a response includes multiple valid policy values, the W3C parsing algorithm selects the last valid value, so later values can take precedence, as the [W3C specification explains](https://w3c.github.io/webappsec-referrer-policy/).
3. Clear the network log and click a link to a different HTTPS domain, then select that new outbound request in the developer tools.
4. Under the default policy, a request to another HTTPS origin should contain only your site's origin, omitting the page path and query parameters.
5. Return to your site, clear the log, and click an internal link. Inspecting that request shows the `Referer` header displaying the full sanitized URL of the previous page, reflecting the less restrictive same-origin behavior.
6. Generate a test link containing UTM parameters and verify that the destination analytics platform records the campaign data accurately, especially if the landing page redirects before loading.
7. For embedded applications, inspect the request loading the iframe to confirm the parent page respects the assigned element-level policy.

## Common Referrer Policy Questions

Site operators often encounter confusion around what analytics platforms can control and how search engines interpret privacy directives. The core principle remains that downstream services cannot manipulate upstream data sharing.

### Hiding the Referrer Completely

Applying `no-referrer` suppresses referrer information from every request made under that policy. The default `strict-origin-when-cross-origin` setting does not hide the referrer entirely, because it still sends the source origin to secure cross-origin destinations. If the browser sends that header and the destination analytics system processes it, the source origin can still appear in referral reports.

### Restoring Missing Incoming Data

UTM parameters don't restore a missing referrer, but they can label planned campaign traffic. Google Analytics maps [custom campaign parameters embedded in the destination URL](https://support.google.com/analytics/answer/11242841?hl=en) to report dimensions when it collects and processes them.

### Referrer Policy and Search Rankings

A referrer policy controls what referrer information a browser sends with outgoing requests and navigations. The W3C [specification describes its effect on the `Referer` header](https://w3c.github.io/webappsec-referrer-policy/), so use this setting to manage referrer disclosure and assess page content and search visibility separately.

---

Transitioning to a privacy-focused analytics platform does not mean sacrificing actionable growth data. Swetrix provides detailed traffic breakdowns, conversion funnels, and session replays without relying on invasive cookies or demanding unredacted referrer paths. Its cookieless approach can also support technical SEO insights while preserving the privacy-conscious tracking described in this guide. Start tracking your campaigns accurately and ethically with [Swetrix](https://swetrix.com).
