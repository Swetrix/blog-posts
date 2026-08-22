---
title: "How to Measure SaaS Retention Rate"
intro: "Learn how to measure SaaS retention rate with logo, GRR, NRR, renewal, and product cohort formulas—plus benchmarks and privacy-first analytics."
date: August 22, 2026
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "dddd1cba-cbc9-41b4-92fb-5ae36ebcccff"
---

Measuring SaaS retention rate requires tracking five distinct metrics to understand growth: customer logo retention, gross revenue retention (GRR), net revenue retention (NRR), renewal rate, and product cohort retention. Your billing system holds the source of truth for revenue and contract renewals, and a privacy-first analytics platform like Swetrix maps the product behavior driving those financial outcomes. Before building a reporting dashboard, write down the business decision you need to make. Identifying expansion opportunities requires NRR, while diagnosing onboarding drop-off requires event-based cohorts.

Evaluating these five metrics together protects you from distorted growth narratives. An organization can post impressive top-line ARR numbers while shedding accounts beneath the surface, because large enterprise expansions can mask high customer churn. Pairing revenue metrics with behavioral cohorts reveals whether users build enduring workflows or abandon the product after trial.

| Business Question | Primary Metric | Core Data Source |
|---|---|---|
| What percentage of customer accounts remain active? | Customer / Logo Retention | Subscription billing ledger |
| How much starting recurring revenue survived churn and downgrades? | Gross Revenue Retention (GRR) | Subscription billing ledger |
| Did the existing customer base expand or shrink financially? | Net Revenue Retention (NRR) | Subscription billing ledger |
| What proportion of ending contracts agreed to renew? | Customer and Revenue Renewal Rate | Contract management / Billing |
| Are users continuously receiving value from core features? | Product / User Cohort Retention | Product event analytics |

## Step 1: Decide Which SaaS Retention to Measure

Define the specific question you want to answer before building your retention metrics. Customer logo retention calculates the percentage of accounts remaining from a starting group. Gross revenue retention measures how much starting recurring revenue survived churn and downgrades over a specific period, whereas net revenue retention adds expansion revenue to that calculation to show whether the financial value of the existing base shrank or grew.

Relying on NRR alone hides customer loss, since upgrades from remaining enterprise customers can offset churned revenue and make the metric look healthy. You need logo retention to monitor the shrinking base alongside GRR to evaluate the protected core. If logo retention drops to 70 percent while NRR sits at 110 percent, your revenue depends on an increasingly concentrated pool of expanding accounts, which elevates business risk if a major account cancels later.

Avoid confusing a web analytics returning-user report based on browser storage with a ledger based on paid invoices. Calculate NRR and renewals through your subscription software, then connect a product analytics layer to explain the usage patterns preceding a downgrade or cancellation. A web analytics dashboard tracking device cookies shows visit frequency, but it cannot determine whether an account renewed a contract, upgraded seats, or failed a recurring payment.

![A SaaS founder comparing a billing ledger with a product-usage dashboard to show why customer, revenue, renewal, and behavioral retention answer different questions.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/dddd1cba-cbc9-41b4-92fb-5ae36ebcccff/1.webp)

## Step 2: Define the Cohort and Denominator

A retention metric depends entirely on its denominator. Treat website visitors, registered users, paying accounts, and enterprise workspaces as distinct entities, because mixing them inflates your starting base and artificially suppresses your retention percentage. If you mix free trial accounts with paying enterprise workspaces, differences in conversion and usage patterns will drag down a paid account retention rate and make the result hard to interpret.

For GRR and NRR calculations, include only customers with active recurring revenue at the beginning of the measurement period and exclude newly acquired accounts. While total revenue growth reports include new sales, mixing those into a retention calculation disguises churn problems. If you start January with $100,000 in recurring revenue and acquire $20,000 in new sales during the month, that $20,000 belongs in your new business acquisition reporting rather than your January cohort retention denominator.

Limit your customer renewal rate denominator to accounts whose contract or billing term ended during the selected period. Customers who canceled midway through a multi-year deal, or users who are not yet up for renewal, belong in a separate churn category rather than the renewal denominator. For example, if you manage 500 total active customers but only 40 reach the end of their subscription term in March, your renewal denominator for March is exactly 40.

