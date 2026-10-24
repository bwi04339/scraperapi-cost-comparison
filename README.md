# scrape website to csv: 4 Methods Compared, What Each One Really Costs, and the Fastest Way to Get Spreadsheet-Ready Data

Most people searching "scrape website to csv" want the same thing: a table of product prices, search results, listings, or reviews that opens cleanly in Excel or Google Sheets. The honest answer is that the right method depends entirely on scale. For one page, your spreadsheet app might do it alone. For 10,000 product pages behind JavaScript and bot protection, you need an API that handles proxies and CAPTCHAs for you — and that's where a service like ScraperAPI comes in.

This article walks through the four workable methods, then goes deep on the API route: which ScraperAPI plans actually make sense, what a request really costs in credits (the part almost nobody explains before you get a surprise bill), and the fastest way to pull structured CSV output from Amazon, Google, Walmart, and beyond.

> **Quick pick:** If you're pulling recurring data from Amazon, Google, Walmart, eBay, or similar sites and want CSV without writing parsers, the **Hobby plan at $49/month (100,000 credits)** covers most solo projects, and you can test everything on a 7-day free trial with 5,000 credits, no credit card required. Grab it here: 👉 [Start a free 7-day trial with 5,000 credits](https://www.scraperapi.com/?fp_ref=coupons)

## Four ways to scrape a website to CSV, and when each one works

There are four practical routes, and picking wrong wastes either your afternoon or your budget. Here's the honest comparison:

