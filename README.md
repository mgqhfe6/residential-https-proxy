# residential https proxy: what the HTTPS label really changes, and how to size a 9Proxy plan without overpaying

Most people searching this term have the same actual problem, and it isn't the one the phrase suggests. They have a target site that only answers over HTTPS, and they want those requests to leave from a household IP instead of a datacenter range. The word "HTTPS" feels like the important part. It usually isn't. Protocol choice is a five-minute detail; the plan you buy is where the money and the frustration live.

So here's the order that actually helps: understand what the HTTPS half means, know what to check before paying, then look at what 9Proxy charges for each usage model.

## The HTTPS part is two different things, and mixing them up costs money

When someone says "residential HTTPS proxy," they could mean either of these:

**1. Proxying HTTPS traffic (the common case).** Your client sends a `CONNECT` request to the proxy, the proxy opens a TCP tunnel to the destination on port 443, and TLS is negotiated end-to-end between your client and the site. The proxy sees the hostname and the amount of data, not the contents. This is what happens when you point Python's `requests`, Node's `axios`, `curl`, or a browser at an HTTP proxy and then load an `https://` URL.

**2. Speaking TLS to the proxy endpoint itself.** Here the proxy URL itself starts with `https://`, so the client-to-proxy hop is encrypted too. Oxylabs documents this as a separate capability, and Zenrows notes that most clients don't need it: their HTTP port is enough for the majority of setups, and the dedicated HTTPS port exists for clients that specifically require one. Some libraries and older tools simply can't connect to an HTTPS-form proxy endpoint at all.

The practical difference: if you only need HTTPS destinations to load, an HTTP proxy endpoint handles it. If your client insists on `https://` in the proxy string, you need a provider that exposes that. 9Proxy supports HTTP, HTTPS and SOCKS5, which covers both readings of the term.

SOCKS5 deserves one warning. It's the more flexible protocol since it tunnels arbitrary TCP traffic, but some sites treat SOCKS5 traffic differently and flag it, and Chrome doesn't support SOCKS5 authentication at all — you'd need Firefox or an HTTP client there.

None of that is why requests fail against protected targets, though. What fails is the egress IP's reputation and how long a session holds. Cloudflare-protected sites score IP type, IP history and connection behaviour, which is the real argument for residential over datacenter, not the scheme in your config file.

## What to check before you buy any residential HTTPS proxy

The feature list on most provider pages is nearly identical. The differences that bite appear later. These are the questions worth answering first:

- **Protocol set.** Does it expose HTTP, HTTPS and SOCKS5, or just one endpoint type? If your stack needs SOCKS5 for non-HTTP traffic, one HTTP-only endpoint is a dead end.
- **Session model.** Rotating per request, or sticky for minutes/hours? Logins, carts and multi-step flows need the same IP across requests.
- **Authentication.** Username and password, IP whitelist, or a desktop app doing local port forwarding? The third option is fine but means your workload has to run on a machine where that app is installed.
- **How long a single IP stays alive.** This varies wildly between providers and between product models at the same provider.
- **How you're billed.** Per IP with unlimited bandwidth, per GB consumed, or both. The wrong model can double your cost on the same traffic.
- **Validity window.** Do your unused units expire in a month, in 180 days, or never?
- **Geo-targeting depth.** Country only, or country/state/city/ZIP/ISP?
- **Replacement and refund terms.** What counts as a dead IP, and how do you get it swapped?

## How 9Proxy lines up against that list

9Proxy is a residential-only network: 20M+ residential IPs across 90+ countries, with targeting down to country, state/province, city, ZIP code and ISP. Independent coverage of the pool consistently lands in that 20M/90-country range, though directory pages elsewhere quote everything from 9M to 95M, so treat headline pool numbers as advertising on any provider.

Other things that are documented and verifiable:

- **Two product models.** Residential by IPs (pay per IP, unlimited traffic per IP, unused IPs never expire) and Residential by GB (pay per GB, unlimited endpoints, sticky or rotating sessions, 180-day traffic validity, unlimited validity on Enterprise).
- **Targeting that binds to a port.** You can pin a country/state/city/ISP selection to specific local ports, which is useful when different tasks need different geos at the same time.
- **A public API and docs** for generating proxy lists, rotating IPs, checking wallet balance and managing sub-users.
- **Integrations** with anti-detect browsers such as ixBrowser and Multilogin, plus app-level routing through Proxifier, and a browser-based tool for generating and routing proxies without a local install.
- **Payments** via cards, Apple Pay, Google Pay, Alipay and crypto (BTC, ETH, LTC, TRX, USDT on TRC20 and ERC20, DOGE, DAI, BCH). Crypto payments carry an automatic +5% IP bonus.
- **Referral signup discount.** Accounts created through a referral link or code get 5% off their purchases, and the discount applies to later purchases too, not just the first one.

