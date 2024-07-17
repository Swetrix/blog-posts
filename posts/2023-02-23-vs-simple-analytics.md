---
title: Swetrix as an alternative to Simple Analytics
intro: Simple Analytics is a privacy-focused web analytics solution, but is it the best one? Let's find it out in this article.
date: February 23, 2023
hidden: false
author: Andrii Romasiun
nickname: blaumaus
---

Simple Analytics was founded in 2018 by Adriaan van Rossum as a privacy-focused alternative to Google Analytics.
<br />

Swetrix was released in August 2021. Let's look at the differences between Swetrix and Simple Analytics, and why it might be worth considering Swetrix as a replacement for Simple Analytics.

<ol>
  <li>
    <a href="#traffic_monitoring">
      Traffic monitoring
    </a>
  </li>

  <li>
    <a href="#open_source">
      Open source vs closed source
    </a>
  </li>

  <li>
    <a href="#ux">
      Usability & Functionality
    </a>
    <ol>
      <li>
        <a href="#ux_1">
          Performance monitoring
        </a>
      </li>
      <li>
        <a href="#ux_uf">
          User flow analysis
        </a>
      </li>
      <li>
        <a href="#ux_2">
          Shared dashboards
        </a>
      </li>
      <li>
        <a href="#ux_3">
          Customisations & Extensions
        </a>
      </li>
      <li>
        <a href="#ux_4">
          Custom alerts functionality
        </a>
      </li>
      <li>
        <a href="#ux_5">
          Multiple themes and languages support
        </a>
      </li>
      <li>
        <a href="#ux_6">
          AI-powered insights
        </a>
      </li>
      <li>
        <a href="#ux_7">
          Other useful features
        </a>
      </li>
    </ol>
  </li>

  <li>
    <a href="#pricing">
      Pricing
    </a>
  </li>

  <li>
    <a href="#conclusions">
      Conclusions
    </a>
  </li>
</ol>

<h2 id="traffic_monitoring">
  Traffic monitoring
</h2>
The main reason why Google Analytics alternatives exist is that Google Analytics is a privacy nightmare. It collects so much data about your users (the absolute majority of which are not monitored by people), and you can't control it. It's even worse because Google Analytics is based on cookies and the data is stored and processed on Google servers based in the United States.
<br />

Both Swetrix and Simple Analytics are privacy-focused competitors to Google Analytics. They both use a cookieless approach to tracking, which means that no cookies are used to track your users.
<br />

Swetrix under no circumstances stores any personal and identifiable data about your users. The data shown in your dashboard is completely anonymised and aggregated and cannot be linked to any specific user.
<br />

Swetrix is using hash-based session tracking (i.e. we store a hash of data like project ID, user IP, user agent and a daily rotating salt). The hashes are based on the blake3 algorithm, are stored in RAM and cannot be reversed or linked to any specific user. Session identifiers are created when a user first visits your website and ends when no action is taken on your website for 30 minutes or midnight UTC - whatever happens first. This way we can provide accurate and most importantly privacy-focused data about your unique website visitors.
<br />

By using Swetrix, you can track countries your users come from, UTM tags to see what sources are driving traffic to your website, browser and device information, and more. You can also set up custom events (i.e. leads) and, for example, track what users are signing up for your newsletter or your website, what actions they take on your website, and more.
<br />

<img src="https://i.imgur.com/2MiY2xa.png" alt="Swetrix traffic monitoring dashboard" title="Swetrix traffic monitoring dashboard" />
<i>Swetrix traffic monitoring dashboard</i>
<br />

Simple Analytics and Swetrix are using a bit different approach in tracking the country your users come from. Simple Analytics is using timezone to indicate the country of the user. Swetrix is also using timezone-based country detection, but in case the timezone is too generic (for example, just GMT+0 instead of Europe/London) - we use the IP address for country detection as a backup measure.
<br />

Swetrix traffic analysis dashboard also shows some extra metrics like the user's locale, session duration, bounce rate, views per session and more. 

<img src="https://i.imgur.com/WBt87tZ.png" alt="Simple Analytics dashboard" title="Simple Analytics dashboard" />
<i>Simple Analytics dashboard</i>
<br />

