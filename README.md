# Link Prospecting Tool

Turn a list of search queries into a ready-to-use link-building outreach list.

This Actor handles the full list-building workflow:

1. **Scrape search results** - get results for your queries across Google Search, ChatGPT, Gemini, Copilot, and Perplexity.
2. **Filter already-mentioned domains** - remove any sources that already mention your brand.
3. **Enrich leads** - get contact details for each source so you can run cold outreach

You can now do all of this directly from the [Google Search Result Scraper](https://apify.com/apify/google-search-scraper)

## What data can I extract with Link Prospecting Tool?

This Actor returns multiple datasets:

1. **Domains with leads** - a list of all domains for which leads were found. You can add these to `competitorDomains` on subsequent Actor runs to exclude them.
2. **Mentions** - an overview of all sources found across Google and AI search, including whether each source mentions your brand and links back to your website.
3. **All leads** - all leads found for cold outreach. To help you organize your outreach, the list is split into three sub-lists: **Batch 1**, **Batch 2**, and **Batch 3**. Each batch includes only one lead per domain.
4. **Author list** - a list of author names to help you personalize your cold outreach.
5. **Sub-Actor results** - links to the results of the sub-Actors triggered by this Actor.

When using the **Export** button you will get the All leads dataset. You need to go to the **Storage** tab in order to download the other datasets.

### Example output

#### Domains with leads
**Output field:**
| brand_mentioned | Was your brand mentioned in the article? |
| --- | --- |
| domain | Domain of the source |
| source_url | Url of sources appearing in search results |
| companyLinkedin | Company Linkedin URL |
| companyName | Company name |
| companySize | Company size |
| companyWebsite | Company website |
| companyCity | City of the company headquarter |
| industry | Company industry |
| departments | Departments of the lead |
| email | Email of the lead |
| firstName | Lead first name |
| lastName | Lead last name |
| linkedinProfile | Lead linkedin profile |
| jobTitle | Lead job title |
| twitter | Lead Twitter profile |
| seniority | Lead seniority level |
| mobileNumber| Mobile phone of the lead |
| city| City of the lead |
| country| Country of the lead |

**Lead example:**
```json
{
  "firstName": "first name",
  "lastName": "last name",
  "linkedinProfile": "http://www.linkedin.com/in/fullName",
  "email": "firstName@example.com",
  "mobileNumber": null,
  "jobTitle": "SEO specialist",
  "industry": null,
  "city": "Prague",
  "country": "Czechia",
  "companyName": "example",
  "companyWebsite": "example.com",
  "companySize": null,
  "companyLinkedin": "http://www.linkedin.com/company/example",
  "companyCity": "Prague",
  "departments": [
    "marketing"
  ],
  "seniority": "vp",
  "twitter": null,
  "domain": "example.com",
  "source_url": [],
  "brand_mentioned": false
}
```
#### Mentions
**Output field:**
| url | URL of the source |
| --- | --- |
| domain | Domain of the source |
| queries | Search queries which return this source |
| brand_mentioned_in_source | Was your brand mentioned in the source? |
| backlink_in_source | Do you have a backlink from this source? |
| AIMode | Did the source appear in Google AI mode? |
| AIOverview_mention | Did the source appear in Google AI Overview? |
| ChatGPT_mention | Did the source appear in ChatGPT search? |
| Copilot_mention | Did the source appear in Copilot search? |
| Gemini_mention | Did the source appear in Gemini search? |
| OrganicResult_mention | Did the source appear in Google organic results? |
| Perplexity_mention | Did the source appear in Perplexity search? |

**Source example:**
```json
{
  "url": "https://www.reliablesoft.net/top-10-search-engines-in-the-world/",
  "domain": "https://www.reliablesoft.net",
  "brand_mentioned_in_source": true,
  "backlink_in_source": true,
  "Perplexity_mention": false,
  "ChatGPT_mention": true,
  "AIOverview_mention": false,
  "AIMode": false,
  "OrganicResult_mention": false,
  "queries": [
    "best search engine"
  ]
}
```

## How much does it cost to use the Link Prospecting Tool?

This Actor is billed per usage and costs approximately $0.02 per query. It also launches sub-Actors that incur additional costs:

- **[Google Search Scraper](https://apify.com/apify/google-search-scraper)** - cost per query and sources included. You can use `organicResult` to limit the number of organic results (costs $4.5 per 1k result). You can also disable some of the AI searches to lower the cost.
- **[Website Content Crawler](https://apify.com/apify/website-content-crawler)** - runs once per source.
- **[Contact Details Scraper](https://apify.com/apify/contact-details-scraper)** - runs only for sources without a backlink. Use `maxContactsPerDomain` to control costs.
- **[AI Web Scraper](https://apify.com/apify/ai-web-scraper)** - $25/1,000 sources. Only charged when the power-up is enabled.

To minimize costs during testing:
- Limit organic results to 10
- Provide only one to two search queries
- Limit the number of leads per source with `maxContactsPerDomain` set to 2.

You can also minimize cost by excluding more websites using `ignoreDomains`.

## How to use the Link Prospecting Tool?
### Input

| Field | Type | Description |
|-------|------|-------------|
| `queries` | string (textarea) | One search query per line |
| `brand` | string | Brand name to search for (case-insensitive) |
| `ownDomains` | string[] | Your domains to exclude from analysis |
| `competitorDomains` | string[] | Competitor domains to exclude |
| `ignoreDomains` | string[] | Other domains to exclude |
| `enableChatGpt` | boolean | Include ChatGPT results in the search |
| `enableAiMode` | boolean | Include AI mode results in the search |
| `enableAiOverviews` | boolean | Include AI Overviews results in the search |
| `enablePerplexity` | boolean | Include Perplexity results in the search |
| `enableGemini` | boolean | Include Gemini results in the search |
| `enableCopilot` | boolean | Include Copilot results in the search |
| `organicResult` | integer | Number of Google organic results to include per search |
| `maxContactsPerDomain` | integer | Max contact leads per source domain (default: 1) |
| `includeMention` | boolean | Run lead enrichment for sources that mention the brand but don't include a backlink |
| `department` | string[] | Target department for lead enrichment |
| `enableEmailVerification` | boolean | Verify if found emails are valid |
| `searchAuthorName` | boolean | Identify author name from the blog posts |

#### Example input

```json
{
  "brand": "Google",
  "competitorDomains": [
    "bing.com",
    "microsoft.com"
  ],
  "department": [
    "marketing",
    "c_suite"
  ],
  "enableAiMode": true,
  "enableAiOverviews": true,
  "enableChatGpt": true,
  "enablePerplexity": true,
  "enableCopilot": false,
  "enableEmailVerification": true,
  "enableGemini": false,
  "includeMention": true,
  "ownDomains": [
    "google.com",
    "developers.google.com"
  ],
  "queries": "best search engine",
  "searchAuthorName": false,
  "organicResult": 10,
  "ignoreDomains": [],
  "maxContactsPerDomain": 1
}
```





## Use cases

- **Link building** - build a list of leads for link building.
- **Outreach opportunities** - find third-party sites that cover your topic but don't mention your brand, along with contact details for outreach.
- **Generative Engine Optimization (GEO)** - track how your brand appears in AI-generated search results.
- **Brand monitoring** - monitor which queries surface your brand in ChatGPT Search.
- **Competitive analysis** - understand where competitors are mentioned and you're not.
- **Guest post search** - find relevant publications and blogs in your industry.
- **Affiliate hiring** - find affiliates and influencers already ranking for relevant topics.

## How it works

For each search query you provide, this Actor:

1. **Queries search engines** via [Google Search Scraper](https://apify.com/apify/google-search-scraper) to sources mentioned in Google organic search, ChatGPT search, Perplexity search, and Google AI mode.
2. **Checks brand mentions in the AI response** using case-insensitive text matching.
3. **Filters sources** by removing your own domains, competitor domains, and domains that aren't relevant for link building, like wikipedia.org and github.com
4. **Crawls each source page** using [Website Content Crawler](https://apify.com/apify/website-content-crawler).
5. **Checks brand mentions in each source** using case-insensitive text matching.
6. **Scrapes contacts** from sources that don't mention your brand (outreach opportunities) using [Contact Details Scraper](https://apify.com/apify/contact-details-scraper).
7. **Finds author names** - when the power-up is enabled, this Actor identifies the author name using [AI Web Scraper](https://apify.com/apify/ai-web-scraper).

### Actors used

This Actor orchestrates four Actors:

- [Google Search Scraper](https://apify.com/apify/google-search-scraper) - fetches results from Google organic search, ChatGPT, Perplexity, Gemini, Copilot, and AI Mode.
- [Website Content Crawler](https://apify.com/apify/website-content-crawler) - crawls cited source pages.
- [Contact Details Scraper](https://apify.com/apify/contact-details-scraper) - scrapes contact information from outreach opportunity domains.
- [AI Web Scraper](https://apify.com/apify/ai-web-scraper) - power-up to find author names.

## FAQ

### What is link building?

Link building is the practice of getting backlinks to your website from other websites. It's a key search engine optimization (SEO) tactic for improving your site's authority and search rankings.

### What is link building outreach?

Link building outreach is an active SEO method that involves contacting bloggers, industry influencers, and website owners to request backlinks.

### How many leads do I need for link building?

You'll typically need to contact up to five people per company to get a response. Don't reach out to all of them at once - sending multiple emails to the same domain can trigger spam filters. Work in batches with one contact per company at a time.

### How often should I run this Actor?

AI search results change quickly, so running this Actor on a weekly or monthly basis is recommended. You can exclude domains for which you already have leads to keep costs down.

### How many links should you build to a site?

The right number depends on your site's size and existing link profile. A new site may benefit from a handful of high-quality links to start, while established sites may need more frequent acquisition. Targeting websites that appear in relevant searches is a reliable way to identify high-quality backlink candidates.

### Can I use integrations with Link Prospecting Tool?
You can integrate Link Prospecting Tool with almost any cloud service or web app. The Apify platform offers integrations with **Make, Zapier, Slack, Airbyte, GitHub, Google Sheets, Google Drive**, [and plenty more](https://docs.apify.com/integrations):

[video integrations tutorial](https://www.youtube.com/watch?v=bNACk1_S_6w)

Alternatively, you could use [webhooks](https://docs.apify.com/integrations/webhooks) to carry out an action whenever an event occurs.

This should allow you to push the lead list to your cold outreach tool.

### Can I use Link Prospecting Tool with the Apify API?
The Apify API gives you programmatic access to the Apify platform. The API is organized around RESTful HTTP endpoints that enable you to manage, schedule, and run Apify Actors. The API also lets you access any datasets, monitor Actor performance, fetch results, create and update versions, and more.

To access the API using Node.js, use the `apify-client` npm package. To access the API using Python, use the `apify-client` PyPI package. Check out the [Apify API reference](https://docs.apify.com/api/v2) docs for all the details.



### Can I use Link Prospecting Tool through an MCP server?
With the Apify API, you can use almost any Actor in conjunction with an MCP server. You can connect to the MCP server using clients like ClaudeDesktop and LibreChat, or even build your own. Read all about how you can [set up Apify Actors with MCP](https://blog.apify.com/how-to-use-mcp/). 

[video MCP tutorial](https://www.youtube.com/watch?v=BKu8H91uCTg)

### Can I edit the Link Prospecting Tool?
The Link Prospecting Tool is open source. You can customize the code to build your own link prospecting tool.