Product retention requires a strict cohort-entry event, so document whether users enter the cohort upon account creation, trial start, first payment, or first use of a core feature. Choose an observation window aligned with product usage patterns, such as daily or weekly for frequent-use tools, monthly for standard B2B SaaS, and quarterly for enterprise reporting, but treat these as starting points rather than universal intervals. If your tool is an invoicing system meant for end-of-month accounting, measuring daily return cohorts produces an artificially low retention curve that reflects the product's natural usage cycle rather than disengagement.

Create a one-page data dictionary before reporting these numbers. Outline the entity, cohort-entry date, denominator, observation window, return event, and source system for every retention metric. Version this document to help future teams understand the exact methodology used for past cohorts, which prevents inconsistencies when leadership or data definitions change.

## Step 3: Calculate Logo, GRR, and NRR

Build a revenue movement ledger combining starting revenue, new business, expansion, contraction, voluntary churn, involuntary churn, reactivations, refunds, and pricing adjustments to feed the three core formulas. Use a [percentage calculator](https://swetrix.com/tools/percentage-calculator) or spreadsheet software to automate the math across monthly cohorts. Classifying every dollar into these specific movement buckets prevents upgrades from disguising cancellations in your reporting.

| Revenue Movement Component | Description | Included in GRR? | Included in NRR? |
|---|---|---|---|
| Starting Recurring Revenue | Active recurring revenue at beginning of period | Yes (Base) | Yes (Base) |
| Expansion Revenue | Upgrades, add-ons, and seat expansions | No | Yes (+) |
| Contraction Revenue | Downgrades and plan reductions | Yes (-) | Yes (-) |
| Voluntary Churn | Explicit cancellations and non-renewals | Yes (-) | Yes (-) |
| Involuntary Churn | Payment failures and expired billing details | Yes (-) | Yes (-) |
| Reactivations | Returning churned accounts from prior cohorts | No | No (Tracked separately) |
| New Business | Brand new customer accounts added during period | No | No (Tracked separately) |

Customer or logo retention rate counts the starting cohort accounts still active at the end of the period, divided by the starting cohort size:

`Customer retention = Starting-cohort customers active at period end ÷ starting-cohort customers × 100`

Gross revenue retention excludes expansion revenue, meaning it cannot exceed 100 percent:

`GRR = (Starting recurring revenue - churned revenue - contraction revenue) ÷ starting recurring revenue × 100`

Net revenue retention captures upgrades, so a high-performing product can score above 100 percent:

`NRR = (Starting recurring revenue + expansion revenue - contraction revenue - churned revenue) ÷ starting recurring revenue × 100`

An illustrative example demonstrates the difference between GRR and NRR. If you begin with $100,000 in recurring revenue, subtract $5,000 in churn, subtract $2,000 in downgrades, and add $8,000 in upgrades, your GRR sits at 93 percent. Upgrades do not count toward protecting the baseline. In contrast, your NRR reaches 101 percent because the $8,000 in new value from existing accounts outweighs the $7,000 lost to churn and contraction.

![A cohort-retention heatmap with signup rows fading across months and an activation milestone highlighted.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/dddd1cba-cbc9-41b4-92fb-5ae36ebcccff/2.webp)

## Step 4: Measure Renewals and Product Cohort Retention

Renewal rate applies only to customers eligible to renew. The customer renewal rate divides the number of renewed customers by the total number of customers up for renewal, and the revenue renewal rate applies the exact same logic to contract value. If your denominator includes new customers or customers holding active multi-year contracts, the calculation breaks.

`Customer Renewal Rate = Renewed customers ÷ customers up for renewal × 100`
`Revenue Renewal Rate = Renewed contract value ÷ contract value up for renewal × 100`

For annual subscriptions, track the renewal timing, contract value up for renewal, renewed value, and payment failures separately. A failed credit card payment can represent operational friction, whereas an explicit cancellation may point to a problem with product value, pricing, or fit. Tracking involuntary churn independently allows your operations team to address dunning, retry logic, and card-updater sequences without mistaking billing issues for product dissatisfaction.

Event-based product cohort retention calculates the percentage of a starting cohort that returns to perform a specific action centered around a core value moment. Automatically firing a login event can generate inflated retention numbers that may not correlate with business health. Track meaningful milestones like creating a report, sending a campaign, connecting an integration, completing a recurring workflow, or inviting a teammate instead.

Google Analytics 4 provides a retention overview report showing return patterns after acquisition, but that report measures return activity rather than verified account-level product value. GA4 can use a device-based identifier or a signed-in User-ID, depending on the reporting identity and implementation, so a cohort report does not automatically represent account-level retention. Building your cohort analysis around custom product events solves this gap by linking retention curves to the actual jobs users complete inside your application.

Compare accounts that reach these activation milestones with accounts that miss them. When a specific product behavior correlates strongly with long-term revenue retention, you establish your [product market fit metrics](https://swetrix.com/blog/evaluate-product-market-fit-metrics) and know which features to highlight during new user onboarding.

## Step 5: Connect Billing and Privacy-First Analytics

Presenting accurate retention data requires a source-of-truth architecture that relies on the billing or subscription system for recurring revenue movements and renewals. Run a [Google Analytics alternative](https://swetrix.com/google-analytics-alternative) like Swetrix alongside the billing system to handle activation tracking, event funnels, error monitoring, and technical diagnosis.

Join billing outcomes to product behavior using an opaque internal account key. Swetrix's Profiles and Sessions functionality can support that connection without sending email addresses, names, or raw customer records as event properties. You should implement lifecycle events precisely as they occur in the application, using identifiers like `signup_started`, `account_created`, `activation_completed`, `integration_connected`, `plan_upgraded`, and `subscription_cancelled`.

```javascript
// Example: Tracking an activation event with an opaque identifier
swetrix.identify('acc_94817a3b');
swetrix.track('activation_completed', {
  plan_tier: 'growth',
  billing_interval: 'annual',
  integration_type: 'slack'
});
```

Use Swetrix ordered funnels to measure the journey from the pricing page through signup and into first product value. Because funnel conversion serves as an activation diagnostic rather than a retention rate, spot drop-offs to investigate confusing onboarding paths or repeated errors using session replays. Swetrix supports ordered funnels, making it practical to map flows such as pricing page view, signup submission, workspace creation, integration setup, and first value delivery.

Replay requires an explicit start call through `startSessionReplay()` and offers masking modes, so configure sensitive pages and input fields to keep passwords and financial data outside the recording. The session replay capability runs as an optional Cloud feature, so recording remains under your implementation's control.

Keep this implementation privacy-conscious, because modern regulatory guidance covers more than traditional cookies, including tracking pixels, device fingerprinting, web storage, and similar technologies. The UK Information Commissioner's Office published final storage and access technologies guidance on April 29, 2026, while France's CNIL treats audience-measurement exemptions as dependent on the purpose and subsequent use of analytics data. Review the requirements that apply to your jurisdictions before launch, map billing IDs strictly to opaque analytics keys, document the deletion process, and audit every event payload before pushing data to production.

![A privacy-conscious developer inspecting masked session replay and error logs beside an opaque account identifier.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/dddd1cba-cbc9-41b4-92fb-5ae36ebcccff/3.webp)

## Step 6: Analyze Cohorts, Benchmarks, and Churn Drivers

Analyze retention as a curve over time by plotting retention percentages against customer age. This visual distinction separates early activation problems from later contraction or mature expansion. Segment the resulting data by signup month, activation status, UTM campaign, pricing plan, monthly versus annual billing, customer size, and support history.

```
Month 0: 100% |====================================| (Cohort of 200 accounts)
Month 1:  78% |============================        | (Early drop-off during onboarding)
Month 3:  65% |=======================             | (First renewal drop-off for monthly)
Month 6:  62% |======================              | (Usage curve stabilizes)
Month 12: 58% |=====================               | (Annual contract renewal point)
```

Start comparisons with your company’s historical cohorts and move to like-for-like segment comparisons before treating third-party benchmarks as targets. Stripe's [May 7, 2026 guidance on NRR](https://stripe.com/resources/more/net-revenue-retention) uses above 100 percent as a directional sign of healthy retention and revenue growth, 80 to 100 percent as a range that warrants investigation, and below 80 percent as low, but it cautions that benchmarks vary by industry and business stage.

[ChartMogul’s 2024 SaaS research](https://chartmogul.com/reports/saas-retention-the-new-normal/) analyzed over 2,500 SaaS companies and found that companies with $15 million to $30 million or more in ARR generated 40 percent of their growth from expansion, an increase from 30 percent in early 2021. The median company with at least 100 percent NRR grew 48 percent year over year. In H1 2024, top-quartile companies with up to 1,500 subscribers reached 100 percent NRR, whereas companies with more than 12,000 subscribers typically recorded 76 percent NRR.

The [2025 SaaS Billing Report from ChartMogul](https://chartmogul.com/reports/saas-billing-report/) analyzed data from 2024 and found that companies with $250 to $500 ARPA had median NRR of 88 percent on annual plans versus 76 percent on monthly plans. For products priced below $25 ARPA, median customer retention was 62 percent for annual plans compared with 41 percent on monthly plans. Its January 2024 cohort analysis also showed that new customers were over three times more likely to switch from monthly to annual billing in month two than in month nine.

[ChartMogul's 2025 AI retention research](https://chartmogul.com/reports/saas-retention-the-ai-churn-wave/) found stark differences across categories. After screening roughly 3,500 software companies, the report compared businesses with at least $250,000 ARR and found median annualized NRR of 82 percent for B2B SaaS, with an upper quartile of 97 percent, and 49 percent for B2C SaaS. AI-native companies had median GRR of 40 percent and median NRR of 48 percent, reflecting early exploratory usage. The report's medians also varied by price band:

- Products priced above $250 per month: 70 percent GRR and 85 percent NRR.
- Products priced at $50 to $249 per month: 45 percent GRR and 61 percent NRR.
- Products priced below $50 per month: 23 percent GRR and 32 percent NRR.

Historical data provides context on growth rates. [ChartMogul's 2023 SaaS Retention Report](https://chartmogul.com/reports/saas-retention-report/) found that businesses with NRR above 100 percent grew at an average of 43.6 percent annually, compared with 13.1 percent for businesses below 60 percent NRR, and reported a best-in-class NRR range of 115 to 125 percent for B2B SaaS selling to mid-market and enterprise customers.

These benchmark figures are segment-specific and directional, not universal SaaS targets, because retention varies by industry, ARPA, billing model, customer mix, and company stage.

Turn benchmark gaps into measurable experiments. High NRR combined with falling logo retention indicates expansion masks account loss, whereas low GRR points directly to churn or contraction. If you see strong product retention combined with weak renewal, investigate pricing, contract terms, or implementation problems. Identify the weak point, deploy A/B tests through Swetrix on your onboarding flows, and allow enough time for the retention outcomes to mature rather than declaring a victory based on a three-day lift in signups. Swetrix experiments support 2 to 20 variants using feature flags, enabling you to test onboarding flows against activation and long-term retention goals.

## Step 7: Validate the Number and Avoid Measurement Traps

Establish a QA checklist before distributing retention reports to investors or leadership. Reconcile the aggregate metric directly with the billing ledger, and confirm that your SQL queries or dashboard filters exclude new customers from GRR, NRR, and starting-cohort retention calculations.

Normalize annual contracts consistently to prevent comparing a month of enterprise revenue against a year of SMB revenue. Separate voluntary cancellations from involuntary payment failures, and define how your data model handles reactivations, mid-cycle refunds, prorated credits, and forced pricing migrations. Compare account-level results against user-level results independently to ensure a single large enterprise account adding seats fails to disguise dozens of smaller accounts churning.

Avoid common measurement failures, such as using total end-of-period revenue as the denominator for a retention calculation. Stop mixing monthly customer retention figures with annual revenue retention targets. Discard default login events when measuring product value, keeping free trial retention isolated from paid retention.

Report the raw cohort size alongside the retention percentage, because a small cohort of ten users can swing 10 percent based on one cancellation and generate false panic in a weekly review. Annotate dates in your reports when you change product packaging, alter billing terms, or adjust the definition of an active user. Tracking retention requires historical comparability, and silently changing the rules breaks the timeline. Build a reproducible dashboard specification containing your logo retention, GRR, NRR, renewal rate, and product cohort retention formulas, then stick to it.

---

Swetrix is a privacy-first, open-source web and product analytics platform built as a cookieless alternative to Google Analytics. Track website traffic, user events, funnels, and marketing campaigns in a privacy-conscious environment without relying on intrusive cookie banners. Start connecting your product behavior to your revenue retention metrics today at [swetrix.com](https://swetrix.com).