| Method | Best for | Cost | Where it breaks |
| --- | --- | --- | --- |
| Spreadsheet import (paste URL or use built-in web queries) | Simple public HTML tables | Free | Fails on JavaScript-rendered pages, paginated lists, anything with bot protection |
| Point-and-click Chrome extension (e.g., Agenty's scraping extension) | One-off list pages you can click through | Free tiers exist, paid tiers for scheduling | Struggles with heavy anti-bot sites and bulk volume |
| Python script (requests + BeautifulSoup + csv module) | Custom logic, full control | Free, but your time | Breaks on CAPTCHAs, needs proxy management, requires ongoing maintenance when page layouts change |
| Scraping API (ScraperAPI and similar) | Recurring, bulk, or protected targets | Paid, credit-based | Overkill for a single page you'll scrape once |

The Python route deserves a note. It's genuinely the right answer if you're a developer scraping a friendly, static site — `requests` for the HTTP call, `BeautifulSoup` for parsing, the built-in `csv` module to write the file, done in twenty lines. The problem shows up the moment a target site renders content with JavaScript, rotates layouts, or sits behind Cloudflare. Then you're maintaining a scraper instead of analyzing data.

No-code tools and extensions occupy the middle ground. They're fast for a one-time extraction — paste a URL, preview detected columns, download the .xlsx or .csv. If your entire project is "grab this one table once," stop reading and use one of those. Everything below is about the scenario where that workflow collapses: repeated runs, thousands of URLs, or sites that actively fight scrapers.

## When free tools stop working: JavaScript, CAPTCHAs, and volume

A scraping API earns its price in three specific situations, and it's worth naming them precisely.

**JavaScript rendering.** Huge chunks of the modern web ship empty HTML and fill it in with client-side code. A `requests` call gets you a blank shell. ScraperAPI renders the page in a headless browser before returning it — that's a 10-credit add-on per request, available on every plan.

**Anti-bot protection.** Sites behind Cloudflare, Datadome, or PerimeterX will block a datacenter IP before the HTML even loads. ScraperAPI routes those requests through its proxy pool (40M+ IPs across 50+ locations) and charges 10 extra credits when its bypass logic kicks in.

**Scale and reliability.** This is the less obvious one. When Amazon changes a page layout — which happens — a DIY scraper silently breaks. ScraperAPI's structured data endpoints come with the promise that their team "will help by identifying and handling any page structure changes," with reported success rates up to 99.99% on supported domains. You're paying someone else to keep the pipes unblocked.

If none of those three apply to your task, don't buy anything. Seriously.

## Getting a CSV out of ScraperAPI: three routes

ScraperAPI gives you three distinct paths from raw web page to structured file. Which one applies depends on your target site.

### Route 1: Structured Data Endpoints (the recommended path for supported sites)

For a fixed set of domains, ScraperAPI maintains pre-built endpoints that return clean, fielded data directly — no selector writing, no HTML parsing. Adding `output_format=csv` to the request returns the parsed result as CSV instead of JSON, which drops straight into Excel or Google Sheets.

What's covered and what each request costs:

| Domain | Endpoints available | Credits per request |
| --- | --- | --- |
| Amazon | Product details, search results, offers/sellers | 5 |
| Walmart | Product, search, category, reviews | 1 |
| Google | Search/SERP, Shopping, News, Jobs, Maps | 25 |
| eBay | Product, search | 5 (e-commerce rate) |
| Redfin & other real estate | Sale pages, search, rentals, agent details | varies by domain |
| LinkedIn / Indeed | Job listings | 30 (LinkedIn) |
| Others | Etsy, YouTube, Reddit, G2, Trustpilot, GitHub, app stores, and more | varies by domain |

Two details here that most comparison articles skip. First, **credit costs are set by the domain, not the endpoint** — an Amazon search request costs the same 5 credits as an Amazon product page. Second, **Walmart structured endpoints run at the standard 1-credit rate**, which makes Walmart one of the cheapest bulk targets on the platform. If you're comparing prices across retailers, that asymmetry matters for your bill.

A real example of what comes back from the Amazon Product endpoint: name, price (`"$99.00"`), package dimensions, weight, ASIN, image URLs, variant options with their own ASINs, average rating (3.8) with ratings count (35), best-seller rank, and first-available date. That's one request, one fielded record, ready for a spreadsheet.

👉 [Browse the full structured data endpoint catalogue](https://www.scraperapi.com/solutions/structured-data/?fp_ref=coupons) to check whether your target site is covered before picking a plan.

### Route 2: The autoparse parameter

Older and simpler: add `autoparse=true` to a standard API request and get parsed structured JSON back instead of raw HTML, then choose `output_format=json` or `output_format=csv`. It works on the same five core domains (Amazon, eBay, Walmart, Google, Redfin). One caveat from the docs themselves: ScraperAPI now recommends the structured data endpoints over autoparse. Autoparse makes sense when you need a custom URL — for instance, a specific Amazon product URL format — rather than the endpoint's standard parameters. Both sync (single GET) and async (POST to the async jobs endpoint) modes support it.

### Route 3: AI Parser for everything else

This is the piece that changed the calculus for non-listed sites. Released in June 2026, the AI Parser takes up to three example URLs of the same page type — say, three product pages from a catalog — and generates a custom parser, typically in under a minute. You then send any similar URL from that site to `GET /parse/{parser_id}` and get structured data back, with fields the AI identified automatically (or fields you define by name, description, and data type, with optional CSS/XPath selectors if you want to steer it).

Pricing details worth knowing before you plan around it:

- Every account gets **one parser free**
- Each additional parser costs **30,000 API credits**
- Parsing requests bill at standard scraping rates through the existing infrastructure
- Parsers have version history, so you can pin production to a known-good version and update fields without breaking integrations
- Standard parameters (`render`, `premium`, `ultra_premium`, `country_code`) work with it, so JS rendering and geotargeting apply

So the full picture for "scrape website to csv" is: structured endpoints for the big supported domains, autoparse for custom URLs on those same domains, and AI Parser for the long tail of everything else. For scheduled, recurring exports, the DataPipeline layer adds CRON-style scheduling with webhook delivery and downloadable JSON/CSV files, handling up to 10,000 listings, queries, or URLs per project.

## ScraperAPI plans: the complete current lineup

Here's every plan currently listed on the official pricing page, including the free tier. All paid plans include JS rendering, premium proxies, CAPTCHA handling, rotating proxy pools, custom headers and sessions, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee.

| Plan | Monthly price | Annual (per month) | Credits/mo | Concurrent threads | Get started |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | 5 | [ Claim 1,000 free credits](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [ Start Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [ Start Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [ Start Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling (Most popular) | $475 | $427.50 | 5,000,000 | 200 | [ Start Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [ Start Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [ Start Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22M+ | 500+ | [ Contact sales](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Beyond the headline numbers, three plan-level features separate the tiers in ways that affect real projects:

- **Geotargeting**: Hobby and Startup are limited to US and EU locations. Country-level global targeting (50+ countries) starts at Business. If your data has to come from APAC or Latin America, the cheap tiers won't do.
- **Analytics history**: 30 days on the lower tiers, unlimited from Business up.
- **Pay-as-you-go overages**: only Scaling and above let you keep scraping past your monthly credit allowance at a fixed per-credit rate, up to a spending cap you set. On Hobby through Professional, there's an opt-in auto-upgrade instead — when credits run out, the account moves to the next plan automatically.

👉 [See the current plan line-up and switch between monthly and annual pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## What a request really costs: credits, not pages

Every comparison article quotes "100,000 credits" and stops there. That number is misleading until you know the multipliers, because **a request and a credit are not the same thing**.

The base rates: a standard page costs 1 credit. Amazon costs 5. Google and Bing (including their subdomains) cost 25. LinkedIn costs 30. On top of the base rate, features stack: JavaScript rendering adds 10 credits, premium residential proxies add 10, ultra-premium proxies add 25, and bypassing heavy bot protection adds another 10.

Run the math on the Hobby plan and the picture changes fast. 100,000 credits at 1 credit per request is 100,000 plain-HTML pages a month — plenty. But if every page needs JS rendering, that's 10 credits a request, and your real capacity is 10,000 pages. Scrape a JS-rendered Amazon category at the 5-credit e-commerce base rate plus 10 for rendering, and you're at 15 credits per page, or roughly 6,600 pages before the plan runs dry. None of this is hidden — the dashboard includes a Domain Cost Estimator that tells you the exact cost of any URL before you fire it — but you have to go looking for it, and most people don't.

Two consumer-friendly policies soften the edges. Failed requests don't cost credits, so a batch of timeouts doesn't eat your allowance. And you can set a `max_cost` per scrape to cap what any single request can burn — useful when a page unexpectedly triggers the 10-credit protection bypass.

## First-year cost: monthly versus annual billing

Annual billing knocks 10% off every plan, which translates to real money at scale:

| Plan | Monthly billing (12 mo) | Annual billing (12 mo) | You save |
| --- | --- | --- | --- |
| Hobby | $588 | $529.20 | $58.80 |
| Startup | $1,788 | $1,609.20 | $178.80 |
| Business | $3,588 | $3,229.20 | $358.80 |
| Scaling | $5,700 | $5,130 | $570 |
| Professional | $11,700 | $10,530 | $1,170 |
| Advanced | $23,700 | $21,330 | $2,370 |

If you're still evaluating, start monthly. The trial plus one monthly cycle gives you a real usage profile — you'll know your average credits-per-page from the Domain Cost Estimator, and then you can commit to annual with actual numbers instead of guesses.

## Who shouldn't buy this

Fairness cuts both ways, so here's the honest exclusion list. If you need one CSV file from one public page, use your spreadsheet app or a free extension — a $49/month plan is spending money to avoid twenty minutes of free work. If your targets are entirely outside the supported endpoint list and you only need a few hundred pages monthly, a Python script with a residential proxy may cost less than Hobby, provided you're comfortable maintaining it. And if your budget is strictly zero, the free plan's 1,000 credits plus the 7-day trial's 5,000 will carry a genuine small project before you ever pay.

Where ScraperAPI clearly wins is the middle ground: recurring jobs, thousands of URLs, JS-heavy or protected sites, and anyone who wants CSV output without owning parsing logic forever.

## FAQ

**Does the CSV output work on any website?**
No. `output_format=csv` applies to the autoparse parameter and structured data endpoints, which cover Amazon, eBay, Walmart, Google, and Redfin (plus a longer list of specialist endpoints). For other sites, the AI Parser produces structured data you can convert to CSV, but the direct CSV format parameter is tied to the supported domains.

**Is there a genuinely free option?**
Yes, two of them. A 7-day trial with 5,000 API credits and no credit card required, plus a permanent free account with 1,000 credits and 5 concurrent connections. If you need extra test credits, support can grant them.

**What happens when I hit my credit limit mid-month?**
On Hobby through Professional, opt-in auto-upgrade moves you to the next plan automatically. Scaling and above get true pay-as-you-go: keep scraping at a fixed per-credit rate up to a monthly spending cap you define. If you've set no auto-upgrade and no overage, requests stop when credits run out.

**How accurate is the data from structured endpoints?**
ScraperAPI reports up to 99.99% success rates on supported domains, with their team maintaining the parsers as page layouts change — that maintenance burden is the main thing you're buying.

## Before you pick a plan

Match your situation to a starting point: one-off small extraction → don't pay, use the free tools; a recurring Amazon/Walmart/eBay/Google data feed in CSV → Hobby at $49/month, and check your pages' credit cost in the estimator first; unpredictable monthly volume that might spike → Scaling, because pay-as-you-go beats being locked out mid-run; multi-region data outside US/EU → Business minimum.

One last practical tip: run your trial against your actual target URLs and check the per-request credit cost before committing to a tier. The plan that looks cheapest on paper is sometimes the wrong one once rendering and protection multipliers apply. When the numbers check out, 👉 [claim your free 5,000-credit trial and test your first CSV export today](https://www.scraperapi.com/?fp_ref=coupons).
