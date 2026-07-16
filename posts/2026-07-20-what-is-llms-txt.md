---
title: "llms.txt: What It Is, Who Actually Reads It, and Whether It Works"
intro: "llms.txt promises to make your site legible to AI. Google says it does nothing, Ahrefs' data says almost nothing fetches it — and yet OpenAI and Anthropic keep nudging it along. Here's the honest state of llms.txt in 2026, and the 10-minute version worth shipping."
date: July 20, 2026
hidden: false
author: Andrii Romasiun
twitter_handle: andrii_rom
---

llms.txt is a proposed web standard: a markdown file at your site's root that gives AI systems a curated map of your most important content — what your site is, which pages matter, where the clean text lives. It was proposed in September 2024 by Jeremy Howard of Answer.AI, and the pitch is appealing: HTML pages are a soup of navigation, cookie banners, and JavaScript; LLMs have limited context windows; so hand them a clean menu instead of making them scrape the whole restaurant.

Eighteen months later, llms.txt occupies a strange spot: half the SEO industry sells it as an "AI visibility" essential, Google flatly says it does nothing, and the measured reality is more interesting than either camp admits. This post covers the spec, the evidence on both sides, and what we'd actually do — with the receipts linked.

## What Goes in the File

The [spec](https://llmstxt.org/) is deliberately simple. One markdown file at `/llms.txt`: an H1 with the site name, a blockquote summary, then H2 sections listing links with one-line descriptions. Here's a condensed real-world example:

```markdown
# Swetrix

> Swetrix is a privacy-first, cookieless web analytics platform —
> an open-source Google Analytics alternative with traffic analytics,
> custom events, funnels, and error tracking.

## Docs

- [Getting started](https://swetrix.com/docs): install the tracking
  script and see data in under 10 minutes
- [Events API](https://swetrix.com/docs/events-api): track custom
  actions like signups and purchases

## Product

- [Pricing](https://swetrix.com/#pricing): plans and free trial
- [Blog](https://swetrix.com/blog): guides on analytics, SEO, and
  AI search traffic
```

There's an optional companion, `llms-full.txt`, which inlines the full text of your documentation into one giant markdown file so an agent can ingest everything in a single fetch.

Note what this is **not**:

| File          | Job                                        | Enforced by anyone?                  |
| :------------ | :----------------------------------------- | :----------------------------------- |
| `robots.txt`  | Access control — what crawlers _may_ fetch | Yes, honored by major crawlers       |
| `sitemap.xml` | Inventory — every URL you want indexed     | Yes, consumed by search engines      |
| `llms.txt`    | Curation — what an AI _should_ read first  | No. Purely voluntary, mostly ignored |

That third row is the entire controversy.

## The Case Against: Nobody's Home

The blunt facts, from the people who run the crawlers and the people who watched the logs:

**Google says no, repeatedly.** John Mueller compared llms.txt to the keywords meta tag — the canonical example of a signal nobody consumes — and Gary Illyes confirmed Google doesn't use it and has no plans to. In June 2026, Google's official AI-features documentation settled it in writing: "You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in Google Search (including its generative AI capabilities), as Google Search itself doesn't use them." That covers AI Overviews and AI Mode — the largest AI search surface there is.

**The logs agree.** [Ahrefs analyzed](https://ahrefs.com/blog/llmstxt-study/) 137,000 sites with llms.txt files and found that in May 2026, 97% of those files received zero bot traffic. Across more than 515 million logged LLM-bot events, requests touching `/llms.txt` were statistically negligible. The bots that matter for AI search visibility — OpenAI's, Anthropic's, Perplexity's crawlers — overwhelmingly fetch your actual pages, not your curated menu.

So the strong version of the pitch — "add llms.txt, get cited by ChatGPT" — is simply false. If a consultant is charging you for llms.txt as an AI-SEO deliverable, you're buying a keywords meta tag with better branding.

## The Case For: Agents, Not Search

Here's where it gets more interesting than "dead on arrival." Through 2026, the file kept accumulating endorsements — just not from search:

- **Anthropic recommends it** in its Writing for Agents guidance, and publishes llms.txt for its own docs.
- **OpenAI publishes the file for its own developer documentation** and references the format in its agent tooling, including work around the Agentic Commerce Protocol.
- **Chrome's Lighthouse 13.3** (May 2026) added an "Agentic Browsing" audit category that checks whether your site provides llms.txt — a hint at where browser vendors think agent traffic is heading.
- **AI coding assistants actually use it.** Cursor, Copilot, Claude and friends fetch documentation on demand while helping someone integrate your product. A curated markdown map means they pull the right page in one hop instead of burning tokens scraping navigation — which is why docs platforms like Mintlify now generate llms.txt automatically for every site they host.

The pattern: llms.txt failed as a _search visibility_ signal but found a real niche as _agent infrastructure_ — particularly for developer documentation, where the reader is increasingly an IDE with a language model inside it. If developers integrate your product, their AI pair-programmer is already part of your funnel, and this file is how you cater to it. And the agentic-browsing story — AI agents visiting sites to research and transact on a human's behalf — is the growth curve to watch; we've written about [what agent traffic means for SEO](https://swetrix.com/blog/ai-agents-and-seo) separately.

## Our Verdict

Ship one — because it costs ten minutes, not because it ranks you.

- **If you have developer docs:** genuinely worth doing properly, llms-full.txt included. This is the one audience measurably using the format.
- **If you're any other site:** a minimal llms.txt is cheap insurance for the agentic web. Expect zero AI-search benefit; Google told you in writing there is none.
- **If someone's selling you an "llms.txt optimization package":** run.

## How to Ship One in 10 Minutes

1. Write the file by hand — H1, blockquote summary, one `## Docs` and one `## Product` section, 5–15 links total. Curate ruthlessly; a 400-link dump defeats the purpose.
2. Serve it at `https://yourdomain.com/llms.txt` as plain text or markdown.
3. If your docs are generated (Fumadocs, Mintlify, Docusaurus and most modern platforms have plugins or built-ins), turn on auto-generation so the file can't rot.
4. Don't block it in robots.txt (yes, people do this).
5. Re-check it whenever your pricing, product names, or doc structure changes — a stale summary quietly feeding agents wrong facts is worse than no file.

## How to Check Whether Anything Reads It

JavaScript analytics can't see this — crawlers fetching a text file don't execute scripts, so no tag-based tool (ours included) will ever show llms.txt hits. Where to look instead:

- **Server or CDN logs.** Grep access logs for `GET /llms.txt` and check the user agents. Cloudflare's analytics make this a two-minute filter.
- **Expect near-silence.** Per the Ahrefs data, that's the norm — a fetch a week from an agent or AI dev tool is a realistic good outcome for a docs-heavy site.

What analytics _can_ see is the part that pays: humans arriving at your site out of AI answers. That's a referrer question — chatgpt.com, perplexity.ai, gemini.google.com showing up in your traffic mix — and it's exactly what [Swetrix](https://swetrix.com) breaks out by default, cookieless and consent-banner-free. The setup for [tracking ChatGPT referrals](https://swetrix.com/blog/how-to-track-chatgpt-referrals-in-swetrix) and [the rest of the AI engines](https://swetrix.com/blog/how-to-track-perplexity-gemini-claude-traffic) takes minutes, and it measures the outcome llms.txt was supposed to influence — which makes it a rather good lie detector for AI-SEO advice in general.

## What Actually Moves AI Visibility

Since the file won't do it, the things that measurably do — each with its own guide on this blog:

- **Content AI engines want to cite:** structured, answer-first, factually sourced, current. At breadth, because citation patterns churn violently. That's a publishing-cadence problem; [SEO autopilot tools](https://swetrix.com/blog/top-10-seo-autopilot-software) like [RankPine](https://rankpine.com/) exist precisely to sustain it.
- **Presence in the sources engines already trust** — the listicles, comparison posts, and platforms like LinkedIn and Reddit that dominate citation share. The playbook: [how to rank in ChatGPT](https://swetrix.com/blog/how-to-rank-in-chatgpt).
- **A monitoring-and-measurement loop** so you know if any of it works: [the GEO toolchain](https://swetrix.com/blog/best-generative-engine-optimization-tools).

## llms.txt FAQ

**Does Google read llms.txt?** No. Google has stated directly that Search — including AI Overviews and AI Mode — doesn't use these files.

**Can llms.txt hurt my SEO?** No. It's invisible to ranking systems. The only real risk is letting it go stale and misinforming the few agents that do read it.

**llms.txt vs robots.txt?** robots.txt restricts crawler access; llms.txt suggests reading order. One is enforced policy, the other is a voluntary menu.

**Should my docs site have one?** Yes — it's the single audience (AI dev tools) with demonstrated usage, and your docs platform can probably generate it automatically.

---

Measure the claim, not the hype: if AI engines start sending you people, it shows up as referral traffic — and Swetrix catches it cookieless, per page, per engine, all the way to signup. [Start the 14-day free trial](https://swetrix.com/signup) and watch what AI search actually delivers.

::CTA:TIME_TO_SWITCH::
