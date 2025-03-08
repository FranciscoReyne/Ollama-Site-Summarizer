:)

# Ollama Site Summarizer

## Overview
This project provides a simple Python script to summarize articles from any blog or news site using the local `ollama` LLM. The script extracts text from the given URL, processes it, and generates a concise summary.

## Features
- Extracts and summarizes text from any given URL
- Allows the user to specify the `ollama` model to use
- Uses `ollama` for local inference, ensuring privacy and efficiency
- Requires minimal dependencies

## Installation
### Prerequisites
- Python 3.8+
- `ollama` installed locally
- `beautifulsoup4` and `requests` for web scraping

Install dependencies:
```bash
pip install beautifulsoup4 requests
```

## Usage
Run the script with a URL and specify the `ollama` model (e.g., `mistral` or any other available model):
```bash
python summarize.py "https://example.com/news-article" "mistral"
```

### Example Output
```
Summary:
This article discusses the latest trends in artificial intelligence and how it is shaping the future of technology.
```

## Code
```python
import requests
from bs4 import BeautifulSoup
import ollama
import sys

def extract_text(url):
    response = requests.get(url)
    if response.status_code != 200:
        raise Exception("Failed to fetch the webpage")
    
    soup = BeautifulSoup(response.text, "html.parser")
    paragraphs = soup.find_all("p")
    text = "\n".join(p.get_text() for p in paragraphs)
    return text

def summarize_text(text, model):
    response = ollama.chat(model, messages=[{"role": "user", "content": f"Summarize this article: {text}"}])
    return response["message"]["content"]

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: python summarize.py <URL> <MODEL>")
        sys.exit(1)
    
    url = sys.argv[1]
    model = sys.argv[2]
    try:
        article_text = extract_text(url)
        summary = summarize_text(article_text, model)
        print("\nSummary:\n", summary)
    except Exception as e:
        print("Error:", e)
```

## License
This project is licensed under the MIT License.


-----

Good luck !!
