---
title: "Why Strict-Origin-When-Cross-Origin Hides Referrer Paths"
intro: "Learn what strict-origin-when-cross-origin sends, why analytics may show a referral domain but not its page, and how UTMs preserve campaign attribution."
date: October 5, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/a34c904c-e9e7-4957-90c0-49131199782d/0-a765dae71451.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "a34c904c-e9e7-4957-90c0-49131199782d"
---

The `strict-origin-when-cross-origin` policy tells a browser to send only the source page's origin, rather than its path or query string, on secure requests to a different origin. If you check your Swetrix analytics dashboard and see a referral from a major publisher but cannot find the exact article that sent the traffic, this default browser behavior is often the cause.

## What Does `strict-origin-when-cross-origin` Mean?

The `Referrer-Policy` header dictates how much URL information a browser includes when making a request. The historical HTTP request header itself is spelled `Referer`, dropping one "r" due to an original specification typo, while the modern policy controlling it uses the standard spelling. Browsers apply `strict-origin-when-cross-origin` as the [default setting for referrer behavior](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Referrer-Policy) whenever a website does not specify its own rules.

This directive combines two distinct privacy mechanisms into a single automated rule. The first part of the rule, "strict-origin," handles the security downgrade boundary by completely withholding referral data whenever an encrypted HTTPS origin navigates to an unencrypted HTTP destination. The second part, "when-cross-origin," inspects the relationship between the origin of the current page and the origin of the target resource, preserving the complete URL path for internal requests while trimming cross-origin requests down to their base domain.

This policy balances attribution data and user privacy, producing three distinct outcomes depending on where the user clicks:

| Request from the source page | Referrer sent to the destination |
|---|---|
| Same origin (`https://example.com/article` to `https://example.com/pricing`) | Full eligible URL, including path and query string (`https://example.com/article`) |
| Cross-origin HTTPS (`https://publisher.example/article?edition=a` to `https://brand.example/landing`) | Source origin only, with path and query parameters removed (`https://publisher.example/`) |
| HTTPS to HTTP (`https://publisher.example/article` to `http://brand.example/landing`) | No referrer header sent at all |

