# HalalCheck API 🛡️

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Python](https://img.shields.io/badge/python-3.11+-green.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=flat&logo=fastapi)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=flat&logo=redis&logoColor=white)
![Postgres](https://img.shields.io/badge/postgres-%23316192.svg?style=flat&logo=postgresql&logoColor=white)

HalalCheck API is an enterprise-grade REST architecture built to seamlessly integrate with BPJPH's (Badan Penyelenggara Jaminan Produk Halal) official halal certification data. Built for scale, it handles thousands of queries seamlessly through advanced caching, background queue processing, and machine learning components.

**[🚀 Live API on RapidAPI Hub (Click Here)](https://rapidapi.com)** 
*(Placeholder link: please replace with your real RapidAPI Hub URL)*

---

## 🏗️ System Architecture

This API is designed using **Microservices-style** background processing and layered caching to ensure sub-second response times even when the upstream sources are slow or unavailable.

```mermaid
graph TD
    Client((Client/App)) --> |REST API| Nginx(Nginx Proxy)
    Nginx --> API[FastAPI Server]
    
    API --> Cache[(Redis Cache)]
    API --> DB[(PostgreSQL)]
    
    API --> |Queue Task| Celery(Celery Workers)
    
    Celery <--> |Retrieve Auth/Tokens| BPJPH[BPJPH Upstream]
    Celery --> |Scraping Core| Scraper(Next.js/Rust Scraper)
    Celery --> |Ingredient Check| LLM(AI Ingredient Analyzer)
    
    Scraper --> |Results| DB
    LLM --> |Results| DB
```

---

## ✨ Key Features & Technical Highlights

- **High-Performance Scraping Engine:** Core logic powered by a combination of headless automation and Rust, bypassing heavy anti-bot algorithms effectively.
- **Resilient Caching Strategy:** Multi-level caching via Redis ensures repeat queries are served in `~50ms`.
- **Background Worker Processing:** Utilizing Celery + Redis broker to handle long-running ingredient extraction requests without blocking the main event loop.
- **AI-Powered Ingredient Verification:** Analyzes and predicts the halal integrity of non-certified or ambiguous ingredients using an integrated Large Language Model (Anthropic/OpenAI) with high accuracy.
- **Fully Dockerized:** Shipped with `docker-compose` wrapping the API, PostgreSQL, Redis, Celery beat, and Nginx.

---

## 💻 Integration Examples

To maintain the security of our internal web-scraping logic and ML pipelines, this repository serves as **documentation and an SDK sandbox**. 

Here are snippets to easily consume the HalalCheck API in your own application:

### Python (Requests)
```python
import requests

url = "https://halalcheck-api.p.rapidapi.com/v1/certificates/search"

querystring = {"query": "Indomie Rasa Kaldu Ayam", "limit": "5"}

headers = {
	"X-RapidAPI-Key": "YOUR_RAPIDAPI_KEY",
	"X-RapidAPI-Host": "halalcheck-api.p.rapidapi.com"
}

response = requests.get(url, headers=headers, params=querystring)
print(response.json())
```

### Node.js (Axios)
```javascript
const axios = require('axios');

const options = {
  method: 'GET',
  url: 'https://halalcheck-api.p.rapidapi.com/v1/certificates/search',
  params: {query: 'Indomie Rasa Kaldu Ayam', limit: '5'},
  headers: {
    'X-RapidAPI-Key': 'YOUR_RAPIDAPI_KEY',
    'X-RapidAPI-Host': 'halalcheck-api.p.rapidapi.com'
  }
};

try {
	const response = await axios.request(options);
	console.log(response.data);
} catch (error) {
	console.error(error);
}
```

---

### 📫 Contact & Custom Enterprise Solutions

For businesses that require custom service-level agreements (SLA), offline database dumps, or specialized AI training datasets:
Please reach out via the Contact Developer section on our [RapidAPI portal]().

---
*© 2026 Developed with ❤️. All source code relating to the scrapper logic is proprietary and closed-source.*
