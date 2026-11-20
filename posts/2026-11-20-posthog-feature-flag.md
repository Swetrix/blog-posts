---
title: "PostHog Feature Flags: Setup, Rollouts, and Privacy"
intro: "Learn how PostHog feature flags work, configure targeted rollouts, distinguish releases from experiments, and decide when Swetrix may fit."
date: November 20, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/1d986b13-7c71-4d38-acc2-52878635d475/0-c39f4b9701cb.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "1d986b13-7c71-4d38-acc2-52878635d475"
---

A working PostHog feature flag safely controls user exposure to a new interface module. You can configure targeted rollouts, write resilient application code that handles network failures, and distinguish a release toggle from a measured experiment. If your primary requirement is privacy-first web analytics alongside these rollout controls, you will also see how Swetrix handles dashboard-managed rollouts without heavy configuration.

Before starting, you need an active PostHog account, an initialized SDK in your application, and the new feature code written and ready to deploy. You must also decide what the default experience should be while the request loads over the network, which ensures users never see a broken interface.

## What a PostHog feature flag controls

A PostHog feature flag acts as a remote configuration switch that determines which already-implemented feature an eligible user receives. The flag does not determine whether the underlying code deploys to your servers or bundles into your client application, because that code ships regardless of the flag state. Instead, an application checks the flag key at runtime, reads the returned value, and decides which code path to execute. 

You control exposure through different flag types. A standard boolean flag returns true or false to act as an on/off switch for a new component. Multivariate flags return specific string values, allowing you to route users between three or more distinct variations of a page. Remote-config flags pass dynamic JSON payloads to the application, which lets you change interface copy, module layouts, or pricing variables without requiring a new deployment. 

PostHog applies release conditions to determine who receives the enabled flag. These rules evaluate user properties, group attributes, or defined cohorts. You can restrict a rollout geographically, limit it to users on a specific browser, or target specific customer accounts. When you need to manage risk, percentage rollouts distribute the new feature to a random but consistent fraction of that targeted audience. If you primarily need analytics-led rollouts paired with strict data-privacy defaults, Swetrix offers an alternative path to manage these exact targeting and percentage mechanisms.

![For the setup section, show a product engineer and marketer watching a new newsletter signup module reach a small audience while other visitors still see the original, with no readable text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/1d986b13-7c71-4d38-acc2-52878635d475/1-b4fc4acba0f3.webp)

## Set up a PostHog feature flag in stages

A reliable rollout requires a clear sequence: defining the rules in the dashboard, implementing the check in your code, providing a deliberate fallback, and verifying the boundaries before scaling up. The following steps use a new newsletter-signup module as a running example. 

### 1. Define the key, type, and release conditions

Open your PostHog dashboard to create a new flag and assign a descriptive key, such as `newsletter-signup-redesign-v2`. This string is what your application will reference, so naming conventions matter when multiple people manage the dashboard. Select a boolean flag type for a standard rollout. 