<h2 id="open_source">
  Open source vs closed source
</h2>
Swetrix is a <a href="https://github.com/Swetrix" target="_blank" rel="noreferrer noopener">fully open-source</a> web analytics service. All of our code is available on Github, anyone can always analyse it or create pull requests with new functionality, fixes or improvements. Swetrix can be self-hosted, we also provide Docker images for that.
<br /><br />
On the other hand, Simple Analytics is a closed-source product (although some of their projects are open source: for example, tracking script or blog). 
You can use Simple Analytics as a cloud product only and rely on their infrastructure. Being a cloud-only product is not always a bad thing, but this way you can't audit the code for any vulnerabilities and rely on their paid tiers only.

<h2 id="ux">
  Usability & Functionality
</h2>
Both Swetrix and Simple Analytics offer quite an easy-to-use and feature-rich traffic analysis: you can apply segmentation to your data, and track the traffic origin countries and live visitors. For example, Google Analytics is a very unintuitive tool to use, so that's a +1 point for being painless in using.

<h3 id="ux_1">
  Performance monitoring
</h3>
With Swetrix, you can monitor how well your website performs under different circumstances (for example, you can see the page loads in different countries, browsers, and devices), apply segmentation to that data, and export it in different formats. For more tech-savvy people Swetrix offers detailed timing breakdowns (e.g. such metrics as TTFB, DNS resolution timings, TLS setup, DOM content load, browser render time and more) which can help in optimising the website.
<br />
You can always share this dashboard with other people (by default, your stats are private and available only to you). Simple Analytics currently does not provide such functionality.

<img src="https://i.imgur.com/vCuD03M.png" alt="Swetrix Performance monitoring dashboard" title="Swetrix Performance monitoring dashboard" />
<i>Swetrix Performance monitoring dashboard</i>
<br />

<h3 id="ux_uf">
  User flow analysis
</h3>
With Swetrix, you can you can track user interactions and navigation patterns throughout your application, providing valuable insights for optimising user experience and engagement. Analysing user flow might help you to improve your website and the user experience of your visitors.
<br />
Simple Analytics currently does not provide such functionality.

<img src="https://i.imgur.com/SnpspEF.png" alt="User flow diagram" title="User flow diagram">
<i>Swetrix user flow diagram</i>
<br />

<h3 id="ux_2">
  Shared dashboards
