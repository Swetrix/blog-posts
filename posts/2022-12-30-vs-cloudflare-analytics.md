---
title: Swetrix as an alternative to Cloudflare Web Analytics
intro: Cloudflare Web Analytics is quite a popular privacy-focused web analytics utility, but is it really the best solution? Let's find it out in this article.
date: December 30, 2022
hidden: false
author: Andrii Romasiun
nickname: blaumaus
---

Cloudflare offers several analytics products and one of them is called Cloudflare Account Analytics, but it is only available to Cloudflare users. Cloudflare Web Analytics, on the other hand, is a free tool that is available to everyone.
<br />

Cloudflare Account Analytics is a server-side tracking utility that provides insights about website requests, bandwidth and so on, while Cloudflare Web Analytics employs client-side tracking, similar to Swetrix and Google Analytics.
Server-side tracking doesn't require the use of scripts on your website, which can be affected by ad-blockers or core web vitals.
However, client-side tracking has the advantage of being able to filter out automated traffic to your website, resulting in more accurate pageview counts.
<br />

<img src="https://i.imgur.com/p5NpNHG.png" alt="Cloudflare Web Analytics Dashboard" title="Cloudflare Web Analytics Dashboard" />
<i>Cloudflare Web Analytics Dashboard</i>

<ol>
  <li>
    <a href="#data">
        Data collection & dashboard.
    </a>
    <ol>
      <li>
        <a href="#data_1">
            Sampling is applied to Cloudflare Dashboard.
        </a>
      </li>
      <li>
        <a href="#data_2">
            Bots are not excluded.
        </a>
      </li>
      <li>
        <a href="#data_3">
            Data retention.
        </a>
      </li>
      <li>
        <a href="#data_4">
            User flow analysis.
        </a>
      </li>
    </ol>
  </li>

  <li>
    <a href="#website_limit">
        Number of websites is limited.
    </a>
  </li>

  <li>
    <a href="#missed_insights">
        Cloudflare dashboard is missing a lot of insights.
    </a>
  </li>

  <li>
    <a href="#open_source">
        Open source vs closed source.
    </a>
  </li>

  <li>
    <a href="#visitors">
        Different techniques of unique visitors tracking.
    </a>
  </li>

  <li>
    <a href="#comparison">
        Comparison table.
    </a>
  </li>

  <li>
    <a href="#pricing">
        Pricing.
    </a>
  </li>
</ol>

<h2 id="data">
  Data collection & dashboard.
</h2>

<h3 id="data_1">
  Sampling is applied to Cloudflare Dashboard.
</h3>

