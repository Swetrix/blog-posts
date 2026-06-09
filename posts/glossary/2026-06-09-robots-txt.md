---
title: "What is Robots.txt? Definition and SEO Analytics Meaning"
date: June 09, 2026
standalone: true
intro: "Robots.txt is a file that tells search engine crawlers which parts of a website they can or cannot crawl. Learn how it affects SEO, indexing, and analytics."
---

Robots.txt is a text file placed at the root of a website to give crawling instructions to search engine bots and other web crawlers. It is usually available at a URL like example.com/robots.txt.

Robots.txt does not directly track visitors, but it matters for SEO and search analytics because it can affect whether search engines can crawl important pages.

## What robots.txt does

Robots.txt can allow or disallow crawlers from requesting certain paths. A simple example:

```txt
User-agent: *
Disallow: /admin/
Allow: /
```

This tells crawlers not to request pages under /admin/ while allowing other paths.

## Robots.txt and indexing

Robots.txt controls crawling, not indexing. A page blocked by robots.txt may still appear in search results if search engines discover it through links, but they may not be able to read its content. If you need to keep a page out of search results, use appropriate noindex controls rather than relying only on robots.txt.

## Why robots.txt matters for analytics

Robots.txt can affect organic search visibility. If important pages are blocked, search engines may not crawl or rank them properly. This can reduce organic search traffic and make search analytics reports look worse than expected.

Analytics teams should check robots.txt during SEO audits, migrations, redesigns, and sudden organic traffic drops.

Swetrix offers free SEO and technical tools, plus search analytics features, to help teams debug crawlability and understand organic traffic performance.

Related terms: [search engine optimization](https://swetrix.com/glossary/search-engine-optimization), [search analytics](https://swetrix.com/glossary/search-analytics), [organic search](https://swetrix.com/glossary/organic-search), and [landing page](https://swetrix.com/glossary/landing-page).

::CTA:TIME_TO_SWITCH::
