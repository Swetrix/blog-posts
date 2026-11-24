---
title: "Caddy Reverse Proxy Setup: A Step-by-Step Guide"
intro: "Configure Caddy to route your domain to an app, enable HTTPS, verify the setup, and troubleshoot Docker networking and 502 errors."
date: November 24, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/6f8fdd2e-ea89-4b46-b906-5f362107764c/0-949433cd5f91.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "6f8fdd2e-ea89-4b46-b906-5f362107764c"
---

Configuring Caddy to accept a public request, present a TLS certificate, and forward traffic to an application on a private port protects the public connection while keeping the application behind the proxy. A Caddy reverse proxy setup can handle routing and HTTPS automatically when the domain and network are configured correctly, freeing the underlying service to process data. This configuration gives you a secured public endpoint, whether you expose a local Node application or a self-hosted analytics platform like Swetrix.

![An editorial network-flow scene showing a browser request reaching Caddy at a public-facing gateway, then continuing to an app on a private server; make the proxy boundary clear and include no text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/6f8fdd2e-ea89-4b46-b906-5f362107764c/1-4113c6af5071.webp)

## Map the Request Flow

Before writing configuration files, map how traffic moves through the proxy. A visitor types your domain into a browser, sending a request across the internet that reaches the public-facing Caddy gateway. The web server reads the domain name, secures the connection using TLS, and forwards the raw request to the upstream.

Caddy's `reverse_proxy` directive sends requests to one or more backends, with upstream addresses that can include local targets such as `localhost:4000` or a hostname such as `example.com`. The same directive also provides a `rewrite` option for changing a request while proxying, as described in the [`reverse_proxy` directive documentation](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy).

## Check the Network and DNS Prerequisites

Configurations fail when the network blocks them. Check these external requirements before touching the server configuration files, because a perfect Caddyfile cannot bypass a closed firewall.

First, confirm the application is running and answering requests at its private address. If Caddy cannot reach the application, the proxy will return an error to the visitor. Send a local curl request to the application port to verify it returns the expected response.

Next, point your public DNS records to the machine hosting Caddy. Check both A records for IPv4 and AAAA records for IPv6. A stale AAAA record pointed at an old server will route validation traffic to the wrong address, causing certificate issuance to fail.

