# 🔥 Firecrawl

Crawl and convert any website into LLM-ready markdown. Build by [Mendable.ai](https://mendable.ai?ref=gfirecrawl)

_This repository is currently in its early stages of development. We are in the process of merging custom modules into this mono repository. The primary objective is to enhance the accuracy of LLM responses by utilizing clean data. It is not ready for full self-host yet - we're working on it_

## What is Firecrawl?

[Firecrawl](https://firecrawl.dev?ref=github) is an API service that takes a URL, crawls it, and converts it into clean markdown. We crawl all accessible subpages and give you clean markdown for each. No sitemap required.

_Pst. hey, you, join our stargazers :)_

<img src="https://github.com/mendableai/firecrawl/assets/44934913/53c4483a-0f0e-40c6-bd84-153a07f94d29" width="200">

## How to use it?

We provide an easy to use API with our hosted version. You can find the playground and documentation [here](https://firecrawl.dev/playground). You can also self host the backend if you'd like.

- [x] [API](https://firecrawl.dev/playground)
- [x] [Python SDK](https://github.com/mendableai/firecrawl/tree/main/apps/python-sdk)
- [x] [Node SDK](https://github.com/mendableai/firecrawl/tree/main/apps/js-sdk)
- [x] [Langchain Integration 🦜🔗](https://python.langchain.com/docs/integrations/document_loaders/firecrawl/)
- [x] [Llama Index Integration 🦙](https://docs.llamaindex.ai/en/latest/examples/data_connectors/WebPageDemo/#using-firecrawl-reader)
- [X] [Langchain JS Integration 🦜🔗](https://js.langchain.com/docs/integrations/document_loaders/web_loaders/firecrawl)
- [ ] Want an SDK or Integration? Let us know by opening an issue.

To run locally, refer to guide [here](https://github.com/mendableai/firecrawl/blob/main/CONTRIBUTING.md).

### API Key

To use the API, you need to sign up on [Firecrawl](https://firecrawl.dev) and get an API key.

### Crawling

Used to crawl a URL and all accessible subpages. This submits a crawl job and returns a job ID to check the status of the crawl.

```bash
curl -X POST https://api.firecrawl.dev/v0/crawl \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "url": "https://mendable.ai"
    }'
```

Returns a jobId

```json
{ "jobId": "1234-5678-9101" }
```

### Check Crawl Job

Used to check the status of a crawl job and get its result.

```bash
curl -X GET https://api.firecrawl.dev/v0/crawl/status/1234-5678-9101 \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer YOUR_API_KEY'
```

```json
{
  "status": "completed",
  "current": 22,
  "total": 22,
  "data": [
    {
      "content": "Raw Content ",
      "markdown": "# Markdown Content",
      "provider": "web-scraper",
      "metadata": {
        "title": "Mendable | AI for CX and Sales",
        "description": "AI for CX and Sales",
        "language": null,
        "sourceURL": "https://www.mendable.ai/"
      }
    }
  ]
}
```

### Scraping

Used to scrape a URL and get its content.

```bash
curl -X POST https://api.firecrawl.dev/v0/scrape \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "url": "https://mendable.ai"
    }'
```

Response:

```json
{
  "success": true,
  "data": {
    "content": "Raw Content ",
    "markdown": "# Markdown Content",
    "provider": "web-scraper",
    "metadata": {
      "title": "Mendable | AI for CX and Sales",
      "description": "AI for CX and Sales",
      "language": null,
      "sourceURL": "https://www.mendable.ai/"
    }
  }
}
```

### Search (Beta)

Used to search the web, get the most relevant results, scrap each page and return the markdown.

```bash
curl -X POST https://api.firecrawl.dev/v0/search \
    -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer YOUR_API_KEY' \
    -d '{
      "query": "firecrawl",
      "pageOptions": {
        "fetchPageContent": true // false for a fast serp api
      }
    }'
```

```json
{
  "success": true,
  "data": [
    {
      "url": "https://mendable.ai",
      "markdown": "# Markdown Content",
      "provider": "web-scraper",
      "metadata": {
        "title": "Mendable | AI for CX and Sales",
        "description": "AI for CX and Sales",
        "language": null,
        "sourceURL": "https://www.mendable.ai/"
      }
    }
  ]
}
```

Coming soon to the SDKs and Integrations.

## Using Python SDK

### Installing Python SDK

```bash
pip install firecrawl-py
```

### Crawl a website

```python
from firecrawl import FirecrawlApp

app = FirecrawlApp(api_key="YOUR_API_KEY")

crawl_result = app.crawl_url('mendable.ai', {'crawlerOptions': {'excludes': ['blog/*']}})

# Get the markdown
for result in crawl_result:
    print(result['markdown'])
```

### Scraping a URL

To scrape a single URL, use the `scrape_url` method. It takes the URL as a parameter and returns the scraped data as a dictionary.

```python
url = 'https://example.com'
scraped_data = app.scrape_url(url)
```

### Search for a query

Performs a web search, retrieve the top results, extract data from each page, and returns their markdown.

```python
query = 'what is mendable?'
search_result = app.search(query)
```

## Contributing

We love contributions! Please read our [contributing guide](CONTRIBUTING.md) before submitting a pull request.
