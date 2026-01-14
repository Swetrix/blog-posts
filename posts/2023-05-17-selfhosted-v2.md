---
title: Swetrix Selfhosted v2 is released!
intro: "Swetrix Selfhosted v2: performance monitoring, traffic comparison, user flow and more!"
date: May 17, 2023
hidden: false
author: Andrii Romasiun
twitter_handle: andrii_rom
---

🎉 We're happy to announce that we've finally released Swetrix Selfhosted version 2. It's been more than a year since the last release and a lot has changed during that time.

From now on, we're getting much more focused on our selfhosted edition and we are working on making it as stable and easy to use as possible!

<b>This release includes such features and improvements as:</b>

<ul>
  <li>Performance monitoring: now you can monitor how well your website performs in various countries for various users, you can track such metrics as TTFB, DOM load, etc., filter out the data and much more!</li>
  <li>Added traffic comparison feature.</li>
  <li>Drastically improved stability: fixed an issue when users were not able to delete the projects or update some settings under various circumstances.</li>
  <li>Added support for timezones & time notations for the self-hosted version.</li>
  <li>Redesigned the Dashboard and the app overall and added multiple new languages.</li>
  <li>Added user flow functionality, added an ability to change the style of charts.</li>
  <li>Added an ability to see what visitors are currently live on your website.</li>
  <li>Added session duration, bounce rate, views per session metrics support, added trend lines and an ability to see your custom events on the chart.</li>
  <li>Dropped MariaDB dependency.</li>
  <li>And lots of smaller updates and improvements!</li>
</ul>

<br />
<b>❗️ Beware that this release has breaking changes that may affect you!</b>

Before migrating to the v2 release, we **strongly** advise you to back up all of your data. Previous self-hosted versions of Swetrix are not compatible with v2 and there may be database-related issues when upgrading.
We have tried to make the upgrading process as smooth as possible, but you may suffer a data loss! Please, always do backups.

To migrate from v1 to v2 you have to run the `selfhosted_v1_to_v2.js` script which is located in the `migrations/clickhouse` directory.
Do not run this script twice as it may cause data corruption in your Clickhouse instance.

To test if the migration engine works at all, we advise you to run `test_migration.js` script first. There should not be any errors printed out.

ℹ️ We also updated some environment variables, please take a look at our <a href="https://docs.swetrix.com/selfhosting/configuring" target="_blank" rel="noreferrer noopener">selfhosting documentation</a> to see the current configuration requirements.

Thank you for using Swetrix and we hope you enjoy it! Much more cool updates will be released soon!
