---
title: The Tech Stack of an Open Source Web Analytics SaaS
intro: Have you ever thought about the technologies and frameworks that go into building and running a web analytics SaaS? The tech stack of a company is an important factor in its product development and growth. It's like a recipe, with each component contribut
date: January 08, 2023
hidden: false
author: Andrii Romasiun
nickname: blaumaus
---

Have you ever thought about the technologies and frameworks that go into building and running a web analytics SaaS? The tech stack of a company is an important factor in its product development and growth. It's like a recipe, with each component contributing to the overall success of the platform.

I've always been curious about what technologies or languages other companies use to build their products. It's always interesting to see the running costs of companies, whether you need this information for your own side project or just out of curiosity.

And now it's time to reveal our tech stack - the tools and technologies that power our open-source and privacy-focused web analytics SaaS, <a href="https://swetrix.com" target="_blank" rel="noreferrer noopener">Swetrix</a>.

<ol>
  <li>
    <a href="#languages">
        Languages
    </a>
  </li>
  
  <li>
    <a href="#frameworks">
        Frameworks
    </a>
  </li>
  
  <li>
    <a href="#databases">
        Databases
    </a>
  </li>
  
  <li>
    <a href="#hosting">
        Hosting & Infrastructure
    </a>
  </li>
  
  <li>
    <a href="#email">
        Email
    </a>
  </li>

  <li>
    <a href="#payments">
        Payments
    </a>
  </li>


  <li>
    <a href="#support">
        Support
    </a>
  </li>
  
  
  <li>
    <a href="#other">
        Other
    </a>
  </li>
</ol>

<h2 id="languages">
  Languages
</h2>
<ol>
  <li>
    <a href="https://www.typescriptlang.org/" target="_blank" rel="noreferrer noopener">TypeScript</a>: all the backend is written using this language. We chose it due to how powerful yet simple this language is and a huge support of the community.
  </li>
  <li>
    <a href="https://en.wikipedia.org/wiki/JavaScript" target="_blank" rel="noreferrer noopener">JavaScript</a>: used on the frontend side in combination with React. We chose not to migrate to TypeScript yet due to how readable and non-bloated React code using simple JS is, the types validation is done via Prop-types library instead.
  </li>
</ol>

<h2 id="frameworks">
  Frameworks
</h2>
<ol>
  <li>
    <a href="https://remix.run/" target="_blank" rel="noreferrer noopener">Remix</a>: in my opinion it's the best frontend framework there is. It allows your React app to be rendered server-side (you may even set it up in a way where not a single byte of JS is loaded to client!). They have great docs, often release updates and overall is a very pleasant framework to work with.
  </li>
  <li>
    <a href="https://tailwindcss.com/" target="_blank" rel="noreferrer noopener">Tailwind CSS</a>: using this framework saved us a ton of time designing and making our frontend. IMO it was one of the best technical decisions to migrate from SCSS to Tailwind.
  </li>
  <li>
    <a href="https://naver.github.io/billboard.js/" target="_blank" rel="noreferrer noopener">billboard.js</a>: we use it as a charting library on our dashboards. Quite a decent and customisable charting library, based on D3.
  </li>
  <li>
    <a href="https://nestjs.com/" target="_blank" rel="noreferrer noopener">Nest.js</a>: all our APIs are written using this framework. It's a very scalable, fast, efficient and widely supported framework. The more you use it the more you appreciate it, being able to share the entities and interfaces between the endpoints, the methodologies it uses makes you love it more and more every day. Also it's based on TypeScript which is great for making reliable APIs.
  </li>
  <li>
    <a href="https://www.djangoproject.com/" target="_blank" rel="noreferrer noopener">Python Django</a>: our blog (you're reading it right now!) is written using this framework.
  </li>
</ol>


<h2 id="databases">
  Databases
</h2>
<ol>
  <li>
    <a href="https://www.mysql.com/" target="_blank" rel="noreferrer noopener">MySQL</a>: one of the most popular choices among SaaS services. Very reliable and easy to use database, especially in combinations with ORMs like typeorm. We use it for storing data about users, projects, alerts and so on.
  </li>
  <li>
    <a href="https://clickhouse.com/" target="_blank" rel="noreferrer noopener">Clickhouse</a>: super fast and efficient column oriented database. It's perfect for data aggregation queries and storing enormous amounts of data. We use it for storing analytics data.
  </li>
  <li>
    <a href="https://redis.io/" target="_blank" rel="noreferrer noopener">Redis</a>: a great and well-documented in-memory key-value database. We use it for rate-limiting, caching and session management.
  </li>
</ol>

<h2 id="hosting">
  Hosting & Infrastructure
</h2>
<ol>
  <li>
    <a href="https://www.hetzner.com/" target="_blank" rel="noreferrer noopener">Hetzner</a>: very reliable and affordable hosting & dedicated servers provider. Perfect for side-projects and SaaS companies.
  </li>
  <li>
    <a href="https://fly.io" target="_blank" rel="noreferrer noopener">Fly</a>: we use it for hosting our frontend code. It's a great, scalable and affordable service which allows your app to run in several regions across the globe which will make it load very fast.
  </li>
  <li>
    <a href="https://njal.la/" target="_blank" rel="noreferrer noopener">Njalla</a>: great privacy-focused domain registrar. Fair pricing (they also accept crypto), easy to use dashboard. Supports 2FA, PGP, API access.
  </li>
</ol>

<h2 id="email">
  Email
</h2>
<ol>
  <li>
    <a href="https://postmarkapp.com/" target="_blank" rel="noreferrer noopener">Postmark</a>: we use it for outbound mailing (transactional and broadcast emails). They provide a lot of integrations into any codebase, plus they give free $75 for bootstrapped startups.
  </li>
  <li>
    <a href="https://www.fastmail.com/" target="_blank" rel="noreferrer noopener">Fastmail</a>: inbound emails provider. Very simple and easy to use.
  </li>
</ol>


<h2 id="payments">
  Payments
</h2>
<ol>
  <li>
    <a href="https://www.paddle.com/" target="_blank" rel="noreferrer noopener">Paddle</a>: subscriptions and credit card processing service. Their documentation is amazing, dashboards are quite simple and easy to use.
  </li>
</ol>

<h2 id="support">
  Support
</h2>
<ol>
  <li>
    <a href="https://github.com/idoco/intergram" target="_blank" rel="noreferrer noopener">Intergram</a>: an amazing, Telegram-based and open-source in-app support chat. Easy to customise and integrate into existing apps.
  </li>
</ol>


<h2 id="support">
  Development
</h2>
<ol>
  <li>
    <a href="https://code.visualstudio.com/" target="_blank" rel="noreferrer noopener">VS Code</a>: beautiful semi code editor / semi-IDE, especially when using extensions. The best code editor for web development.
  </li>
  <li>
    <a href="https://github.com/" target="_blank" rel="noreferrer noopener">Github</a>: source code hosting and versioning, we also host our tasks kanban board there.
  </li>
  <li>
    <a href="https://pnpm.io/" target="_blank" rel="noreferrer noopener">pnpm</a>: fast and disk space efficient JavaScript package manager.
  </li>
</ol>


<h2 id="other">
  Other
</h2>
<ol>
  <li>
    <a href="https://swetrix.com" target="_blank" rel="noreferrer noopener">Swetrix</a>: we use our tool to monitor our websites traffic and performance🙂.
  </li>
  <li>
    <a href="https://uptimerobot.com/" target="_blank" rel="noreferrer noopener">UptimeRobot</a>: quite a decent uptime monitoring service. Their free tier includes 50 monitors, which is totally more than enough for us.
  </li>
</ol>