Performance numbers need a caveat. 9Proxy publishes figures around 99.5% success rate, ~0.6s average response time and 99.95% uptime. Those are vendor claims, not an SLA. A third-party review aggregating its own tests and lab reports puts success rate near 97% with P95 latency around 1.3s for rotating residential, and individual US IP latency in the 0.8–1.4s range. Realistic enough for scraping and automation; not a promise.

## 9Proxy plans and prices

One thing to know before reading the table: on June 1, 2026, 9Proxy adjusted pricing for IP-Based and Bundle packages for the first time since launch. GB-Based pricing was left alone. The figures below are the post-adjustment ones, and packages are one-off purchases rather than monthly subscriptions.

### Residential by IPs (unlimited bandwidth per IP)

| Package | Effective rate | Price | Traffic | Validity of unused IPs | Get it |
| --- | --- | --- | --- | --- | --- |
| 100 IPs | $0.24/IP | $24 | unlimited | never expires | Buy the 100-IP pack |
| 500 IPs | $0.144/IP | $72 | unlimited | never expires | Buy the 500-IP pack |
| 1,000 IPs + 500 bonus | $0.084/IP | $126 | unlimited | never expires | Buy 1,000 IPs with 500 bonus |
| 2,500 IPs | $0.084/IP | $210 | unlimited | never expires | Buy the 2,500-IP pack |
| 5,000 IPs | $0.072/IP | $360 | unlimited | never expires | Buy the 5,000-IP pack |
| 15,000 IPs | $0.048/IP | $720 | unlimited | never expires | Buy the 15,000-IP pack |
| 25,000 IPs | $0.035/IP | $863 | unlimited | never expires | Buy the 25,000-IP pack |
| 50,000 IPs | $0.029/IP | $1,438 | unlimited | never expires | Buy the 50,000-IP pack |
| 100,000 IPs (Business) | $0.023/IP | $2,300 | unlimited | never expires | Check Business IP pricing |
| 200,000 IPs (Business) | $0.021/IP | $4,140 | unlimited | never expires | Check Business IP pricing |
| 500,000 IPs (Business) | $0.018/IP | $8,625 | unlimited | never expires | Check Business IP pricing |

Each IP in this model stays usable for a few hours up to roughly 24 hours, depending on the individual IP. Generation and port binding run through the 9Proxy desktop app (Windows and Mac), with optional proxy authentication on top of local port forwarding.

### Residential by GB (rotating endpoints, bandwidth-metered)

| Package | Rate | Price | Validity | Get it |
| --- | --- | --- | --- | --- |
| 5 GB | $3.00/GB | $15 | 180 days | Buy the 5 GB pack |
| 50 GB + 5 GB bonus | $2.10/GB | $105 | 180 days | Buy the 50 GB pack |
| 100 GB | $1.50/GB | $150 | 180 days | Buy the 100 GB pack |
| 200 GB | $1.00/GB | $200 | 180 days | Buy the 200 GB pack |
| 1,000 GB | $0.80/GB | $800 | 180 days | Buy the 1,000 GB pack |
| 2,000 GB | $0.75/GB | $1,500 | 180 days | Buy the 2,000 GB pack |
| 3,000 GB (Enterprise) | $0.72/GB | $2,160 | no expiry | Check Enterprise GB pricing |
| 6,000 GB (Enterprise) | $0.70/GB | $4,200 | no expiry | Check Enterprise GB pricing |
| 10,000 GB (Enterprise) | $0.68/GB | $6,800 | no expiry | Check Enterprise GB pricing |

This model generates unlimited endpoints, so you never count IPs. You pay only for the data that moves. Enterprise adds team mode (one owner plus up to five members), shared bandwidth without expiry inside the team, per-member traffic caps and activity logs.

### Bundle packages (IPs plus GB together)

| Package | Contents | Price | Get it |
| --- | --- | --- | --- |
| Starter | 100 IPs + 5 GB | $30 | Buy the Starter bundle |
| Popular | 1,500 IPs + 50 GB | $180 | Buy the Popular bundle |
| Pro | 5,000 IPs + 500 GB | $720 | Buy the Pro bundle |

Bundles matter more than they look. Unused bundle traffic is valid for 180 days, so a project that goes quiet for a month doesn't burn the balance.

## Which model you actually want

The billing model should follow how your workload consumes resources, and there's a simple way to think about it.

If your jobs move a lot of data through a small number of IPs, per-IP pricing wins by a wide margin, because bandwidth is unmetered. One IP pushing 50 GB costs $0.24 on the 100-IP pack. The same 50 GB at $3.00/GB costs $150. Scraping image-heavy listings, downloading large documents, or running long authenticated sessions all sit here.

