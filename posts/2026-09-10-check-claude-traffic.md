---
title: "How to Check Claude Traffic in Swetrix and GA4"
intro: "Learn how to check website traffic from Claude, find AI referrals in Swetrix or GA4, separate crawler activity from visits, and measure conversions."
date: September 10, 2026
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "495dd32b-4354-4580-a1af-7455d4cbfcc7"
---

To check Claude traffic, open Swetrix and inspect your Traffic dashboard. Select your target date range, then review **Source / Medium** and **Referrers** for Claude- or Anthropic-related values. Once you locate the source, use the **SEO → AI Referrals** panel for a high-level view alongside search performance, then filter your sessions, goals, and funnels to see what those visitors did after they arrived.

If no Claude row appears in Google Analytics 4 under **Reports → Acquisition → Traffic acquisition**, check your **Direct / None** metrics and review your server logs separately, because a missing browser referrer can leave the session without a named source even when the analytics script still records it. Meanwhile, automated requests from Anthropic’s crawler bots represent different network activity.

To see this data, verify your tracking setup. You need an active analytics script on the target landing pages, appropriate access permissions for your property or project, a date range that includes expected AI assistant usage, and configured events or goals to measure actions beyond a simple pageview.

![An editorial scene of a marketer tracing a Claude citation to a browser landing page, with separate paths for an unattributed visit and an automated server request; no readable text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/495dd32b-4354-4580-a1af-7455d4cbfcc7/1-7c666487699e.webp)

## Start With the Right Claude Traffic Signal

Artificial intelligence platforms generate multiple signals that look like website visits, so you need to isolate exactly what you want to measure before opening a dashboard.

