---
title: Introducing Swetrix Selfhosted v3!
intro: 'Swetrix Selfhosted v3: our biggest update so far! Session analytics, funnels, error tracking and much more!'
date: May 25, 2024
hidden: false
author: Andrii Romasiun
twitter_handle: andrii_rom
---

Over 1 year have passed since our [previous selfhosted release](https://swetrix.com/blog/selfhosted-v2). At that time, it was a solid product with unique and very useful features. But now, we're happy to introduce: Swetrix Selfhosted v3.

It's a very powerful release, with dozens of unique features and hundreds of hours spent into it. We've made it more stable and even easier to install and use.

<h2>
  What changed
</h2>

<h3>
  🔥 Major updates
</h3>

- Added session analytics: you can now see which pages / custom events were tracked within a session, as well as some details about them
- Added support for marketing funnels
- Added client-side error tracking
- Added metadata feature for custom events
- Added more metrics for performance monitoring: you can now see load time distribution over different quantiles, as well as apply different aggregation functions to your data
- Added [API](https://docs.swetrix.com/statistics-api) key support

<h3>
  🙂 Minor updates
</h3>

- Project ID now generated on the backend for new sites
- Removed validate-ip-node library in favour of built-in Node.js net module
- Project "Allowed origins" setting now supports wildcards
- You can now partially clear analytics data with filters
- Added support for more detailed filters; you can now apply multiple filters to the same data column
- Added new time buckets for aggregated reporting: "All time" and "This hour
- Added site search functionality to the dashboard page
- Added support for cumulative mode for charts
- Migrated frontend frow React to Remix to enable server-side rendering and improve performance
- Added support for [embedded dashboards](https://docs.swetrix.com/how-to-embed)
- Updated analytics dashboard design and other UI / UX improvements throughout the application
- Added support for dashboard hotkeys
- Updated API dependencies
- Updated Clickhouse & Redis versions

<h3>
  🔧 Fixes
</h3>

- Fixed an issue where users could not disable the "Show live visitors in page title" feature.
- Fixed a possible issue where a non-existent tab could be opened in the dashboard

<h2>
  How to upgrade to v3?
</h2>

You can find upgrade instructions on our [release notes](https://github.com/Swetrix/swetrix-api/releases/tag/v3.0.0) page on Github.

Thanks a lot for using Swetrix, our work would not be possible without your support, and we hope you will love this release!
