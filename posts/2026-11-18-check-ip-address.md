---
title: "Check Your IP Address Fast"
intro: "Check the public IP a website sees, find your local address on Windows or Mac, and use it to exclude your own Swetrix visits."
date: November 18, 2026
image: "https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/4bb9f3a0-d07c-4820-9be0-c2f515d02d44/0-18fc3e58aafe.webp"
hidden: false
author: "Andrii Romasiun"
twitter_handle: "andrii_rom"
rankpine_id: "4bb9f3a0-d07c-4820-9be0-c2f515d02d44"
---

When you test a new landing page or review a live campaign, your own clicks generate traffic data. Removing that internal activity from your analytics dashboard requires knowing the outward-facing address your browser leaves behind. You can check your IP address quickly using a browser-based tool, but the result often differs from the network information shown in your computer settings. Bridging that gap means understanding which connection faces the wider internet and which one manages your home or office equipment. 

Once you locate the correct public address, you can configure platforms like Swetrix to ignore your device. This keeps your traffic metrics accurate and prevents your own testing from inflating pageview counts. The process involves identifying the exact connection reaching the tracking server, understanding why local networks disguise that connection, and applying the right exclusion rule for your team.

## Check your public IP address

The fastest way to find the address an external server sees is to ask a server to read it back to you. When your browser requests a webpage, the request packet includes a return address so the destination knows where to send the text and images. An online lookup utility reads that return address from the incoming request and displays it on your screen.

You can view your current outward-facing connection by opening the Swetrix IP lookup tool linked from the platform's [exclusion documentation](https://swetrix.com/docs/exclude-own-visits). The address displayed on that page is the public IP address your browser presented to the server. This quick check requires no terminal commands or digging through control panel settings, providing an immediate result that reflects the active connection your browser is using at that moment.

The address you see there belongs to your router, your company gateway, or your virtual private network provider. The wider internet uses this sequence to route data packets across global infrastructure. If you load a website from your phone while connected to a cellular network, the lookup tool displays the mobile carrier's assigned address. If you connect that same phone to your home Wi-Fi network and reload the page, the tool displays the home internet service provider's address, because the lookup always reports the final point of origin for the request reaching it.

## Public IP vs. local IP

Many people open an online checker, read the result, and then compare it to the network details on their laptop, only to find two completely different sets of numbers. This mismatch happens because modern networks separate external routing from internal device management. 

Your internet service provider assigns one public address to your modem. Your wireless router then takes that single connection and creates a private network to share it across your laptop, phone, television, and smart speakers. To manage traffic among these devices, the router assigns a private, local IP address to each one.

| Address Type | What it describes | Where to find it | When it matters |
| :--- | :--- | :--- | :--- |
| **Public IP** | The outward-facing connection your network uses to access the internet. | Browser-based lookup tools. | Whitelisting server access, excluding analytics traffic, or configuring external DNS. |
| **Local IP** | The internal connection a specific device uses on a private network. | Device system settings or command line tools. | Troubleshooting a printer, configuring local file sharing, or setting up a local development server. |