Site analytics are designed to measure browser sessions and events. According to Anthropic, [Claude web search responses include citations and source links](https://support.claude.com/en/articles/10684626-enable-and-use-web-search). For analytics purposes, that documentation confirms the presence of those links but does not specify how visits from them are recorded or whether referrer data is preserved.

That interaction differs from a citation without a click. If the language model mentions your brand or cites a page but the user never follows the link, ordinary website analytics cannot record the event because the analytics platform measures visits rather than citation impressions.

Unattributed traffic forms a third category. Analytics tools commonly group visits without a recognizable referrer or campaign tag into a bucket called Direct or None. This classification does not prove the visitor typed your URL manually, because it can also reflect missing source data when the session started.

Finally, server requests from automated agents require separate measurement. Anthropic uses distinct bots to collect public web content for model development, retrieve content for user-directed Claude requests, and improve search-result quality. Since a bot request is not evidence of a human website session, counting crawler activity with referral sessions can inflate traffic totals and distort conversion metrics.

## Check Claude Traffic in Swetrix

We designed Swetrix to be a privacy-first Google Analytics alternative that simplifies source tracking without forcing you to abandon conversion funnels or technical insights. Our Traffic Analytics dashboard combines visitor trends with referrers, campaigns, and session-level drill-downs.

### 1. Inspect Source and Medium

Open your Swetrix project, select a date range, and navigate to the **Traffic** dashboard. Scroll to the traffic-source breakdown and search the table for values containing `claude`, `anthropic`, or another related hostname you noticed in your project data.

Look for the actual value your project receives rather than assuming every visit carries a universal label. Swetrix [recognizes browser-reported referrers as primary sources](https://swetrix.com/docs/traffic-sources) whenever they are available, grouping recognized platforms into broader categories and listing unrecognized sources under their raw hostnames. Any UTM parameters applied to the link will override other source inference.

### 2. Check Referrers for Raw Data

Source and medium breakdowns excel at grouped acquisition reporting, but when analytics normalization makes the displayed source unclear, the Referrers view can expose the external hostname behind the grouped label.

If you do not see a clean "Claude" row in the source list, click over to the Referrers tab to look for an Anthropic-related hostname before relying on a predefined source category.

### 3. Filter Sessions and Save a Segment

Once you identify the exact source or referrer value, click the filter icon and configure it to match your identified Claude source. Applying this filter to the entire dashboard lets you review the resulting metrics:

*   **Entry pages:** Which specific articles or tools receive AI citations?
*   **Pageviews and duration:** Do users read the landing page and continue navigating, or immediately leave?
*   **Custom events:** Are visitors clicking specific buttons or interacting with dynamic elements?
*   **Errors:** Do Claude visitors trigger frontend errors upon arrival?

Save this filtered view as a reusable segment named "Claude / AI Referrals" to speed up monthly reporting and client dashboards. If your project also receives traffic from ChatGPT or Perplexity, build separate segments for each assistant rather than combining them immediately, because granular segments reveal which AI platforms drive growth.

### 4. Confirm the Pattern in AI Referrals

If you connected Google Search Console to your project, open the **SEO** dashboard. This section combines search visibility data with referral analytics and includes an AI Referrals panel. Use this panel as a high-level cross-check to understand your AI traffic alongside traditional organic search performance, while keeping the Traffic dashboard as your primary tool for investigating individual sessions, pages, and conversion events.

![A privacy-focused analytics workspace on a laptop where a highlighted Claude segment flows from an entry page through a signup funnel; no readable text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/495dd32b-4354-4580-a1af-7455d4cbfcc7/2-a1ebaba3a358.webp)

## Check Claude Traffic in GA4

Google Analytics 4 requires a different workflow because it separates user acquisition from session acquisition. The User acquisition report focuses on the source that first acquired a user, while the Traffic acquisition report shows where new and returning users came from.

### 1. Open Traffic Acquisition

Navigate to **Reports → Acquisition → Traffic acquisition**. Google's [Traffic acquisition report](https://support.google.com/analytics/answer/12923437?co=GENIE.Platform%3DDesktop&hl=en) helps you understand where your website and app visitors come from, and its Session source / medium dimension describes the source and medium associated with a new session. That supports session attribution, but the documentation does not identify the dimension as a measure of link clicks.

### 2. Apply Session Source / Medium

Change the primary dimension in the table from the default channel grouping to **Session source / medium**, then search the resulting table for Claude or Anthropic values. The exact string depends on how the visitor's browser passed the data and whether the URL contained UTM tags. If you find matching rows, note the specific string GA4 recorded.

### 3. Create an AI Assistants Channel

For recurring reports, configure a custom channel group by navigating to the GA4 admin panel and creating a new channel named "AI assistants." Set the matching rule to evaluate the relevant source dimensions for the values you found. A practical condition uses regex to match `claude|anthropic` in the session source.

Because GA4 assigns traffic to the first matching channel in your ruleset, place your new AI assistants channel above broad "Referral" or other catch-all rules. Custom channel groups reorganize recorded traffic-source values for reporting, so keep the raw Session source / medium dimension available when you need to audit the underlying attribution.

### 4. Compare With Direct / None

If your expected Claude rows remain empty or show fewer visits than anticipated, inspect the `(direct) / (none)` row. GA4 uses this bucket when it cannot determine how a user arrived, so a rise in Direct / None during a period of heavy Claude citation may indicate lost referrer data, but it cannot prove that the visits came from Claude.

## Troubleshoot Missing or Misclassified Traffic

Do not assume every Claude visit will appear as a tidy `claude.ai/referral` label, because the exact value depends on the link structure, browser configuration, redirect chains, and platform normalization. When traffic seems missing, several technical barriers typically explain the gap.

### Trace Missing Referrers and Redirects

Browsers control referrer transmission, and strict privacy settings or tracking protection can strip the referring domain entirely before the visitor loads your site. Without a referrer, UTM parameters, or a recognized click ID to supply another source, analytics may place the session in Direct / None.

Redirect chains create similar problems. If an AI assistant links to an HTTP version of your URL and your server forces an HTTPS redirect without passing the original referrer, source data can disappear. Similar loss can occur when a URL shortener or intermediate tracking domain drops query parameters. Check your landing URLs and redirect behavior, and preserve campaign parameters during necessary redirects, because a sudden traffic drop following a domain structure change may stem from broken redirect attribution rather than a loss of visitors.

### Account for Copy-Paste Journeys

A citation can inform a user even when the user never clicks it, so citation visibility and referral traffic are different measures. A person using Claude might read your domain name, open a new browser tab, and type the URL directly, or they might copy the link and paste it into a corporate messaging app where a colleague clicks it later.

Neither journey reliably retains Claude attribution. The manually typed URL registers as true direct traffic, while the messaging app click may register as direct or under the chat tool's referral string. Direct / None remains an attribution hypothesis rather than definitive proof of manually typed traffic, so avoid blindly relabeling all direct visits as missing AI referrals.

### Verify Landing Page Tracking

Check the specific pages Claude links to. If the assistant cites an obscure PDF, an unstyled XML file, or a landing page missing your analytics tracking snippet, you will not see the browser session unless the page fires an analytics script or the site records a server-side event. Test the cited URLs yourself to confirm the tracking payload fires correctly upon load.

![A website operations team examining server and CDN logs while distinct Anthropic bot requests remain separate from a human browser session; no readable text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/495dd32b-4354-4580-a1af-7455d4cbfcc7/3-a7977381264e.webp)

## Separate Claude Referrals From Claude Crawlers

Server requests from bots look different from human analytics sessions, but site owners frequently conflate them. Searching a server log for Anthropic activity reveals a request from an automated agent, not proof that a person visited through a referral.

### Identify Anthropic Agents in Logs

Anthropic [distinguishes between ClaudeBot, Claude-User, and Claude-SearchBot](https://support.claude.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler), assigning each a specific purpose.

| Agent Name | Primary Purpose | Analytics Implication |
| :--- | :--- | :--- |
| **ClaudeBot** | Collects public web content that may contribute to model training. | Automated crawling. Do not count as human traffic. |
| **Claude-User** | Retrieves websites for user-directed Claude requests. | Automated retrieval. May indicate a user-directed request, but does not prove the user clicked through to your browser. |
| **Claude-SearchBot** | Analyzes online content to improve search-result quality and relevance. | Automated search indexing. It can affect search visibility, but it is not an acquisition session. |

You can identify these requests by inspecting your server, CDN, or firewall logs for the documented agent names. Analyzing these logs helps you understand which pages Anthropic retrieves, but a matching request establishes only that an automated agent requested the page.

### Use Robots.txt for Crawl Preferences, Not Attribution

If you want to evaluate how bots interact with your site, inspect your server, CDN, or firewall logs. Use your `robots.txt` file to express your crawl preferences for these agents, not as an analytics filter.

If you block ClaudeBot to get cleaner referral data, you signal that future materials should be excluded from Anthropic's model-training datasets. Blocking Claude-SearchBot can reduce your site's visibility and accuracy in Claude search results, which may reduce potential human referral traffic. Use server or CDN controls for request handling, and measure human acquisition at the analytics level.

## Measure What Claude Visitors Do Next

Counting sessions answers only part of the question. Moving from volume measurement to outcome evaluation reveals whether AI assistants send qualified visitors or incidental clicks, so group your Claude segment by entry page to evaluate the subsequent actions.

### Build Goals and Funnels in Swetrix

Connecting an AI referral to a business outcome requires configured goals. Swetrix goals can use pageviews, custom events, or multi-condition rules involving referrer, campaign, device, and country.

If your site publishes research, configure a goal for newsletter signups and filter it by your Claude segment. For software teams, build a conversion funnel that maps the entire customer journey. A SaaS funnel might include:

1.  Claude referral lands on a feature explanation page.
2.  Visitor navigates to the pricing page.
3.  Visitor submits the registration form.
4.  Visitor triggers a product activation custom event.

Funnels track page and custom-event steps, highlighting where AI-referred traffic abandons the sequence. You can compare the Claude funnel completion rate against organic search, social media, and partner referrals over the same date range to determine which channel drives revenue.

### Provide Insights to Clients and Users

Because agencies and B2B platforms handle analytics differently from standalone bloggers, save the Claude conversion view and export the filtered dashboard if you manage reporting for clients.

For platforms embedding analytics into customer-facing admin panels, Swetrix supports dashboard integration and provides a Statistics API. This architecture allows B2B2B teams to query AI referral data and present it natively within their own software interfaces, giving end users visibility into AI-driven acquisition without forcing them to configure complex GA4 properties.

### Use Session Replays with Care

When a Claude-referred visitor navigates deep into a funnel but fails to convert, session replays show what happened before the drop-off. You can observe erratic scrolling, missed buttons, or form validation errors that quantitative metrics hide.

Identify the underperforming Claude segment first using standard traffic analysis, then enable session replay only where needed. Swetrix provides replays on Cloud instances and requires an explicit start call in the tracking code. Always apply appropriate masking settings to prevent the collection of sensitive user input, ensuring your deep behavioral analysis remains compliant with data protection standards.

## Claude Traffic Questions, Answered

### Why Can’t I Find Claude in Analytics?

The referral data might be missing because the user's browser blocked the referrer header, a redirect stripped the attribution, or the user manually copied and pasted the URL. A citation in an AI interface does not create a session until a human clicks the link. Ensure you are searching for the exact source label your property receives rather than a presumed default value.

### Is claude.ai/referral Universal, and Does ClaudeBot Count?

No universal source value guarantees capture of all Claude traffic, as the recorded string fluctuates based on browser behavior, redirect paths, and analytics normalization. ClaudeBot should not count as referral traffic because it operates as an automated crawler for model training. Anthropic identifies its crawlers distinctly in server logs, meaning you must separate this bot activity from human browser sessions.

### Can I Track Claude Referrals Without Cookies?

Yes. Source tracking, measuring conversions, and analyzing funnels can work without advertising cookies. Configured goals and custom events can attribute actions to a referring session, while the exact data collected depends on your analytics setup and applicable privacy requirements.

---

Stop guessing whether artificial intelligence platforms are driving growth. See Claude, AI, search, and referral traffic in one dashboard built for privacy and speed. Transitioning to a cookieless platform does not mean sacrificing advanced product analytics. You can track conversion funnels, monitor technical SEO performance, and identify custom events with a privacy-focused setup, while consent requirements depend on your implementation and audience. [Start analyzing your traffic with Swetrix today](https://swetrix.com).