If your jobs rotate constantly and each request is small, per-GB wins. Lightweight HTML scraping, ad verification, geo-checking and API polling all burn a few kilobytes per request but want hundreds of different exit IPs. Paying per IP for that is paying for capacity you don't use.

Mixed workloads are the reason bundles exist. If half your pipeline maintains stable logins and the other half sprays requests across the pool, the $180 Popular bundle (1,500 IPs + 50 GB) is cheaper than buying those separately at list price.

There's one more small saving that applies to the IP model: the Today List feature lets you reuse IPs from the previous 24 hours at no extra cost, which 9Proxy describes as a 20–30% reduction in consumption on typical workloads. Auto-refresh also swaps out IPs that go offline within about 60 seconds, which matters if you're not watching a dashboard all day.

## From dashboard to a working HTTPS request

Two paths, depending on which model you bought. Both start the same way: 👉 create an account through the referral signup link, which applies the 5% discount to your purchases.

**With GB-based plans,** everything happens in the dashboard. Create a sub-user, assign it traffic, then generate proxies while picking your target location (country, state, city, ZIP, ISP) and session behaviour. You'll get credentials to use with username/password authentication or, if you'd rather skip passwords, you whitelist your own device IP. The generated username encodes your targeting, in the format `<subaccount>-country-<country_code>-st-<state_code>-city-<city_code>-isp-<isp_code>-ssid-<session_id>-sst-<session_time>`. Copy the host and port, drop them into your client, and check the result against an IP echo endpoint before running anything real.

**With IP-based plans,** install the desktop app. You set a starting port and the number of ports, filter the available IPs by country, state, city, postal code or IP range, then bind the IPs you want to those ports. From there it's a local endpoint: `127.0.0.1` plus your chosen port. Turn on proxy authentication if you don't want anything else on the machine using it. For tools without native proxy support, Proxifier works well — add the proxy with type set to HTTPS or SOCKS5, enable authentication, and paste the sub-account credentials.

If your client absolutely requires an `https://` proxy string rather than an HTTP one, that's the point to confirm your endpoint form with support, since not every client library handles a TLS proxy hop.

## Limits, caveats and the parts reviews complain about

Honest list, because these are the things that generate refund requests:

- **It's residential only.** No datacenter, ISP or mobile products. If a target is easier to hit from datacenter IPs, this isn't the provider for it, and there's no managed scraper or unblocker API for teams that want the whole pipeline handled.
- **Streaming is off the table on IP-based plans.** Third-party coverage reports a policy shift under 9Proxy's Acceptable Use Policy that drops media streaming (YouTube being the example cited) on IP-based plans. If streaming is your use case, confirm the current terms before paying.
- **The credit policy is narrow.** ProxyLook's review reports published credit-refund terms essentially covering IPs that die within about 60 seconds, which is a narrower window than some competitors offer.
- **Standard GB balances expire in 180 days.** Only Enterprise GB packages are unlimited, and only IP-based packages have non-expiring IPs.
- **Free trials aren't advertised on the site.** In forum threads the company says it offers limited trials for new users depending on availability, and that you should specify whether you want an IP-based or GB-based trial. Practical move: ask support before buying if you want to test IP quality against your own targets first.

Everything above is checkable on the official pages, and plan prices do change. You'll see the live figures on the pricing page when you sign up: 👉 check current 9Proxy plans and pricing.

## Quick answers

**Does 9Proxy work for HTTPS sites?** Yes. It supports HTTP, HTTPS and SOCKS5, so HTTPS destinations load through a standard CONNECT tunnel, and clients that need an HTTPS-form proxy endpoint or SOCKS5 are covered too.

**Do I need SOCKS5 if my targets are HTTPS?** No. For HTTPS destinations, the HTTP endpoint is enough in most clients. Reach for SOCKS5 when you have non-HTTP traffic or a client that requires it — but remember Chrome can't do SOCKS5 authentication.

**How long does one residential IP last?** On IP-based packages, a few hours up to about 24 hours per IP. On GB-based packages, IPs rotate per request or per sticky session you configure.

**Does the invite link do anything besides sign me up?** The referral code attaches a 5% discount to your purchases, including repeat purchases, so it's worth using when you register rather than later. 👉 Start with the referral link and keep the 5% off.

The short version: if you're typing "residential https proxy" because a site rejected your datacenter IP, the HTTPS part is already solved by any decent client and a plain HTTP proxy endpoint. What you're actually buying is IP reputation, session control and a billing model that matches your traffic. Get those three right and 9Proxy's per-IP or per-GB structure will do the job for a low price; get them wrong and the cheapest plan on the page is still the wrong one.