Third, check your firewall rules. For a standard public HTTPS deployment that redirects HTTP to HTTPS, allow inbound traffic on both ports 80 and 443. Caddy's HTTP challenge uses port 80, while its TLS-ALPN challenge uses port 443, as explained in [Caddy's ACME challenge documentation](https://caddyserver.com/docs/automatic-https).

Finally, verify that no other service binds those ports. If a forgotten Apache or Nginx process holds either port, Caddy will fail to bind it. DNS validation can avoid opening ports for certificate validation, but public HTTPS still needs port 443 reachable, and HTTP-to-HTTPS redirects need port 80. DNS validation also requires provider credentials and a Caddy build that includes the provider's DNS module.

![A self-hosting operator checking a small server while visual DNS and network-port cues connect a public domain to the machine running Caddy; emphasize useful prerequisite checks rather than decorative code.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/6f8fdd2e-ea89-4b46-b906-5f362107764c/2-e0dc6c1bcee4.webp)

## Install Caddy and Write a Basic Caddyfile

Most Linux distributions offer official Caddy packages. Use the official Caddy installation instructions for your distribution rather than downloading generic binaries or relying on outdated default repositories. The official Debian and Ubuntu packages register Caddy as a systemd service, so Caddy restarts on boot and reads configuration from a predictable location.

That location is `/etc/caddy/Caddyfile`. Open this file and replace any default content with a minimal reverse proxy block.

```caddyfile
example.com {
    reverse_proxy 127.0.0.1:3000
}
```

Replace the placeholder domain with your production domain, and substitute the IP and port with the address where your upstream application listens. Using the loopback address works well when Caddy and the application share the same operating system host. The reverse_proxy directive tells Caddy to forward all incoming traffic for that domain to the specified address. The web server processes the request, waits for the upstream response, and proxies that response back to the client.

## Enable HTTPS and Test the Proxy

Caddy obtains and renews certificates for eligible hostnames automatically, and it redirects HTTP traffic on port 80 to HTTPS on port 443 by default. Public certificate issuance still depends on DNS and network access, as described in [Caddy's Automatic HTTPS guide](https://caddyserver.com/docs/automatic-https). Do not assume automatic HTTPS bypasses network routing rules.

Apply your configuration using the systemd service workflow. First, validate the syntax of your file.

```bash
sudo caddy validate --config /etc/caddy/Caddyfile
```

Validation reads the file and catches formatting errors before they break your live server. If the output confirms the file is valid, instruct the running service to load the new rules.

```bash
sudo systemctl reload caddy
```

A graceful reload applies the new configuration without stopping the service, so routine edits do not require a full stop and restart. Stopping Caddy instead can interrupt visitor traffic.

Test the public endpoint using a command-line HTTP client.

```bash
curl -v https://example.com/
```

The verbose output shows the TLS handshake and certificate details. If curl exits without a TLS verification error, the client trusts the certificate, which may be issued by Let's Encrypt or another configured authority. The response body should match what your upstream application serves. You now have a visitor-facing HTTPS site proxying to a local HTTP application.

## Run Caddy Beside a Dockerized App

When you run Caddy and your application in Docker containers, the host loopback address no longer works. Inside the Caddy container, the loopback IP points to Caddy itself, not the host machine or other containers.

To route traffic between containers, place both on the same Docker network. Use the application's Docker service name as the upstream address in your Caddyfile.

```caddyfile
example.com {
    reverse_proxy app:3000
}
```

In this configuration, Caddy uses Docker's internal DNS to resolve the service name `app`. Ensure the target port matches the internal port the application container listens on, rather than a port mapped to the external host. The upstream must be reachable from the Caddy process across the shared container network.

If you define these services in Docker Compose, the project already gets a default bridge network, so you don't need to declare a custom network for service-name discovery. If you need a different topology or want to control which services can communicate, define a custom network and attach Caddy and the app to it. The [Docker Compose networking guide](https://docs.docker.com/compose/how-tos/networking/) also explains service-name discovery and how recreated containers keep their service names even when their IP addresses change.

![Two containers on a shared private network, with outside traffic entering through Caddy and Caddy reaching the app through a service-to-service connection rather than localhost; use no labels or text.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/6f8fdd2e-ea89-4b46-b906-5f362107764c/3-ed448049cf0f.webp)

## Handle Client IPs and Forwarded Headers

Proxying changes how the upstream application sees network traffic: the connection comes from Caddy's internal IP, so applications that log the connection address rather than forwarded headers may record Caddy instead of the original visitor.

Caddy's `reverse_proxy` directive lets you set, add, delete, or replace request headers sent to the backend and response headers coming back from it, as described in [Caddy's `reverse_proxy` documentation](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy). To verify what your configuration sends and receives, inspect the HTTP headers during a test request.

By default, Caddy ignores client-supplied values for these forwarded headers. This prevents untrusted users from spoofing their IP address by injecting a fake header into their request. If you place a CDN like Cloudflare or Fastly in front of Caddy and need the original visitor address, configure trusted proxy ranges. When Caddy trusts the CDN connection, it can use the forwarded client information instead of attributing the request to the CDN's node IP.

You can manipulate headers further using specific directives. The `request_header` directive modifies headers before sending the request to the upstream, while the `header` directive modifies the response headers before Caddy sends them back to the client. Use these tools to add security headers like Content-Security-Policy or to strip internal debugging headers from the outbound response.

## Load Balance Multiple Upstreams

A single reverse proxy can distribute traffic across multiple instances of your application. When you list more than one address in the upstream configuration, Caddy balances requests using its configured policy.

```caddyfile
example.com {
    reverse_proxy 10.0.0.1:3000 10.0.0.2:3000 10.0.0.3:3000
}
```

The default policy chooses an upstream at random, while round-robin and least-connections policies are also available, as explained in the [Caddy `reverse_proxy` reference](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy). Active health checks and retries are available, but they require configuration. Enable health-check settings and a retry count or duration if you want Caddy to identify unhealthy upstreams and try another server after a failure. With retries enabled, Caddy can select another available upstream before returning an error, within the configured retry count or duration. A request matcher controls retries when the connection succeeds but the subsequent round trip fails, while connection failures can always be retried. This can help the proxy ride through brief backend interruptions.

## Configure WebSockets and Upstream TLS

WebSockets require persistent connections that survive beyond a single HTTP request. Caddy supports WebSocket connections natively through `reverse_proxy`, so a standard configuration does not need separate upgrade-header directives.

Front-end TLS remains separate from upstream TLS. A public HTTPS site can proxy to a local HTTP application, but only the visitor-to-Caddy connection is encrypted. The hop between Caddy and the application remains unencrypted unless you configure HTTPS or protect it through another mechanism. If your upstream application expects an encrypted HTTPS connection, verify that it has a trusted certificate and presents the right credentials.

Caddy proxies to an HTTPS upstream when you specify `https://` in the upstream address, and verifies the certificate using its configured trust store. Disabling upstream certificate verification leaves the connection vulnerable to man-in-the-middle attacks, so do not use that setting in production.

For HTTPS upstreams, override the `Host` header to match the TLS SNI value when the upstream uses that value for routing or certificate selection, as described in the [Caddy `reverse_proxy` documentation](https://caddyserver.com/docs/caddyfile/directives/reverse_proxy).

## Put Self-Hosted Swetrix Behind Caddy

Swetrix is an open-source, privacy-focused alternative to Google Analytics, and its self-hosted edition is cookieless. It tracks website traffic, sessions, and user flows, as described in the [Swetrix self-hosting repository](https://github.com/swetrix/selfhosting). You can self-host the platform using the repository's Docker Compose setup.

The default Swetrix deployment includes an internal `nginx-proxy` service. Keep this service in place and configure Caddy to sit in front of it, unless you have confirmed how its routes work and plan to recreate them in your Caddyfile.

If Caddy runs directly on your host machine, map the internal proxy to a host-only port to avoid conflicting with Caddy on port 80. For example, map the Swetrix proxy to `127.0.0.1:8080`.

```caddyfile
analytics.example.com {
    reverse_proxy 127.0.0.1:8080
}
```

Next, set `BASE_URL` to the URL where you want to access the Swetrix portal, because the [self-hosting instructions](https://github.com/swetrix/selfhosting) use this variable for the portal URL.

```env
BASE_URL=https://analytics.example.com
```

Once Caddy forwards requests to the internal proxy, test the dashboard and API routes through the public domain to confirm that the deployment's routing still works.

## Troubleshoot Common Caddy Proxy Failures

When a proxy configuration fails, the HTTP status codes and server logs often point toward the misconfiguration. Reading these signals can reduce guesswork as you identify which specific endpoints are failing.

| Error Code or Symptom | Common Cause | Resolution Step |
|---|---|---|
| 502 Bad Gateway | Caddy cannot reach the upstream application. | Check application status and network address. |
| 526 Invalid SSL | Cloudflare cannot validate the origin certificate. | Check the origin certificate and Cloudflare's TLS validation settings. |
| Browser Privacy Error | Caddy failed an ACME challenge. | Verify DNS records and challenge reachability. |
| Local IPs in Logs | The application ignores forwarded IP headers, or Caddy does not trust a preceding proxy. | Check the app's header settings and configure trusted proxy ranges. |

A 502 Bad Gateway response indicates that Caddy cannot reach the upstream. Verify the application is running, then confirm the configured address is reachable from Caddy's environment. If Caddy runs in a container, it cannot reach a bare loopback address on the host, so you must use the host's internal network IP or the appropriate Docker service name.

Start by checking the DNS records required by the selected ACME challenge, since Caddy's HTTP and TLS-ALPN challenges look up A/AAAA records while the DNS challenge checks a TXT record. The HTTP challenge requires port 80 to be externally accessible, and TLS-ALPN requires port 443; DNS validation requires configuration but no open ports. If Caddy cannot listen on port 80, forward port 80 to its HTTP port for the HTTP challenge, as described in [Caddy's Automatic HTTPS documentation](https://caddyserver.com/docs/automatic-https).

You can view the certificate failure reasons by reading the systemd logs. Run `journalctl -u caddy --no-pager | less` to scan the recent output. Caddy logs its ACME challenge attempts in detail, showing which authority rejected the request and why.

If you edit the Caddyfile and see no changes in server behavior, verify that the active service is reading the file you edited. The standard Linux installation reads `/etc/caddy/Caddyfile`. If you run a custom command manually, you might be loading a file from your home directory instead. Check the path, validate the syntax, and run the graceful reload command again.

When visitor IP attribution shows local network addresses in your application logs, review your proxy chain. Ensure the upstream application actively reads the `X-Forwarded-For` header. If traffic passes through a CDN before reaching your server, configure Caddy to trust that specific provider's proxy range. Otherwise, Caddy will use the CDN node's IP address as the connecting client address.

## Verify the Configuration

Verification requires watching live traffic hit the application. Tail the logs of your upstream service and load the public URL in a private browser window. With the proxy chain configured correctly, you should see a successful HTTPS connection, followed by the upstream service logging the client address passed through the forwarding headers. Test a few application routes to confirm that static assets load and API endpoints respond correctly.

A working proxy configuration forms the foundation of any self-hosted infrastructure. You provide the domain and application, and the web server manages the encryption, routing, and header adjustments. This frees your application layer from handling transport security.

---

With the routing secured, you can focus on the data your applications generate. Swetrix offers cookieless web analytics for traffic, sessions, and user flows. If you want to see how visitors interact with your newly proxied applications, try [Swetrix](https://swetrix.com).