The [Referrer Policy specification](https://w3c.github.io/webappsec-referrer-policy/) defines how a policy controls the `Referer` information sent with requests. Depending on the selected policy, a browser can omit that information or limit it to the source origin on cross-origin requests, which helps keep sensitive URL details from reaching third-party servers.

Browsers execute this evaluation locally on the client device before dispatching the network payload. When a link click triggers a navigation event, the browser compares the source origin against the target origin, checks the protocol security levels, and trims the string in memory. Because this calculation occurs before the packet leaves the network interface, the receiving server cannot bypass the restriction through server-side configuration.

![A clean three-lane request diagram traces a full same-origin referrer, an HTTPS cross-origin referrer reduced to the source origin, and an HTTPS-to-HTTP request with no referrer, using the labels "Full URL", "Origin only", and "No referrer".](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/a34c904c-e9e7-4957-90c0-49131199782d/1-250c6b39886c.webp)

## What Counts as Cross-Origin?

A web origin consists of three strict components: the scheme, the hostname, and the port. Two URLs share an origin only if all three match perfectly. Modifying the file path or query string does not change the origin, which is why navigating between two internal pages on the same domain leaves the full referrer intact. For same-origin requests, the browser sends the full eligible URL to the destination.

Changing a subdomain creates a cross-origin request, meaning a link pointing from `www.example.com` to `shop.example.com` crosses origins because the hostnames differ. Moving from a secure HTTPS connection to an unsecure HTTP connection changes the scheme. This crosses origins while triggering a security downgrade that strips the referrer. Modifying the port number operates the same way, so a local development environment running on `http://localhost:3000` is cross-origin to `http://localhost:8080`.

Many site owners assume that registered domains share an origin by default, but web security standards draw a firm line between a registrable domain and an exact origin. Even if your organization owns every subdomain under a common domain name, the browser treats each host label as an independent origin boundary. A visitor moving from your main marketing blog to your dedicated application portal at `app.example.com` undergoes the exact same cross-origin trimming as an external visit heading to an entirely separate platform.

Corporate ownership and parent domains do not influence this technical classification. The browser treats `blog.company.example` and `app.company.example` as separate entities. When a user clicks a link between those two subdomains, the browser applies the cross-origin rules and strips the file path from the HTTP request, even though the same company operates both servers.

This separation can create unexpected blind spots in your internal site architecture if you rely on referrer paths to follow user paths across multiple microservices. If your documentation portal runs on `docs.example.com` and directs readers to `pricing.example.com`, the receiving pricing page will only see `https://docs.example.com/` in the request header. You will not see which guide or tutorial prompted the pricing visit unless you handle that path data through parameters or shared session states.

## Why Analytics Shows a Domain, Not a Page

Consider a visitor clicking a link from `https://publisher.example/review/analytics?edition=a` to `https://brand.example/landing`. Both URLs use the secure HTTPS scheme, but their hostnames differ. Because the request moves from one HTTPS origin to a different HTTPS origin, the browser enforces the cross-origin restriction.

The HTTP request arrives at the destination server carrying a truncated header (`Referer: https://publisher.example/`), which your analytics platform reads to identify the referring publication. The article path and query parameter disappeared before the request left the visitor's browser. Reports show a domain-level referral because that is the maximum amount of data provided, and the destination server has no technical way to recover the missing `/review/analytics` path.

This behavior differs from visits categorized as Direct / None. Complete referrer loss happens when a request drops from HTTPS to HTTP, prompting the browser to withhold the header. Without a recognized click ID or UTM parameter on the landing URL, the analytics tool has no context and logs the visit as direct traffic. Other factors like privacy browser extensions, in-app web views, or a strict `rel="noreferrer"` link attribute can also wipe out the referrer. The strict origin policy alone preserves the domain name, keeping the visit out of the unassigned traffic bucket.

When you analyze acquisition channels in Swetrix, the platform captures this domain-level string and populates the Referrers breakdown under Traffic Analytics. Because the platform receives `https://publisher.example/`, it successfully credits that publisher as the acquisition source. The platform cannot show you whether the reader clicked a link in a sidebar, an introductory paragraph, or an updated product review, because those directory locations were stripped out before the network packet arrived.

Chrome announced plans to make `strict-origin-when-cross-origin` the [default referrer policy when a site sets no policy](https://developer.chrome.com/blog/referrer-policy-new-chrome-default/?referral=LP) to improve privacy. Under Chrome's previous `no-referrer-when-downgrade` default, cross-origin HTTPS requests included the full source URL, so sensitive data in its path or query string could reach the destination in the `Referer` header. The new policy sends only the source origin on secure cross-origin requests, which keeps the path and query out of that header while preserving origin-level attribution; on HTTPS-to-HTTP requests, it sends no referrer.

The security gains from this standard are substantial. Web applications historically exposed sensitive user states, search queries, pagination states, and unique session tokens through URL structures. By enforcing origin-level truncation across origin boundaries, modern browsers keep URL paths and query strings out of the `Referer` header on secure cross-origin requests.

## How to Preserve Placement-Level Attribution

Knowing the referring domain answers broad questions, but measuring a specific marketing campaign requires finer detail. You might sponsor three different newsletters on the same publishing platform. If all three links send the exact same domain-only referrer, you cannot attribute conversions to the winning placement, causing the analytics dashboard to group them all under a single source.

Append UTM parameters directly to the destination URL to solve this visibility gap. Instead of publishing a bare link to `https://brand.example/landing`, provide partners with a structured link like `https://brand.example/landing?utm_source=publisher&utm_medium=referral&utm_campaign=partner&utm_content=review_article`.

The browser still strips the source article's path from the HTTP header, but the destination URL carries the UTM tags into the analytics platform. Swetrix captures those parameters and displays them in the Campaigns and Source / Medium views. The platform documentation outlines how to map specific placements to downstream behavior using goal conditions. The tags identify the specific newsletter or guest post without relying on the source path, which the browser's origin restriction removes.

UTM parameters take direct precedence in attribution modeling over fallback referrer strings. When a visitor arrives with both a domain referrer and valid campaign tags, privacy-focused platforms like Swetrix use the explicit campaign parameters to categorize the session. This structure lets you separate paid guest posts, organic editorial mentions, and sidebar banners hosted on the exact same partner domain without asking the publisher to alter their server headers.

Those tags function only if the visitor's browser retains them through the navigation sequence. If a server redirects the initial landing page and drops the query string in the process, the attribution data vanishes. Verify your server routing by [testing link routing through a redirect validator](https://swetrix.com/tools/seo-migration-redirect-validator) to confirm parameters survive the jump.

Redirect configurations often drop query parameters when converting non-www URLs to www, or when enforcing trailing slashes. If an external link points to `https://brand.example/landing?utm_source=partner` and your server issues an HTTP 301 redirect to `https://brand.example/landing/` without appending the query string, the visitor reaches the final destination as an untagged referral. Testing these transition points prevents silent data loss across your active campaigns.

Relying on parameters rather than referrer strings puts attribution control in your hands. Webmasters often change their site structures, migrating articles to new subdirectories. If you depend on the exact referring page path to track a campaign, a URL change on the publisher's site breaks your tracking logic, whereas UTM parameters remain stable regardless of where the publisher places the link.

![A publisher and marketer compare distinct tagged campaign links beside a simple analytics display with no readable interface text, illustrating placement attribution without a full source-page URL.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/a34c904c-e9e7-4957-90c0-49131199782d/2-37305ce630ee.webp)

## How to Check Which Referrer the Browser Sent

You can inspect the data arriving at your server using built-in browser developer tools. Open the original source page and check its configuration, as a webmaster might declare a specific policy in the HTML `<head>` using a meta tag or configure the web server to send a custom policy header.

The `<head>` section of an HTML document can define the document-wide policy using a dedicated meta tag formatted as `<meta name="referrer" content="strict-origin-when-cross-origin">`. When present, this tag informs the browser of the parsing rules before any external scripts or assets load.

Individual links can also carry their own instructions; inspecting the specific anchor tag for a `referrerpolicy` attribute or a `rel="noreferrer"` directive reveals these element-level settings, which override broader server instructions.

For example, an anchor element written as `<a href="https://brand.example/landing" referrerpolicy="no-referrer">` forces the browser to discard all referrer information for that single link click, ignoring both the document-level meta tag and the server's default headers.

Click the link to navigate to the landing page while keeping the developer tools open to the Network panel. Selecting the "Preserve log" option before clicking prevents the browser from clearing the network history if the landing page initiates a redirect. Locate the first document request in the timeline, select it, and scroll down to the Request Headers section to find the `Referer` field. Comparing the printed value to the original source URL will show if the field contains only the domain name, indicating the browser applied an origin-only restriction.

You can also examine the Response Headers of the initial document request to locate the `Referrer-Policy` declaration returned by the originating server. If the server explicitly declared a policy, its string will appear in this section.

You can [review the HTTP headers](https://swetrix.com/tools/http-headers-checker) of the source website to confirm whether the server forces a specific policy. When no explicit policy appears in the headers or the HTML, the browser defaults to the strict origin rules. Testing the output removes the guesswork from debugging analytics anomalies.

## Is It a CORS Error, and Should You Change the Policy?

A browser restricting referrer data does not block the navigation. Network monitoring tools sometimes list the active referrer policy next to a request, leading developers to mistake it for a blocked connection. The policy dictates privacy hygiene for the HTTP header by preventing sensitive URL paths from leaking across domains, rather than handling request authorization.

Cross-Origin Resource Sharing (CORS) determines whether a script running on one origin can read data returned by a different origin. Server administrators configure CORS access using permission headers like `Access-Control-Allow-Origin`. A restricted referrer policy does not cause a CORS error, and opening the referrer policy does not grant cross-origin script access because the two mechanisms manage different security concerns. When a CORS error occurs, the browser reports a permission failure in the JavaScript console and prevents the application from reading the response, even if the request reached the server. Conversely, a strict referrer policy allows the request to complete while redacting the historical source URL during transport. Fixing a blocked API request requires adjusting the CORS configuration rather than the referrer settings.

While the two systems serve different roles, requests can include both the `Origin` and `Referer` headers. Their values follow different rules, and changing the referrer policy does not resolve an unauthorized cross-origin API block.

When you control the source website, you can override the browser default by assigning a different value in the `Referrer-Policy` HTTP response header. You can consult the [implementation guidelines for referrer policies](https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/Referrer_policy) to evaluate the available directives.

Different web applications have unique security profiles that call for specific policy configurations:

- `no-referrer`: Strips the header completely from every outgoing request, turning all outbound clicks into direct traffic for receiving servers.
- `same-origin`: Passes the complete URL path to pages on the exact same origin, but sends nothing at all to external websites or subdomains.
- `strict-origin`: Sends the origin for same-origin and cross-origin requests over HTTPS, but sends no path data and drops the header on protocol downgrades.
- `origin`: Sends the origin to all destinations, including cross-origin requests and unencrypted HTTP connections, without suppressing data during downgrades.
- `origin-when-cross-origin`: Sends the full path to internal pages and only the origin to external sites, but continues to transmit the origin during an HTTPS to HTTP downgrade.
- `no-referrer-when-downgrade`: The legacy web standard, sending the full URL across origins over HTTPS, but withholding data on HTTP downgrades.
- `unsafe-url`: Sends the full URL path and query parameters to all destinations on every request.

Setting the value to `no-referrer` strips the header from every outgoing request, turning all external clicks into direct traffic. The `same-origin` directive passes the full path to internal pages but sends nothing to external websites. Avoid applying `unsafe-url` as a quick fix for analytics reporting, as that directive forces the browser to broadcast the full URL path and query parameters across all origins, including unencrypted HTTP destinations. Exposing internal query strings to third-party servers creates severe security vulnerabilities, especially if URLs contain session tokens, password reset keys, or personally identifiable information.

Retaining the default strict origin policy keeps your visitors secure. Relying on tagged destination URLs provides clean campaign attribution without compromising privacy, and when [deploying a privacy-first web analytics alternative](https://swetrix.com/google-analytics-alternative), preserving user security takes priority over harvesting exhaustive URL strings.

---

Transitioning away from invasive tracking does not mean abandoning campaign measurement. Swetrix provides granular attribution, conversion funnels, and performance monitoring without relying on cookies or capturing personally identifiable information. The platform respects browser privacy defaults while giving you the technical SEO utilities and product analytics necessary to scale. Explore the full suite of features and start tracking ethically at [Swetrix.com](https://swetrix.com).