Next, establish your release conditions. To set up the flag, [create and configure it in PostHog](https://posthog.com/docs/feature-flags/creating-feature-flags) before evaluating it in your application code.

### 2. Add the SDK check and a deliberate fallback

Your application must query the SDK using the exact key defined in the dashboard. Because the new newsletter module affects the initial view, make the flag value available before the first render. 

When you write the implementation code, include a specific fallback state. The SDK evaluates the flag over the network, and requests can fail, time out, or be blocked by privacy extensions. If the SDK cannot reach the server, it returns the fallback value. Set this default to `false` so that a failed network request safely displays the existing newsletter module rather than crashing the layout or showing a blank space. 

### 3. Test both paths, then expand the rollout

Test both enabled and disabled states, along with the targeting boundaries. Override the flag locally in your development environment to force the enabled state, ensuring the new newsletter module renders correctly. Then force the disabled state to confirm the old module remains functional. 

Once deployed to production, monitor the initial targeted audience. Check your application error logs and user feedback for the 10% of UK visitors receiving the new module. When you confirm the code behaves as expected, return to the PostHog dashboard and adjust the release condition. Increase the percentage rollout to 50%, observe the metrics for another cycle, and expand it to 100% of all users before removing the geographical restriction entirely.

![For the safety section, show a developer tracing a browser UI switch beside a separate server-side authorization gate, clarifying that a visible flag is not a security boundary; include no readable text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/1d986b13-7c71-4d38-acc2-52878635d475/2-ac7bd6996a6f.webp)

## Prevent evaluation failures that break rollouts

Where your application evaluates a flag determines what that flag can safely control and whether users experience consistent results. Evaluating conditions in the browser differs from evaluating them on your backend servers. 

### Keep client-side checks to presentation changes

Browser-side evaluation suits interface modifications, visual redesigns, and client-side routing. When you gate a new navigation menu behind a client-side check, the browser asks the SDK if the current user qualifies, then renders the appropriate HTML. 

This client-side check is never a security boundary, because gated code remains visible in the JavaScript bundle delivered to the browser. Anyone can inspect the network requests, view the returned flag values, or manually alter the local state to force the new component to render. If your flag gates a premium feature or an administrative dashboard, a client-side check prevents accidental clicks but offers zero protection against intentional access. 

### Enforce sensitive behavior on the server

Sensitive access, business logic, and database writes belong behind server-side checks. When evaluating a flag on the backend, the client never sees the decision-making process or the unauthorized code paths. 

PostHog supports server-side local evaluation, which downloads the flag definitions to your server and evaluates requests in memory without waiting for an external network call. This approach requires passing a secret API key, which must remain strictly on the server and never leak into frontend bundles. 

Evaluating the same flag across different runtimes means both sides need to agree on who the user is, as [PostHog's server and client guide advises](https://posthog.com/learn/feature-flags/pocket-guide/server-and-client). So keep that identity consistent across both environments.

![For the experiment section, show an unlabeled two-variant conversion chart beside a content marketer reviewing newsletter signups, with no result or winner implied.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/1d986b13-7c71-4d38-acc2-52878635d475/3-b2d800fc2581.webp)

## Distinguish a release flag from an experiment

While a feature flag controls exposure, an experiment introduces a formal hypothesis, defined variants, structured inclusion criteria, and specific outcome metrics. The two concepts overlap functionally, but they serve different business goals.

PostHog sets up experiments as flag-backed operations. In the newsletter example, the feature flag assigns visitors to either the old layout or the redesigned version. The experiment layer wraps around that flag to track a conversion metric, such as completed signups or subsequent email-link clicks. You define a hypothesis stating that the new layout will increase signups by 15%, and the platform calculates statistical significance based on the data flowing through both variants. 

For account-based B2B products, random individual assignment causes friction. If you assign users to a new pricing page individually, two employees at the same company might see different subscription tiers during a shared meeting. So plan the experiment around consistent experiences for each account, and measure the overall impact across your client base.

## Verify cookie behavior instead of assuming flags are cookieless

Implementing a feature flag does not automatically make your application cookieless. The tracking behavior depends entirely on how you configure the SDK and manage the data path. 

The standard browser setup stores specific user information and session identifiers in a first-party cookie. This persistence helps maintain a consistent experience when a user refreshes the page or returns later, which ensures they do not bounce between the control and variant groups. 

PostHog [normally stores some information about the user in their browser using a cookie](https://posthog.com/tutorials/cookieless-tracking), a common approach for analytics tools. So review your specific configuration rather than assuming any feature-flag implementation fulfills regional compliance requirements automatically.

## Manage analytics-led rollouts with Swetrix

Transitioning from legacy tools to a privacy-first platform often forces you to choose between strict data ethics and practical product measurement. Swetrix bridges this gap if your main requirement is targeted rollouts and experiment measurement built directly into a compliant standard analytics workflow.

If you are evaluating Swetrix as a [PostHog open source alternative](https://swetrix.com/blog/posthog-open-source-alternative), the evaluation centers on your engineering requirements. Swetrix supports dashboard-managed flags with targeting rules based on country, browser, custom attributes, and percentage rollouts. Its linked experiments use these flags to manage variant eligibility, track exposures, and analyze conversion attribution. The documented implementation relies on a client-side `getFeatureFlags()` method, making it effective for UI switches, marketing-site testing, and component rollouts.

For standard pageview tracking, Swetrix acts as a cookieless Google Analytics alternative. It uses no cookies or client-side tracking identifiers, allowing you to operate without intrusive consent banners under GDPR, CCPA, and PECR. However, when you configure experiments or optional session replays, privacy boundaries require your attention. Swetrix recommends passing a `profileId` when your site already identifies users, because that identity ties exposure to conversion attribution across sessions. You retain full responsibility for the data you send through custom events, and session replays capture distinct behavioral data depending on your configured masking settings.

For measuring the impact of landing-page changes, Swetrix provides a unique structural advantage through its Google Search Console integration. While feature flags themselves do not improve search rankings, pulling search queries and performance metrics into the same dashboard where you measure variant conversions provides immediate context for how organic traffic reacts to a rollout. You can run the numbers through an [A/B test calculator](https://swetrix.com/tools/ab-test-calculator) directly alongside your technical search data, keeping marketing and product measurement in a single privacy-conscious environment. 

## Frequently asked questions

### What is a PostHog feature flag?
It is a code-level evaluation check whose value controls whether a specific feature, layout, or configuration variant is shown to an eligible user. The flag acts as a remote switch for code that is already deployed.

### Are PostHog feature flags the same as A/B tests?
No. A feature flag controls exposure to a code path, while an experiment (or A/B test) adds a structured hypothesis, variant setup, specific exposure rules, and defined outcome metrics to measure the business impact of that exposure.

### Can PostHog evaluate flags in the browser and on the server?
Yes. Browser evaluation handles UI changes and client-side routing, while server evaluation protects sensitive business logic and database operations. Aligning user IDs across both environments prevents inconsistent behavior.

### Are PostHog feature flags automatically cookieless?
No. The standard browser setup uses first-party cookies for persistence. Operating without cookies requires configuring specific memory modes in the SDK, which trades automatic session persistence for manual identity management.

### Can Swetrix replace PostHog for feature flags?
Swetrix offers a streamlined replacement for organizations focused on client-side targeting, percentage rollouts, and analytics-backed experiments. If you require complex backend local evaluation or specialized enterprise flag controls, review the current API documentation for both tools to confirm technical parity.

---
Ready to manage your rollouts and measure experiment conversions without sacrificing user privacy? Try Swetrix today for a complete, cookieless analytics platform built for modern product teams. Get started at https://swetrix.com.