</h3>
Shared dashboards i.e. roles system (don't confuse it with public dashboards) allow you to make your stats visible to other analytics users and assign different roles to them. This feature is super-useful for marketing agencies or in cases where you have multiple people working on your website who should have access to the analytics data, yet you don't want to make it public.
<br />

With Swetrix you can share our dashboards with other users, assign them roles (or even let other project administrators invite other people). Simple Analytics currently does not provide such functionality.

<img src="https://i.imgur.com/07E4qr0.png" alt="Table of users of a shared project on Swetrix" title="Table of users of a shared project on Swetrix" />
<i>Table of users of a shared project on Swetrix</i>
<br />

<h3 id="ux_3">
  Customisations & Extensions
</h3>
Swetrix offers a great functionality called Swetrix Extensions & Extensions Marketplace. For example, by using our <a href="https://docs.swetrix.com/sdk-introduction" target="_blank">SDK</a> you can customise your dashboard, add new features or manipulate the data any way you want. The coolest part is that you can use extensions other people created or publish your own to our Extension Marketplace (the Marketplace is still in beta-stage, but we're working hard to improve it) - yes, this is something like browser extensions, but for analytics.
<br />

This way Swetrix can stay simple and usable for most people and if someone needs an additional functionality - they can always add it themselves by installing an appropriate extension. 

<h3 id="ux_4">
  Custom alerts functionality
</h3>
With Swetrix you can set up your custom alerts for a variety of metrics like traffic spikes, live visitors, pageviews and more. After you set up your customisable alert and it gets triggered, Swetrix will notify you via a messenger like Telegram, or simply by email. 
<br />

Swetrix offers super easy integration with messengers like Telegram. Plus, after you set it up, you'll be able to manage your projects via it and we'll notify you of any sign-ins into your account. Simple Analytics does not provide such functionality yet.

<h3 id="ux_5">
  Multiple themes and languages support
</h3>
Both Swetrix and Simple Analytics support multiple themes and languages. This is an important feature because it allows services to be convenient for a bigger variety of people.
<br />

Currently, Swetrix supports the following languages: English, German, Ukrainian, Polish, Swedish, Russian, Greek, Hindi and Chinese. You can customise your language or theme on the dashboard or any other page of swetrix.com. 
<br />

Simple Analytics also offers a multilinguistic service with light and dark themes, but only for the marketing (landing) page (please, correct me if I'm wrong, but I haven't found a way to switch a theme or language on the Simple Analytics dashboard).

<h3 id="ux_6">
  AI-powered insights
</h3>
Swetrix supports a unique feature called 'Traffic forecasting'. It's based on our AI forecasting model which can predict metrics like pageviews, unique visitors and session duration for custom time periods. We are working on improving this model and integrating it not only for the charts but also for all the traffic & performance data in general. By using this you will be able to predict how well your ad campaign performs, how many visitors will you get on a specific day and much, much more! All of this is of course open source and will be included in our public API soon.

<h3 id="ux_7">
  Other useful features
</h3>
Both Swetrix and Simple Analytics offer APIs for developers, which can help you integrate them into your projects or dashboards. Swetrix also provides features like two-factor authentication (2FA) for enhanced security, some customisations like time notation (12/24 hour) or timezone preference, weekly and monthly email reports, multiple domains per one project, IP blacklisting and more! 
<br />

Also, both Swetrix (4.82 KB) and Simple Analytics (7.12 KB) are lightweight web analytics solutions, for example, the Google Analytics tracking script is 45.7 KB in size. Not using heavy scripts like Google Analytics and optimising the performance of your website will positively affect your Google search rankings.

<h2 id="pricing">
  Pricing
</h2>
Swetrix is a self-hostable utility which means that you can host it on your server for free without any limitations. This option requires some technical knowledge: not all people can afford to do it, plus you should keep the software updated, pay for your hosting and set it up yourself.
<br />

For that reason, we offer Swetrix as a cloud service which is super easy to use, but it costs some money. Here are comparison tables of Swetrix and Simple Analytics paid offerings:
<table>
  <tbody>
    <tr>
      <td></td>
      <td><b>Swetrix</b></td>
      <td><b>Simple Analytics</b></td>
    </tr>
    <tr>
      <td>10k events</td>
      <td>$5 / mo</td>
      <td>-</td>
    </tr>
    <tr>
      <td>100k events</td>
      <td>$15 / mo</td>
      <td>$19 / mo</td>
    </tr>
    <tr>
      <td>1m events</td>
      <td>$59 / mo</td>
      <td>$59 / mo</td>
    </tr>
    <tr>
      <td>5m events</td>
      <td>$110 / mo</td>
      <td>-</td>
    </tr>
  </tbody>
</table>

Here's a comparison of yearly subscription expenses:
<table>
  <tbody>
    <tr>
      <td></td>
      <td><b>Swetrix</b></td>
      <td><b>Simple Analytics</b></td>
    </tr>
    <tr>
      <td>10k events</td>
      <td>$50 / yr</td>
      <td>-</td>
    </tr>
    <tr>
      <td>100k events</td>
      <td>$150 / yr</td>
      <td>$108 / yr</td>
    </tr>
    <tr>
      <td>1m events</td>
      <td>$590 / yr</td>
      <td>$588 / yr</td>
    </tr>
    <tr>
      <td>5m events</td>
      <td>$1100 / yr</td>
      <td>-</td>
    </tr>
  </tbody>
</table>

<h2 id="conclusions">
  Conclusions
</h2>
Swetrix is a privacy-focused, fully open-source and cookieless web analytics solution. We are trying to bring privacy back to the web and we believe that we can do it by making analytics simple and easy to use.
<br />

We are a small and independent business that cares about privacy and will always fight for it. Feel free to <a href="https://swetrix.com/signup" target="_blank">give us a try</a>.
