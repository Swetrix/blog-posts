---
title: "How To Check If AI Bots Are Crawling My Site"
intro: "Learn how to find AI crawlers in server logs, verify bot identities, check robots.txt, and measure AI referrals with Swetrix."
date: August 24, 2026
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "3a9fad0a-2cf1-41ed-a33e-fc2de9ecaeec"
---

Check your web server, CDN, WAF, or reverse-proxy access logs for AI crawler user-agent tokens such as `GPTBot`, `OAI-SearchBot`, `ClaudeBot`, `Claude-SearchBot`, and `PerplexityBot`. Record the request path, timestamp, response status, response size, and source IP. Then compare the request with your `robots.txt` rules and verify the IP against the crawler operator’s published lists. 

If you want to know how to check if AI bots are crawling your site, look past your browser analytics. A standard browser tag cannot prove an AI crawler visited because many training bots request plain HTML without executing page JavaScript, so you need server logs to show what actually requested your site.

This distinction requires a workflow that covers both the technical request and the human impact. Start by using Swetrix’s [AI search LLM crawlability checker](https://swetrix.com/tools/ai-search-llm-crawlability-checker) to verify that your important pages are properly configured for discovery. Next, use your server or CDN logs as the hard evidence of incoming bot requests before relying on Swetrix’s privacy-first analytics to measure the human AI referrals, events, and conversions that result from that crawlability.

## Step 0: Prepare Your Bot List and Log Access

Before running a single log query, define exactly what you are looking for. Training crawlers, AI search crawlers, and user-triggered fetchers serve different functions and should not be treated as interchangeable.

Classify the crawlers into categories. A [2026 Cloudflare bot reference](https://developers.cloudflare.com/ai-crawl-control/reference/bots/) lists a selection of crawlers from major AI operators and assigns them categories, so use the user-agent names most relevant to your logs for the initial search. Match the listed user-agent name rather than hard-coding a longer string.

| Operator | User-agent token | General purpose |
|---|---|---|
| OpenAI | `GPTBot` | Training-related crawling |
| OpenAI | `OAI-SearchBot` | ChatGPT Search discovery |
| OpenAI | `ChatGPT-User` | User-triggered page retrieval |
| Anthropic | `ClaudeBot` | Training-related crawling |
| Anthropic | `Claude-SearchBot` | Claude search discovery |
| Anthropic | `Claude-User` | User-triggered page retrieval |
| Perplexity | `PerplexityBot` | AI search crawling |
| Common Crawl | `CCBot` | Public web dataset crawling |
| ByteDance | `Bytespider` | AI crawler |
| Meta | `Meta-ExternalAgent` | AI crawler |
| Google | `Google-CloudVertexBot` | AI crawler |

OpenAI documents that `ChatGPT-User` responds to specific user actions rather than operating as an automatic web crawler, meaning it does not control ChatGPT Search eligibility. Anthropic makes similar distinctions between its background crawlers and user-directed retrievals, so exclude specialized agents like `OAI-AdsBot` from this initial list unless you are validating advertising landing pages.

Next, locate your log source by identifying the hostname, retention window, and available fields. If a Content Delivery Network sits in front of your origin server, prioritize the edge logs because an origin server often only sees requests that bypassed the CDN cache. This caching can leave you blind to thousands of bot hits answered directly from the edge.

Gather the fields you need to prove the interaction: timestamp, IP, method, path, HTTP status, response size, cache status, WAF action, user-agent, and referrer. Redact IPs, authentication tokens, email addresses, and sensitive query parameters before exporting any rows or sharing examples with your team.

![A clean, minimal flow diagram showing an AI request passing through a CDN or WAF, reverse proxy, and origin logs, then branching to privacy-first human analytics; no text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/3a9fad0a-2cf1-41ed-a33e-fc2de9ecaeec/2-2e090d669a88.webp)

## Step 1: Search Server, CDN, and WAF Logs

Start your search at the layer that first saw the request. The evidence hierarchy flows from the outermost edge inward. Check CDN or edge logs first, followed by WAF or bot-management logs, reverse-proxy logs, and finally origin web-server logs. 

Searching only the origin can miss data, because a bot might request a popular article, receive a cached response from the CDN, and trigger zero activity on the origin server. Conversely, a WAF might block a malicious scraper spoofing an AI user-agent and leave a record only in the security logs.

Export or query your text logs for the specific user-agent tokens. The format will vary based on your host, but a standard Apache or Nginx access log entry looks roughly like this:

`203.0.113.50 - - [24/Aug/2026:14:32:10 +0000] "GET /guide HTTP/1.1" 200 4521 "-" "OAI-SearchBot/1.0"`

Filter your results by date and hostname, then run a quick user-agent search using the command line:

```bash
grep -hEi 'GPTBot|OAI-SearchBot|ChatGPT-User|ClaudeBot|Claude-SearchBot|Claude-User|PerplexityBot|Perplexity-User|CCBot|Bytespider|Meta-ExternalAgent|Google-CloudVertexBot' access.log
```

If your server rotates and compresses logs, search the archives directly:

```bash
zgrep -hEi 'GPTBot|OAI-SearchBot|ChatGPT-User|ClaudeBot|Claude-SearchBot|Claude-User|PerplexityBot|Perplexity-User|CCBot|Bytespider|Meta-ExternalAgent|Google-CloudVertexBot' access.log.*.gz
```

Aggregate the results by token, path, status code, and day. If you pull logs from both the CDN and the origin, deduplicate the entries using a unique request ID or by matching the timestamp, path, and edge IP, so one request does not look like two separate bot visits.

## Step 2: Decide What Each Hit Means

Finding a user-agent string in a log file confirms a request arrived, but it does not tell the whole story. Interpret the raw data carefully rather than treating every matching row as a successful content scrape.

| Log result | Reasonable interpretation |
|---|---|
| Named AI user-agent plus a `2xx` page response | The request reached a layer that returned a successful page response. |
| Named AI user-agent plus `3xx` | The bot hit a redirect. Inspect the complete redirect chain and final response. |
| Named AI user-agent plus `4xx`, `403`, `429`, or `5xx` | The bot attempted access, but the request was denied, rate-limited, or failed. |
| Request only for `/robots.txt` | The bot checked crawler instructions. This does not prove it fetched a content page. |
| User-agent name with an unexpected IP | Treat as unverified spoofing until the source is checked. |
| AI referral in analytics without crawler hits | A human arrived from an existing index, cached result, or user-triggered retrieval. |

A named AI user-agent combined with a `200 OK` response proves only that your server delivered the file, not that the bot parsed the text, retained the data, indexed the URL, or used the content for model training. 

Some OpenAI crawler requests include a distinct `robots.txt` marker in the user-agent string to help you differentiate policy checks from resource fetches, but always inspect the requested path column to see exactly what the bot asked for.

Keep crawler requests strictly separated from AI referrals. When a visitor arrives with a referrer like `chatgpt.com`, `claude.ai`, or `perplexity.ai`, a human clicked a link in an AI interface. These referrals belong in your analytics dashboard while bot requests belong in your access logs, and neither signal proves the other occurred on the same day.

This separation is where Swetrix connects the technical data to human behavior. After you identify crawler activity in your server logs, use Swetrix's privacy-first referral reporting, custom event tracking, and conversion funnels to understand what those AI platform visitors do once they land on your site.

## Step 3: Verify the Crawler’s Identity

A user-agent is a declaration rather than authentication, meaning anyone writing a web scraping script can type "GPTBot" into the header. Do not trust a log entry until you validate the source IP.

Use the relevant log entry to identify the source IP, then compare it with the operator’s published information. OpenAI, for example, [publishes IP addresses](https://platform.openai.com/docs/bots) for several of its bots.

Perform a reverse DNS lookup to check the claimed network origin. Forward-resolve the resulting hostname and compare the final IP with the one in your log. 

```bash
dig -x 203.0.113.10 +short
```

Compare the result with official documentation. Anthropic provides a dedicated source-IP list for its bots. Treat a mismatch as suspicious, but rule out IPv6 formatting differences, undocumented proxying, or recent provider documentation changes before implementing an automatic, network-wide block. 

To see how easily a user-agent is faked, test your own server access rules without trusting the declared string:

```bash
curl -sS -D - -o /dev/null -A 'OAI-SearchBot' https://example.com/article
```

The `-A` flag changes the declared user-agent to `OAI-SearchBot` to test whether your WAF or server routing treats the bot differently than a standard browser. This command does not reproduce the provider’s actual source IP, proving only how your infrastructure reacts to the string rather than whether the genuine crawler can read the page.

![An editorial split-screen scene of a developer comparing a robots.txt file with a terminal access-log entry, emphasizing declared policy versus observed request; no text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/3a9fad0a-2cf1-41ed-a33e-fc2de9ecaeec/1-9c80d5a138be.webp)

## Step 4: Check robots.txt, noindex, and Edge Rules

Crawler behavior relies heavily on your stated preferences, so check the effective policy you are broadcasting to the internet by fetching the file exactly as a bot sees it:

```bash
curl -sS -D - https://example.com/robots.txt
```

Confirm the request returns a successful response for the correct hostname and subdomain. Review the independent `User-agent` groups, ensuring `Allow` and `Disallow` rules cover the intended paths. Look closely for redirects, authentication walls, or WAF rules that might alter the response before it reaches the bot.

Write independent policy groups to achieve specific goals. Because OpenAI separates its search discovery and training crawlers, you can permit visibility in ChatGPT Search while denying background data collection:

```text
# OpenAI search discovery
User-agent: OAI-SearchBot
Allow: /

# OpenAI training crawler
User-agent: GPTBot
Disallow: /

# Anthropic search discovery
User-agent: Claude-SearchBot
Allow: /

# Anthropic training crawler
User-agent: ClaudeBot
Disallow: /
```

Treat `Claude-User` as a separate policy decision if you want to permit user-triggered retrieval. Anthropic [documents support for the non-standard Crawl-delay directive](https://support.anthropic.com/en/articles/8896518-does-anthropic-crawl-data-from-the-web-and-how-can-site-owners-block-the-crawler) to limit crawling activity, but recognize this as a provider-specific feature rather than a universal standard.

A `robots.txt` file is voluntary and manages crawler access, but a disallowed URL can still be discovered if linked elsewhere, and non-compliant scrapers will ignore the file entirely. 

Similarly, do not confuse `noindex` with a crawler block. Google Search Central [explicitly states that `noindex` robots meta directives](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag) and `X-Robots-Tag` `noindex` instructions are honored only if the crawler can access and read the page, so a `robots.txt` block prevents the crawler from finding and applying them.

## Step 5: Troubleshoot Results and Choose Your Next Action

Analyzing bot traffic rarely yields a perfect match on the first attempt. Common false positives and missing evidence can derail an investigation if you misinterpret the signals.

If you find zero matching hits in your logs, investigate the pipeline by checking whether log rotation deleted the records, confirming you queried the correct CDN zone, and looking for WAF rules that dropped the requests. Bots change user-agent tokens or may not have revisited the site recently, so never conclude a crawl failed just because a browser analytics event is missing.

If your logs only show requests for `/robots.txt`, the bot requested the rules but did not fetch a content page.

When an AI referral appears in your analytics without a corresponding recent crawler hit, the platform likely served the human visitor from an existing index, a cached result, or a manual retrieval action. Referral data identifies the source of the human traffic, but it cannot reverse-engineer the exact timestamp of the background crawl that made the link available.

Do not rely on the Google Search Console Crawl Stats report for this task. That report tracks Googlebot. [Combine Search Console and Analytics data](https://swetrix.com/blog/combine-search-console-and-analytics-data) in Swetrix to monitor Google's performance, but keep AI crawler monitoring focused on your raw server logs.

### Build an Ongoing Monitoring Workflow

Make crawlability checks and human measurement a routine process using Swetrix by running the AI search LLM checker on high-value URLs to prevent configuration mistakes. You can also use Swetrix's [SEO migration redirect validator](https://swetrix.com/tools/seo-migration-redirect-validator) to ensure bots find clean paths rather than stalling in redirect loops.

Deploy server-side performance monitors to catch slow responses or infrastructure strain that causes automated requests to time out. Bots abandon slow connections, meaning a sluggish server often looks like a blocked crawl in the access logs until you fix the performance.

Keep your focus on what visitors do after the bot does its job. If the goal is [identifying zombie pages in SEO](https://swetrix.com/blog/identify-zombie-pages-seo), use Swetrix's funnels, session replays, and error monitoring to see if human traffic arriving from AI platforms actually engages with the content, or if they bounce due to broken layouts.

### Make the Policy Decision

Apply the log results to a clear business framework:

*   **To appear in AI search results:** Allow documented search crawlers (`OAI-SearchBot`, `Claude-SearchBot`) in `robots.txt` and ensure the WAF permits their IP ranges.
*   **To opt out of training crawlers:** Use provider-specific rules (`Disallow: /` for `GPTBot` and `ClaudeBot`) while leaving search agents untouched. 
*   **To reduce server load:** Implement WAF rate limits or IP-based connection dropping.
*   **To protect private content:** Require strict authentication. `robots.txt` is not a firewall.
*   **To measure business value:** Track AI referral conversions in Swetrix.

### Frequently Asked Questions

**Can Google Analytics tell me whether AI bots are crawling my site?**
No. Browser analytics measure human visits and require JavaScript execution. Use server, CDN, WAF, or reverse-proxy logs to record automated requests reliably.

**Does robots.txt prove that an AI bot is not crawling my site?**
No. It records your preferred policy. Official guidelines describe `robots.txt` as a preference mechanism rather than a security boundary, meaning compliant bots follow it while malicious scrapers ignore it.

**What is the difference between GPTBot and OAI-SearchBot?**
OpenAI operates `GPTBot` to crawl content for model training, but uses `OAI-SearchBot` specifically to surface websites in ChatGPT Search, requiring you to manage them independently in your site rules.

**Does seeing GPTBot in my logs mean my content was used to train a model?**
No. A log entry confirms a request identifying itself as GPTBot reached your server, but it does not authenticate the request, prove the content was successfully parsed, or guarantee the data was retained for training.

**Is ChatGPT-User an automatic crawler?**
OpenAI documents `ChatGPT-User` as an agent for user-triggered actions. It does not perform automatic background crawling or determine search eligibility.

**How long does a robots.txt change take to affect ChatGPT Search?**
OpenAI’s crawler documentation sets an expectation of approximately 24 hours for its Search systems to reflect a `robots.txt` update, though propagation timing varies by provider and crawl frequency.

**Can Swetrix show whether AI bots are crawling my site?**
Swetrix provides the crawlability preflight and the human analytics layer. Use its AI search LLM checker to test configuration, review your server logs to confirm the raw automated requests, and use Swetrix analytics to track the resulting human referrals and conversions. 

### Final Checklist

Bring the technical and analytical steps together:

1. Identify the specific crawler token you need to track.
2. Locate the outermost logging layer receiving the request.
3. Capture the path, status code, and source IP from the logs.
4. Classify the request as a fetch, a redirect, or an error.
5. Verify the IP against the operator's documentation.
6. Compare your effective `robots.txt` with your edge WAF rules.
7. Measure the resulting human outcomes.

---
Check whether AI search crawlers can reach your important pages, then measure the human traffic and conversions they help generate with [Swetrix](https://swetrix.com). Keep your server logs for the bots, and use privacy-first analytics for the people.
