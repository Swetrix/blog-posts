---
title: "Caddy Reverse Proxy A Practical Guide"
intro: "Master the Caddy reverse proxy with this practical guide. Learn to configure sites, Docker, WebSockets, and secure self-hosted apps with real examples."
date: April 4, 2026
hidden: false
author: Andrii Romasiun
twitter_handle: andrii_rom
---

A Caddy reverse proxy acts as a secure front door for all your web services. It sits between the internet and your backend applications, routing incoming requests to the right place while automatically handling things like **HTTPS**, load balancing, and security. Think of it as a smart receptionist for your server.

This approach is managed through Caddy's refreshingly simple Caddyfile, which is a big reason why so many developers have embraced it.

## Why Choose Caddy for Your Next Reverse Proxy

![A cluttered server rack with tangled cables contrasts a neat Caddy server with a Caddyfile, highlighting simplicity.](https://cdnimg.co/ec97efe1-e4fa-4a91-85f3-25c0f1a37f9f/651e652c-bacb-400f-af43-37e81d1ffd62/caddy-reverse-proxy-server-comparison.jpg)

If you're running more than one service—say, a blog, an API, and a private analytics tool—managing them all can get messy. Exposing each one directly to the internet is not only a configuration headache but a security risk. A reverse proxy solves this, but traditional options like [Nginx](https://nginx.org/) or Apache often come with a steep learning curve, demanding complex config files and manual TLS certificate wrangling.

This is exactly where [Caddy](https://caddyserver.com/) shines. It was built from the ground up to eliminate that complexity, prioritizing security and a fantastic user experience right out of the box.

Let's quickly compare the core philosophy behind Caddy versus more traditional servers.

### Caddy vs Traditional Reverse Proxy At a Glance

| Feature            | Caddy                                                       | Traditional Servers (e.g., Nginx)                                      |
| ------------------ | ----------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Configuration**  | Minimal, human-readable Caddyfile; often just a few lines.  | Verbose, block-based syntax; requires more boilerplate.                |
| **Default State**  | Secure by default; HTTPS is the standard.                   | "Works out of the box" but often insecure (HTTP) by default.           |
| **TLS Management** | Automatic HTTPS via Let's Encrypt & ZeroSSL, fully managed. | Manual setup; requires Certbot, cron jobs, or other external tools.    |
| **Primary Goal**   | Simplicity and security for the modern web.                 | Flexibility and fine-grained control, often at the cost of simplicity. |

As you can see, the difference isn't just about features—it's about a fundamental shift in how a web server should behave in 2026.

### Simplicity and Automatic HTTPS

Caddy's killer feature, without a doubt, is its **automatic HTTPS**. Just point your domain to your server, add it to the Caddyfile, and Caddy does the rest. It automatically provisions and renews TLS certificates from public CAs like [Let's Encrypt](https://letsencrypt.org/) and [ZeroSSL](https://zerossl.com/) on its own.

This is a world away from the old way of doing things, which involved manually generating certificates, meticulously configuring them, and praying your renewal script doesn't fail. Caddy's approach saves hours of work and prevents one of the most common security slip-ups: expired certificates.

> The goal here is simple: trade needless complexity for sane defaults. A minimal Caddyfile can accomplish in a few lines what might take dozens in another system, making your deployments faster and far less prone to human error. For developers and small teams, this is a massive win.

### Modern Performance Without Compromise

While Caddy is known for its simplicity, it doesn't skimp on performance. It was written in Go, a language known for its concurrency and efficiency, making it a powerful contender in the web server space.

In fact, some independent benchmarks have shown Caddy v2 can outperform Nginx by nearly **2x** in certain reverse proxy tests. Even more impressive, the results revealed almost no performance drop when comparing its HTTP and HTTPS proxying. This completely flips the old script that HTTPS inherently means a big performance hit.

This level of performance, combined with its robust security-first architecture, makes Caddy a solid choice for even the most demanding applications. You get speed, security, and ease of use without having to compromise. And because it's open-source, you also get the transparency and community support that comes with it; you can read more about the [benefits of open-source software](https://swetrix.com/blog/benefits-of-open-source) to see how that helps ensure reliability and continued innovation.

## Your First Caddy Reverse Proxy Configurations

![A diagram shows a browser accessing app.yourdomain.com, which directs traffic to a server acting as a reverse proxy for localhost:3000.](https://cdnimg.co/ec97efe1-e4fa-4a91-85f3-25c0f1a37f9f/034a1173-beac-497f-ae4b-b70f1181432a/caddy-reverse-proxy-proxy-setup.jpg)

Alright, let's dive in and get our hands dirty with some Caddyfiles. The theory is great, but seeing a reverse proxy click into place is where you really appreciate its power. We'll start with the bread-and-butter scenario: getting a single backend application live on the web.

Picture this: you've built a cool new service. It could be a [Node.js](https://nodejs.org/) app humming away on port `3000`, a Python backend on port `8000`, or maybe a [Go](https://go.dev/) API on port `8080`. The goal is to make it publicly and securely accessible at `app.yourdomain.com` without exposing its internal port to the internet.

This is exactly what Caddy was born to do. You'll point your domain’s DNS A record to your server's public IP, and Caddy will take it from there, acting as the secure gateway between the outside world and your app.

### A Simple Single-Site Reverse Proxy

For this common setup, the Caddyfile is so concise it almost feels like cheating. If your application is listening on port `3000`, this is all you need:

app.yourdomain.com {
reverse_proxy localhost:3000
}

That's it. Seriously. With just those two lines, Caddy works its magic.

The first line, **`app.yourdomain.com { ... }`**, defines a **site block**. It tells Caddy to answer any traffic coming in for that specific domain. Behind the scenes, Caddy immediately gets to work provisioning a free, trusted TLS certificate from [Let's Encrypt](https://letsencrypt.org/) for you.

The second line, **`reverse_proxy localhost:3000`**, is the core instruction. It tells Caddy to forward every single one of those incoming requests over to the service running locally on port `3000`. You get automatic HTTPS, managed certificate renewals, and a rock-solid proxy, all with minimal effort. This is the foundational pattern you'll use again and again.

### Subdomain vs. Subpath Routing

Of course, projects rarely stay that simple for long. Soon you'll have multiple services—an API, a frontend, maybe a documentation site—and you'll need a strategy for routing traffic. The two main paths you can take are using **subdomains** (`service.yourdomain.com`) or **subpaths** (`yourdomain.com/service`). Caddy handles both beautifully.

From experience, a subdomain strategy is usually cleaner and scales much better as you add more services. It creates a very clear separation of concerns. For instance, you might put your API at `api.yourdomain.com` and your web app at `app.yourdomain.com`.

Here’s how you'd configure Caddy to proxy two distinct services running on different ports:

# Frontend application on port 3000

app.yourdomain.com {
reverse_proxy localhost:3000
}

# API service on port 8080

api.yourdomain.com {
reverse_proxy localhost:8080
}
This configuration is incredibly organized. Each service has its own dedicated site block and its own automatically provisioned TLS certificate. It's my go-to for anything beyond a single service.

The other option is to use subpaths, which can be handy for smaller projects or when you want to keep everything under a single domain name. Let's say you want traffic for `yourdomain.com/api/` to hit your API on port `8080`, while everything else goes to your main app on port `3000`.

> The choice between subdomains and subpaths really comes down to architecture. I generally lean toward subdomains for better service isolation, but subpaths are great for consolidating related endpoints under one domain.

Here's how you'd set up that subpath routing in your Caddyfile:

yourdomain.com { # Route traffic starting with /api/
handle_path /api/\* {
reverse_proxy localhost:8080
}

    # Fallback for all other traffic
    handle {
        reverse_proxy localhost:3000
    }

}

In this setup, the **`handle_path`** directive is a powerful matcher. It intercepts any request URL that begins with `/api/` and sends it straight to the API service on port `8080`. The plain **`handle`** block below it acts as a catch-all, directing any other request to the main application. It's an elegant way to manage traffic flow from a single domain.

## Handling Real-Time Traffic and Modern Apps

![Diagram showing a browser connecting to a server using WebSocket and SSE, highlighting X-Forwarded-For header.](https://cdnimg.co/ec97efe1-e4fa-4a91-85f3-25c0f1a37f9f/cab981e2-136a-47c4-9ff9-cfd3eb3832bc/caddy-reverse-proxy-proxy-protocols.jpg)

Simple request-response proxying is one thing, but today's apps are all about real-time interaction. Think live chat, data dashboards that update instantly, or collaborative editing tools. These features depend on persistent connections, usually through **WebSockets** or **Server-Sent Events (SSE)**.

Unlike a typical HTTP request that opens, serves, and closes, these protocols keep a connection alive for two-way communication. This can often trip up reverse proxies that aren't built to handle them, but Caddy takes care of it beautifully.

The `reverse_proxy` directive is smart enough to detect when a client is trying to upgrade its connection to a WebSocket. You don't need to add any special flags or complex modules. The same simple config we used for a basic site just works.

chat.yourdomain.com {
reverse_proxy localhost:8080
}

That's it. Caddy sees the upgrade request and seamlessly passes the WebSocket traffic through to your backend. It's one of those "it just works" moments that makes Caddy so great to work with.

### Forwarding the Real Client IP Address

There's a classic "gotcha" when you first put a reverse proxy in front of an application. Suddenly, your backend app thinks every single request is coming from Caddy's own IP address. This completely breaks any analytics, logging, or security rules based on the visitor's IP.

We need to tell Caddy to pass along the original user's IP. The standard method is to add a few `X-Forwarded-*` headers to the request Caddy sends to your backend. For any serious setup, this isn't optional—it's essential, especially if you're using analytics tools that rely on accurate visitor data.

You can add these headers easily with the `header_up` subdirective.

app.yourdomain.com {
reverse_proxy localhost:3000 {
header_up X-Forwarded-For {http.request.remote.host}
header_up X-Forwarded-Proto {http.request.scheme}
header_up Host {http.request.host}
}
}

Let's quickly break down what's happening here:

- **`X-Forwarded-For`**: This is the big one. It passes the original IP address of the user.
- **`X-Forwarded-Proto`**: Tells your backend if the user connected via `http` or `https`.
- **`Host`**: Forwards the original domain the user requested, like `app.yourdomain.com`.

> By adding these headers, you give your backend applications the full picture. Your logs become meaningful, analytics can geolocate users correctly, and security tools like rate limiters can work as intended.

### Multi-Layer Proxies and Performance Notes

In more complex setups, you might have proxies behind other proxies. For instance, a main Caddy server could route traffic to another Caddy instance that manages a specific group of microservices.

While Caddy handles these multi-layer scenarios, it's something to be mindful of from a performance perspective. A [performance issue reported on GitHub](https://github.com/caddyserver/caddy/issues/6751) for version **2.8.4** pointed out some multi-threading challenges that could add latency in deeply nested `reverse_proxy` configurations.

This is particularly relevant for platforms like Swetrix, which might proxy requests through several internal services for data processing. If your architecture involves chaining proxies, keep an eye on performance. This is a core tenet of effective infrastructure management, and you can learn more about its importance in our guide on [what is performance monitoring](https://swetrix.com/blog/what-is-performance-monitoring).

## Getting Caddy Ready for Production

Having a working Caddyfile is a great start, but it's only part of the story. The real challenge comes when you push your Caddy reverse proxy to a live production server, where it absolutely must be reliable, resilient, and easy to manage.

So, let's dive into the two most common and battle-tested ways to run Caddy in the wild: using [Docker Compose](https://docs.docker.com/compose/) for container-based setups and running it as a **systemd** service on a traditional server or VM. Your choice here usually comes down to how you're already managing the rest of your applications.

### Containerizing Caddy with Docker Compose

If you're already running your apps in containers, using Docker Compose to manage Caddy is a no-brainer. It lets you define your entire stack—Caddy, your web app, a database, you name it—all in one clean `docker-compose.yml` file. This approach keeps all your services neatly isolated but allows them to communicate over a private Docker network.

The secret to a solid Docker Compose setup is how you handle Caddy's configuration and data. You'll want to mount your `Caddyfile` directly into the container and, more importantly, create a dedicated volume for Caddy's persistent data. This is where it stores crucial things like TLS certificates.

Without a volume, Caddy would have to fetch brand-new certificates every single time the container restarts. Trust me, you'll hit Let's Encrypt's rate limits faster than you can say "downtime."

Here’s a practical `docker-compose.yml` that I often use as a starting point. It runs Caddy alongside a sample application container called `my-app`:

version: "3.8"

services:
caddy:
image: caddy:latest
container_name: caddy_proxy
restart: unless-stopped
ports: - "80:80" - "443:443" - "443:443/udp" # Don't forget this for HTTP/3!
volumes: - ./Caddyfile:/etc/caddy/Caddyfile - caddy_data:/data - caddy_config:/config

my-app:
image: your-app-image:latest # Swap this with your actual app image
container_name: my_app
restart: unless-stopped # Notice we don't expose any ports here. # Caddy talks to it over the internal Docker network.

volumes:
caddy_data:
caddy_config:

With this configuration, Caddy can talk to the `my-app` service by simply using its name as the address. The corresponding `Caddyfile` becomes incredibly simple, proxying requests to port `8000` on the `my-app` container:

your.domain.com {
reverse_proxy my-app:8000
}

The real magic here is that the `my-app` service is completely cut off from the public internet. The only way in is through Caddy. For a complete, end-to-end example of this in action, check out the guide on how to [self-host the Swetrix analytics platform](https://swetrix.com/docs/selfhosting/how-to).

### Running Caddy as a Systemd Service

On the other hand, if you aren't using containers and prefer running services directly on a Linux server, setting up Caddy as a **systemd** service is the way to go. Systemd is the modern init system for nearly all major Linux distros, and it’s perfect for ensuring Caddy starts on boot, restarts automatically if it ever crashes, and plays nicely with standard logging tools.

The best part? If you install Caddy using one of the official packages (like from its APT or YUM repositories), the systemd service file is usually configured for you right out of the box.

> Using systemd turns Caddy from a command you run into a first-class citizen on your server. It’s the professional way to manage a long-running service on a VM or bare-metal machine.

Once it's installed as a service, you can manage Caddy with the `systemctl` commands you're likely already familiar with:

- **`sudo systemctl start caddy`**: Fires up the Caddy service.
- **`sudo systemctl enable caddy`**: Makes sure Caddy starts automatically every time the server boots.
- **`sudo systemctl status caddy`**: Gives you a quick health check and shows recent log entries.
- **`sudo systemctl reload caddy`**: This is the big one. It gracefully reloads your configuration with zero downtime.

Your main `Caddyfile` will typically live at `/etc/caddy/Caddyfile`. Whenever you edit that file, a quick `sudo systemctl reload caddy` is all you need to apply the changes. This setup is incredibly robust and is my go-to recommendation for any non-containerized deployment.

Alright, enough theory. The best way to learn is by doing. Let's walk through a real-world example and put Caddy to work by self-hosting [Swetrix](https://swetrix.com/), a great privacy-focused analytics platform.

This isn't just a generic exercise. We're going to deploy the self-hosted version of Swetrix and build the exact Caddyfile needed to securely expose its dashboard and data ingestion endpoint. This is a setup I've used myself, and it's a solid pattern for many similar applications.

![Caddy deployment process flow showing Docker image, systemd service configuration, and Caddy server with automated SSL.](https://cdnimg.co/ec97efe1-e4fa-4a91-85f3-25c0f1a37f9f/6400343d-0bc3-4efb-9347-c8a402893000/caddy-reverse-proxy-deployment-process.jpg)

Once we're done, you'll have a clean, informative dashboard like the one above, all running on your own server and served securely through our Caddy reverse proxy.

### A Production-Ready Caddyfile for Swetrix

The self-hosted Swetrix stack has two key services we need to make accessible: the API/dashboard and the separate data ingestion service. To keep things clean, we'll assign them to different subdomains: `app.yourdomain.com` for the dashboard and `tr.yourdomain.com` for the tracking endpoint.

If you follow the official Swetrix `docker-compose.yml` file, the API runs on `localhost:8080` and the ingestion service is on `localhost:8081`.

Here's the entire Caddyfile you'll need. It's that simple.

# Swetrix Dashboard

app.yourdomain.com {
reverse_proxy localhost:8080 {
header_up X-Forwarded-For {http.request.remote.host}
}
}

# Swetrix Ingestion Endpoint

tr.yourdomain.com {
reverse_proxy localhost:8081 {
header_up X-Forwarded-For {http.request.remote.host}
}
}

This Caddyfile is a perfect example of Caddy's elegance. With just a handful of lines, it automatically handles getting and renewing **HTTPS certificates** for both subdomains.

We've also made sure to include `header_up X-Forwarded-For`. This is non-negotiable for an analytics platform. It passes the real visitor's IP address to Swetrix, so you get accurate data instead of seeing all traffic as coming from Caddy's own IP.

> **Why This Works:** This setup cleanly separates the two services using subdomains, a common and robust pattern. Caddy takes care of all the TLS complexity while ensuring the backend application gets the headers it needs to function correctly. It's a textbook reverse proxy implementation.

### A Note on Performance for Analytics Traffic

When you're self-hosting an analytics tool, performance is everything. Every single event your website tracks hits your Caddy reverse proxy first. It has to be incredibly fast and reliable to avoid slowing down your site or, even worse, dropping valuable data.

Caddy is known for being lightweight and fast, but it's important to know where it stands. In a detailed reverse proxy benchmark, Caddy's default setup handled an impressive **230,619** requests. For comparison, a highly tuned Nginx instance pushed **326,510** requests.

The latency figures were also telling. Caddy's max latency was over **21,000ms**, while the optimized Nginx setup stayed around **~2,500ms**. For a high-traffic system like Swetrix that's constantly receiving data, these numbers matter. Understanding these performance trade-offs is crucial for building a solid infrastructure.

This flow chart visualizes the two main paths for getting Caddy up and running: deploying it as a Docker container or setting it up as a native systemd service.

![Caddy deployment process flow showing Docker image, systemd service configuration, and Caddy server with automated SSL.](https://cdnimg.co/ec97efe1-e4fa-4a91-85f3-25c0f1a37f9f/6400343d-0bc3-4efb-9347-c8a402893000/caddy-reverse-proxy-deployment-process.jpg)

Ultimately, both paths lead to the same result: a professionally managed, secure Caddy server ready to handle your traffic. This flexibility means you can easily fit Caddy into whatever workflow you prefer, whether you're all-in on containers or run services directly on the host.

## Answering Your Caddy Reverse Proxy Questions

Once you get Caddy up and running as a reverse proxy, a few common questions always seem to pop up. It's a powerful piece of software, and figuring out the "why" behind its behavior is the key to really getting comfortable with it. Let's walk through the questions I hear most often, with answers based on what I've learned from countless real-world setups.

### How Does Automatic HTTPS Actually Work Here?

Caddy's automatic HTTPS is hands-down its best feature, and it works beautifully with the reverse proxy. As soon as you give Caddy a public domain name in your Caddyfile, it springs into action, using the ACME protocol to grab a TLS certificate from a provider like [Let's Encrypt](https://letsencrypt.org/).

The whole process is completely hands-off. Caddy proves it controls the domain, installs the certificate, and schedules renewals long before the cert ever expires. You don't have to do a thing.

For your application sitting behind the proxy, this setup is perfect. Caddy handles the secure, encrypted HTTPS traffic from the user. It then passes that traffic along to your backend service—maybe a Node.js app on `localhost:3000`—as simple, unencrypted HTTP.

> The beauty of this is that it puts all your TLS management in one spot: Caddy. Your own applications get a lot simpler because they no longer have to mess with certificates, encryption, or renewal logic at all.

### Can I Really Run Multiple Services on Just One Server?

Yes, absolutely. This is what Caddy was built for, and it's where it saves you an enormous amount of time and effort. You can easily host a dozen different apps on a single machine, each with its own internal port, and just let Caddy act as the traffic cop.

It’s surprisingly simple. You just add a new site block to your `Caddyfile` for each service. Caddy looks at the domain name of an incoming request and instantly knows which backend to send it to.

You might have a setup that looks something like this:

- **`analytics.yourdomain.com`** points to your self-hosted [Swetrix](https://swetrix.com/) instance.
- **`blog.yourdomain.com`** sends traffic to a [Ghost](https://ghost.org/) or [WordPress](https://wordpress.org/) container.
- **`api.yourdomain.com`** is routed to your custom API server.

For each of those subdomains, Caddy will fetch and manage a separate, automatically renewed TLS certificate. This makes it incredibly cheap and easy to run a whole suite of services from a single IP address—a common pattern for home labs, personal projects, and small companies.

### What are the Must-Do Security Practices for a Caddy Proxy?

Caddy has great security defaults, but there are a few extra things you should do to really lock down your server. Think of Caddy as the front gate; you want to make it as tough as possible.

First, and this is the most important part: make sure your backend services are _not_ accessible from the public internet. They should only be listening on the local machine (`localhost`) or on a private Docker network. If you expose them directly, you defeat the entire purpose of having a reverse proxy.

Next, you should use Caddy’s `header` directive to add security headers to every response. These headers are instructions for the browser that add extra layers of defense.

A few essential headers to include are:

- **`Strict-Transport-Security (HSTS)`**: Tells browsers to _only_ use HTTPS when talking to your site in the future.
- **`Content-Security-Policy (CSP)`**: Helps prevent cross-site scripting (XSS) by telling the browser which sources are safe for scripts, styles, and other content.
- **`X-Frame-Options`**: Stops "clickjacking" attacks by blocking other sites from embedding your page in an `<iframe>`.

Finally, keep Caddy updated. The development team is very quick to patch any security issues. Simply running the latest version is one of the easiest and most effective things you can do to stay secure.

### How Do I Figure Out What’s Wrong with My Proxy Setup?

Compared to the old guard of web servers, troubleshooting Caddy is a breath of fresh air. When things go wrong, it's usually one of a few common problems.

Your first stop should always be the logs. Caddy produces clean, structured JSON logs that are easy to read.

- If you're using **`systemd`**, you can watch the logs live with `journalctl -u caddy -f`.
- If you're in **Docker**, the command is `docker logs -f <caddy_container_name>`.

Before you even try to reload your server, get into the habit of running `caddy validate --config /path/to/Caddyfile`. This one command will save you so much time by catching syntax errors, typos, and misplaced brackets before they can cause trouble.

If the logs and validation look clean, the next culprit is almost always networking. The single most common issue I run into is a server firewall blocking Caddy from talking to the backend port. Double-check that Caddy can actually reach the address you've put in the `reverse_proxy` directive.

For really tricky problems, you can turn on verbose debug logging. Just add a `debug` global option to the very top of your Caddyfile. This will give you an incredible amount of detail on how Caddy is handling every single request.

{
debug
}

yourdomain.com { # ... your config
}

Between validation, logs, and basic network checks, you’ll be able to solve 99% of your Caddy reverse proxy issues in minutes.

---

Turn your website traffic into actionable insight with **Swetrix**. Our privacy-first, cookieless web analytics platform gives you the data you need without compromising user privacy. [Start your 14-day free trial](https://swetrix.com).