Local IPv4 addresses follow allocation rules documented decades ago. Those rules appear in [RFC 1918](https://datatracker.ietf.org/doc/rfc1918/), an IETF best-current-practice document that lists the ranges IANA reserved for private internets: `10.0.0.0/8`, `172.16.0.0/12`, and `192.168.0.0/16`. Because these addresses have no global meaning and RFC 1918 says routes for private networks should not be propagated between enterprises, separate private networks can reuse `192.168.1.1` without causing global routing conflicts.

When your computer tries to load a webpage, it sends the request from its `192.168.x.x` local assignment to the router. The router translates the packet's private source address to the network's public address and forwards the packet to the destination server. When the server replies, the router remembers which local device initiated the request and sends the data back to the correct private assignment. Because this translation happens inside your home or office hardware, an external website never sees the local address.

![An editorial cutaway of a home Wi-Fi setup, with a laptop and phone sharing a router and one route extending from the home network to the wider internet, clarifying the local-versus-public distinction without labels.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/4bb9f3a0-d07c-4820-9be0-c2f515d02d44/1-89d42d9b51ff.webp)

## Find your local IP on Windows or Mac

While an online checker is the right tool for finding the public address, you will need the local assignment for tasks restricted to your immediate network. Configuring a wireless printer, testing a local web application, or mapping a shared network drive requires knowing the exact internal address assigned to your computer. 

Finding this information means checking the specific network interface connecting your device to the local router. The operating system handles these assignments dynamically, meaning the laptop might receive a different internal number if the router restarts or the wireless lease expires. 

### Windows

In Windows 10 and 11, open Settings and select Network & internet. Select Wi-Fi and the network you're connected to, or select Ethernet. To view connection details, select Properties near the connected network and look for the IP address listed next to IPv4 address. That gives you the connection's IPv4 address. If you need to troubleshoot further, the official [Microsoft network connectivity guide](https://support.microsoft.com/en-us/windows/experience/connectivity-networking/essential-network-settings-and-tasks-in-windows) explains how to change TCP/IP settings.

### Mac

Apple organizes network interfaces through System Settings on modern macOS releases. Open the Apple menu, select System Settings, and click Network in the sidebar. Select the active service, which is usually Wi-Fi or Ethernet, to view the connection status.

Clicking Details for a connected Wi-Fi network shows its IP and router addresses. The [Apple Wi-Fi settings documentation](https://support.apple.com/en-mide/guide/mac-help/mh11935/27/mac/27) also says the Details view lets you configure DNS servers and proxies.

## Why your IP result may look different

Even after you check a browser-based utility to find your outward-facing address, the resulting number or location might seem incorrect. Practical network routing often places intermediary servers between your device and the destination website. 

Virtual private networks and web proxies intentionally change the connection address the checker sees. When you activate a VPN client, your computer establishes an encrypted tunnel to a remote server. The remote server then retrieves webpages on your behalf. Because the VPN server makes the final request to the external website, the IP lookup tool displays the VPN provider's address rather than your home ISP assignment. If you select a VPN endpoint in London, the checker will report a London-based origin, masking your physical presence.

Internet providers use carrier-grade NAT (CGN) to share scarce public IPv4 addresses among subscribers. The [RFC 6598 specification](https://datatracker.ietf.org/doc/rfc6598/) identifies `100.64.0.0/10` as Shared Address Space for CGN, separate from RFC 1918 private ranges. When multiple subscribers share a public IPv4 address through CGN, destination servers see the same public source address for their traffic.

MaxMind's GeoIP data provides approximate location information. The [MaxMind accuracy explainer](https://support.maxmind.com/knowledge-base/articles/maxmind-geolocation-accuracy) says its data isn't precise enough to identify a specific street address, household, or person. Because accuracy varies by location and IP type, city-level results remain estimates.

![A small agency team working on devices connected to the same office network while one person tests a website, illustrating why blocking a shared public IP can exclude colleagues’ visits too.](https://cdn.rankpine.com/website/8df9bdef-394e-4e49-a723-5b18608373fb/article/4bb9f3a0-d07c-4820-9be0-c2f515d02d44/2-e83e22581dd5.webp)

## Use your public IP to exclude your own Swetrix visits

You rely on accurate traffic data to evaluate content performance and conversion rates. Operating a [cookieless Google Analytics alternative](https://swetrix.com/google-analytics-alternative) allows you to respect user privacy while retaining advanced product analytics, but it also means standard platform visits track visitors without invasive consent banners. If you spend an afternoon adjusting a layout, clicking through your own navigation menus generates legitimate pageviews and session replays. Maintaining clean reporting requires configuring the tracking platform to ignore this internal activity.

Excluding this traffic requires the public address, not the local one. Telling an external server to ignore `192.168.1.5` achieves nothing, because the tracking server only ever interacts with the public connection managing your network. 

### Exclude a network or individual browser

Swetrix provides a dedicated mechanism for discarding incoming data from specific origins. You can implement this block by logging into your dashboard, navigating to Project Settings, opening the Protection tab, and selecting Shields. The Blocked IP addresses field accepts the public number you retrieved from the lookup tool. 

When you save this configuration, the platform drops any incoming hits originating from that address before they reach your reporting dashboards. Swetrix supports both standard IPv4 numbers and the newer, longer IPv6 formats. If your internet provider uses a dual-stack configuration that routes traffic over both protocols simultaneously, you may need to add both public addresses to the blocklist. 

Before blocking the network, consider who else shares that connection. Applying a block on a shared office network excludes your visits, but it also excludes visits from any colleague connected to the same corporate Wi-Fi. If an agency team tests a client website from the same building, blocking the building's public IP prevents the entire staff from triggering conversion events during quality assurance. 

Dynamic assignments also require maintenance. If your residential provider issues a new public address after a modem reboot, your exclusion rule will stop working until you update the platform with the new digits. When blocking the entire network creates too much friction, you can exclude only your specific device. Setting a browser-exclusion cookie instructs the platform to ignore activity from that single browser, regardless of which network you join. You can verify this setup using a [cookie checker](https://swetrix.com/tools/cookie-checker) to confirm the exclusion token is present on your machine, allowing colleagues to continue tracking their own tests while your sessions remain hidden. 

### For server-side events, preserve the visitor IP

Many analytics setups move tracking logic away from the browser entirely. B2B applications and complex SaaS platforms frequently trigger analytics events from their backend databases or content delivery networks. While this architecture improves client-side performance, it changes how tracking platforms read origin data.

When a backend server sends an event directly to the Swetrix API, the tracking platform sees the backend server's IP address rather than the end user's connection. If five hundred users trigger a purchase event, the system logs five hundred hits originating from a single cloud hosting facility. This centralization breaks geolocation mapping and distorts unique visitor counts. 

To maintain accurate location and session data during server-side implementations, your backend code needs to forward the original visitor IP in the request headers. The [Swetrix Events API reference](https://swetrix.com/docs/events-api) details the specific payload structures required to pass the original user data through a proxy or server architecture. Forwarding this data ensures the reporting dashboard correctly attributes the action to a user in Berlin rather than a data center in Frankfurt.

Handling this data requires strict adherence to privacy frameworks. Swetrix operates a privacy-first infrastructure that avoids invasive data collection. As outlined in the [Data Policy](https://swetrix.com/data-policy), the platform's standard analytics engine uses the incoming request IP transiently in memory to generate an anonymized session identifier. It does not store raw IP addresses in its database or system logs. This transient processing provides accurate session continuity without retaining identifiable network footprints. Site operators retain the responsibility to ensure no identifiable personal information inadvertently passes through custom event payloads, URL parameters, or server-side metadata headers. 

Monitoring network assignments allows you to maintain control over your analytics environment. By distinguishing between the internal numbers managing your hardware and the public connections facing the internet, you can apply accurate filtering rules, maintain pristine data quality, and confidently measure how real users interact with your content. 

---
Stop polluting your own analytics data during website testing. Swetrix offers a privacy-first, cookieless tracking environment with built-in tools for IP filtering, session replays, and conversion funnels. Try the [Swetrix analytics platform](https://swetrix.com) today to see exactly how real users navigate your site without compromising their privacy.
