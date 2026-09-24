---
title: "What Is llms.txt? Format, Examples, and Its Role in SEO"
intro: "llms.txt gives agents a concise guide to a website. Learn what belongs in it, how the proposal differs from robots.txt and sitemaps, and how to test its usefulness."
date: July 20, 2026
modified: September 24, 2026
image: "https://cdn.swetrix.com/file/bbe14bf43845651f63bb9a6ae041d209.webp"
hidden: false
author: Andrii Romasiun
twitter_handle: andrii_rom
seoTitle: "What Is llms.txt? Format, Examples, and SEO"
seoDescription: "Understand llms.txt, see a minimal example, compare it with robots.txt and XML sitemaps, and learn how to evaluate agent use without assuming ranking benefits."
---

**llms.txt is a proposed Markdown format for helping AI agents find and use information on a website.** It gives a short description of the site and links to useful resources. Think of it as a maintained reading guide, especially for documentation.

It is not an indexing request, an access-control mechanism, or a promise that an AI search engine will cite you. Build it to solve a navigation problem for agents that use it, and measure that use separately from search traffic.

_Reviewed September 24, 2026 against the current proposal and search documentation._

## What Goes in llms.txt?

The [current llms.txt proposal](https://llmstxt.org/) describes a Markdown file with a project-name H1, an optional summary, and sections linking to useful content. Its August 2026 revision also describes files under subpaths, such as `/docs/llms.txt`, and discovery links to Markdown resources.

A small illustrative file might look like this:

```markdown
# Example Product

> Documentation for a website analytics product.

## Guides

- [Installation](https://example.com/docs/install.md): Add the tracker.
- [Events](https://example.com/docs/events.md): Record product actions.
- [Troubleshooting](https://example.com/docs/troubleshooting.md): Diagnose missing events.

## Optional

- [Changelog](https://example.com/changelog.md): Version history.
```

This is a format example using a reserved example domain, not a file to publish unchanged. Substitute real, working pages from your own site. If you link to Markdown versions, make sure those versions exist and contain the same relevant facts as the human-readable documentation.

## llms.txt vs robots.txt vs sitemap.xml

| File         | Purpose                                           | What it does not do                                      |
| :----------- | :------------------------------------------------ | :------------------------------------------------------- |
| `llms.txt`   | Offer agents a concise guide to useful resources  | Force an agent to read, trust, or cite them              |
| `robots.txt` | Communicate rules to compliant automated crawlers | Protect private content or guarantee removal from search |
| XML sitemap  | Help search engines discover canonical URLs       | Guarantee crawling, indexing, or rankings                |

Use authentication for private material. Publishing a path in any public file can reveal that it exists; crawler rules are not a security boundary. Google's [robots.txt introduction](https://developers.google.com/search/docs/crawling-indexing/robots/intro) explains its limitations.

## Does llms.txt Improve SEO or AI Citations?

Do not assume that adding the file improves rankings. Google's [AI features guidance](https://developers.google.com/search/docs/appearance/ai-features) says no new AI text files or special markup are required for its AI search features.

A documentation workflow is a different use case. An agent that has been given your llms.txt file can use it to locate installation instructions or an API reference. Whether an agent discovers it automatically depends on the tool and workflow. Publishing the file proves availability, not consumption or citation.

For commercial visibility, prioritize a page that answers the buyer's question, accurate product information, and a clear next step. Our [ChatGPT visibility guide](https://swetrix.com/blog/how-to-rank-in-chatgpt) separates crawl access, content work, sampled answers, and referral measurement.

## When Is It Worth Maintaining?

It is a reasonable addition when people use agents to integrate your product, navigate a large documentation set, or find a specific reference. The cost is low if the file is generated from the same source as the docs and checked alongside them.

It is a lower priority when your important pages are broken, inaccessible, contradictory, or missing. Fix those problems first. A well-written index cannot compensate for incorrect instructions behind its links.

Before adding a full-text companion such as `llms-full.txt`, consider size and maintenance. A compact index that leads to the correct page may suit a task better than a single file containing every historical detail.

## How to Create and Test a Useful File

1. Pick a concrete task, such as installing your tracker or finding an API parameter.
2. Identify the few pages an agent needs to complete that task correctly.
3. Write a short description and link to those pages with clear labels.
4. Serve the file successfully at the chosen public path.
5. Follow each link and check its content, redirects, and response status.
6. Give an agent the file as a starting point and ask it to complete the task.
7. Review the answer against the actual documentation, then remove ambiguity from the index or source pages.

Repeat the test when documentation moves or product behavior changes. This tests a specific navigation workflow. It does not demonstrate that a search engine uses the file as a ranking signal.

## How to Check Whether Anything Reads It

Use server or CDN access logs to inspect requests for the file. Record the time, path, response status, and declared user agent. If you need to identify a crawler, verify it using that operator's published guidance rather than trusting the user-agent string alone.

An ordinary browser analytics tag will not reliably measure a plain-text fetch. Use logs for those requests and analytics for visitors who arrive at your website. Our [AI crawler diagnostic guide](https://swetrix.com/blog/how-to-check-if-ai-bots-are-crawling-my-site) explains this distinction.

In [Swetrix](https://swetrix.com), inspect identifiable AI referrals, landing pages, and relevant conversion events. A change in those visits after adding llms.txt is not, by itself, evidence that the file caused it.

## Where Content Production Fits

If an agent reaches your website and finds no useful answer, the missing work is usually on the page itself. Publish accurate explanations, useful examples, and current product details before spending heavily on a file-generation service.

[RankPine](https://rankpine.com/) handles keyword research, article writing, anti-slop editing, images, and publishing. If you need a steady flow of useful content, [see RankPine's content workflow](https://rankpine.com/features/article-generation).