Cloudflare's documentation states that the analytics they provide are based on a sample — a subset of the dataset.
<br />
That means that Cloudflare Web Analytics is not displaying the actual stats of your website, but they take a sample of data they collect (it's about 10% of your total traffic) and show you the data based on the patterns seen in the sample.
10% is a very small sample size and it cannot always provide you with the accurate data.
<br /><br />
The main reason of why some analytics companies apply sampling techniques is to reduce the running costs, especially if the analytics service is 'free'.
<br /><br />
At Swetrix, we prioritise both the accuracy of the data we provide and the privacy of your users. To achieve this, we never sample the data we collect and always show you the actual traffic coming to your website. We believe that you should have access to the most accurate data available while also preserving the privacy of those visiting your site.


<h3 id="data_2">
  Bots are not excluded.
</h3>

Another reason why Cloudflare Web Analytics dashboard is inaccurate is that they are not excluding bot traffic. Bots are labelled as 'Unknown' and in some cases they take a decent percentage of total statistics.
<br />
At Swetrix, we detect bot traffic and exclude it from your website statistics, as this bot traffic does not represent actual pageviews. Bot traffic is also not accounted in our pricing.


<h3 id="data_3">
  Data retention.
</h3>
As stated in their documentation, Cloudflare only stores the analytics data for 6 months (when they launched it was only 7 days). Long data retention is a crucial feature for people who use analytics, as it's very common for people to compare large data sets, i.e. month-to-month or year-to-year comparison.
<br /><br />
Cloudflare Analytics is currently a free product, so it makes sense they want to reduce costs of operating it. It's not their core product, so we are not expecting for them to upper the limits of data retention.
<br /><br />
At Swetrix, we currently store your data indefinetely. You can compare and check the data for any period you want.

<h3 id="data_4">
  User flow analysis
</h3>
With Swetrix, you can you can track user interactions and navigation patterns throughout your application, providing valuable insights for optimising user experience and engagement. Analysing user flow might help you to improve your website and the user experience of your visitors.
<br />
Cloudflare Web Analytics currently does not provide such functionality.

<img src="https://i.imgur.com/SnpspEF.png" alt="User flow diagram" title="User flow diagram">
<i>Swetrix user flow diagram</i>
<br />

<h2 id="website_limit">
  Number of websites is limited.
</h2>
Cloudflare has a maximum of 10 websites you can add to the web analytics. Swetrix free tier also provides you with 10 websites - we do it as it currently costs us to store the website data. Swetrix paid tiers have a limit of 20 to 30 websites per account, but in feature these limits will be greatly increased.


<h2 id="missed_insights">
  Cloudflare dashboard is missing a lot of insights.
</h2>

Cloudflare Dashboard is too limited and misses a lot of crucial features most people use in their daily life.
For example, here's what Cloudflare is missing compared to what you can get on Swetrix:
<ol>
  <li>
    No visit duration metric. With Cloudflare you're not able to see for how long people stay on your website and measure the quality of traffic based on this information.
  </li>
  <li>
    No realtime dashboard view. You cannot see how many people are currently on your website and cannot check what browser or OS are the people currently on your website are using or what country do they come from.
  </li>
  <li>
    Cloudflare does not support UTM tags, so you cannot measure the effectiveness of your marketing or email campaigns.
  </li>
  <li>
    No custom event tracking. With Cloudflare you can't track some specific events like user signing up or purchasing something on your webiste.
  </li>
  <li>
    No bounce rate metric so you cannot analyse the quality of traffic coming from a specific source.
  </li>
  <li>
    No custom extensions functionality. With Swetrix you can create or install custom addons to extend the dashboard and even publish them our Marketplace. Cloudflare does not support such functionality.
  </li>
  <li>
    You're not able to blacklist specific IP addresses that will be excluded from traffic collection. This is a useful feature that helps to ignore analytics data coming from you or other website administrators.
  </li>
  <li>
    No teams support. With Swetrix you can invite other people to your project, or even set different roles to them. Cloudflare does not support this.
  </li>
  <li>
    No public dashboards, so you cannot share your analytics data with other people, for example your website visitors.
  </li>
</ol>


<h2 id="open_source">
  Open source vs closed source.
</h2>
Swetrix is a <a href="https://github.com/Swetrix" target="_blank" rel="noreferrer noopener">fully opensourced</a> web analytics utility. Our code is available on Github, anyone can always analyse it or create pull requests with new features, fixes or improvements. Swetrix can be self-hosted, we also provide Docker images for that.
<br /><br />
On the other hand, Cloudflare Web Analytics is a fully closed source product. People who use Cloudflare have to blindly trust this company, as there's no way of knowing what's going on when they use it. You will never know if there are any data leaks on their side or if the code has some backdoors.


<h2 id="visitors">
  Different techniques of unique visitors tracking.
</h2>
Swetrix and Cloudflare Web Analytics have different definitions of a unique visitor. Cloudflare Web Analytics counts a visit as a successful page view that has an HTTP referrer that doesn't match the hostname of the request. This means that Cloudflare counts the number of times someone comes to any page of your site from another site, and it may differ significantly from the counts of other analytics tools depending on the type of site and audience you have. For example, if the same person visits your site ten times in one day, Cloudflare Web Analytics would list that person as ten unique visitors, while Swetrix would show them as one unique visitor.
<br /><br />
Swetrix does not use cookies or local storage to track unique visitors, and it does not generate a device-persistent identifier because these are considered personal data under GDPR. Instead, it generates a daily changing identifier using the visitor's IP address and User Agent, and it anonymises this data by running it through a hash function with a dailt rotating salt. 

<h2 id="comparison">
  Comparison table.
</h2>
<table>
  <tbody>
    <tr>
      <td></td>
      <td><b>Swetrix</b></td>
      <td><b>Cloudflare Analytics</b></td>
    </tr>
    <tr>
      <td>Price</td>
      <td>Free up to 5,000 pageviews, then $15 per month</td>
      <td>Free, with data retention limits. Then $20 / month per site</td>
    </tr>
    <tr>
      <td>Data accuracy</td>
      <td>100% accurate</td>
      <td>Inaccurate, as data sampling is applied</td>
    </tr>
    <tr>
      <td>Open source</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Privacy-focused</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>Custom addons</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Cookieless</td>
      <td>Yes</td>
      <td>Yes</td>
    </tr>
    <tr>
      <td>Custom events</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Public dashboards</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Sharable dashboards</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>UTM tags support</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Realtime dashboard</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Visit duration metric</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Bounce rate metric</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Data segmentation</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
    <tr>
      <td>Script size</td>
      <td>&lt; 5 KB</td>
      <td>&lt; 5 KB</td>
    </tr>
    <tr>
      <td>Browser addons</td>
      <td>Yes</td>
      <td>No</td>
    </tr>
  </tbody>
</table>

<h2 id="pricing">
  Pricing.
</h2>
Like Google Analytics, Cloudflare Web Analytics is free to use. It's natural to be suspicious of free services, as the saying goes, "If you're not paying for the product, you are the product.". This means that the company providing the free service is likely monetizing your data in some way. While it's unclear how Cloudflare is funding its free web analytics service, it's worth considering that the company may be using it as a form of marketing, a strategy in which a free service is offered to attract users to the company's main products.
<br /><br />
As a large company with significant resources, Cloudflare may be using its web analytics service as a way to increase awareness of its other offerings. It's worth considering whether Cloudflare will continue to offer this service if it is not successful in attracting users to its other products.
<br /><br />
Cloudflare is a company that is not primarily focused on web analytics, and this is evident in the design of its analytics dashboard. One issue with the dashboard is the lack of easy access to the stats and an oversimplified dashboard with a lot of missing features. This inconvenience suggests that Cloudflare is not as specialized in web analytics as other companies in the industry.
<br /><br />
Swetrix, on the other hand, is build by a small, bootstrapped and dedicated team that fully focuses on analytics. We are a transparent company with a simple and fair pricing policies. It costs time and money to develop and maintain a SaaS, so we chare a fee for our subscription plans. We also offer a free tier for small websites, as we understand the need of privacy-focused analytics by a website of any size.
<br /><br />
By using Swetrix you will support a small and fully independent web analytics business, so why not to <a href="https://swetrix.com/signup" target="_blank" rel="noreferrer noopener">give us a try</a>.

<br />
<br />
Thank you for reading this, I hope 2022 was a great year for you! We wish you an amazing new year, we hope it brings you joy, success, and all that you hope for. May it be a year filled with positive experiences and growth :)
<br />
See you in the next blog post.