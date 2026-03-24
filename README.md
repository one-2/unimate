# UniMate Backend (MVP)

I co-founded and solo-engineered UniMate in my second year of computer science. It aggregated and processed student events data from a variety of sources, providing students one place to see information about social activities at Sydney universities. It used a distributed proxy service for web scraping and a GPT text classifier to process events data into categories, which we served to users. The system served over 1,500 users, validating our product, before the university released a competing product. Unfortunately they didn't communicate their work on this to us when we contacted them. Nonetheless, this was a very education project. It gave me footing in quick engineering under competing business pressures, reinforced by the pressure of live users.

This repo is a sanitised version of the final commit of UniMate's MVP. Development finished when we wound up the business.

[You may also wish to read the retrospective](https://stelliott.online/2025/03/31/unimate-retrospective-of-a-first-time-engineering-founder/) for more information on the engineering process and its lessons.

---

## What It Does

- Scrapes data on club events from a variety of sources using Infatica proxies.
- Parsed and regularised HTML data.
- Classified text into bins for user filtering using the OpenAI API.
- Dumps processed data to CSV and binary for partial-restarts.
- Terminal frontend for ease-of-use.

---

## Pipeline Architecture

scraper/ \
├── Scraper.py → Entry point for scraping club and event pages \
├── Scripts.py → Extracts data from dynamically loaded web content \
├── InfaticaRequests.py → Calls the proxies

processor/ \
├── Tagger.py → Classify event descriptions using OpenAI API

utils/ \
├── Parser.py → Methods for parsing HTML \
├── Utils.py → Date handling and URL validation 

io/ \
├── Filesystem.py → Centralised handling of file structure \
├── Input.py → Reads from saved files and dumps \
├── Output.py → Writes CSV and binaries to disk 

config/ \
├── Config.py → Prompts and constants \
├── Env.py

core/ \
├── Interface.py → Logic for the command-line interface \
├── Debug.py → Test methods


---

## Setup

Note that this was an MVP and it was designed for speed rather than safety or reliability. Proceed at your own risk.

1. Clone the repo
2. Set your `infatica_api_key` in `Config.py`  
3. Run `Interface.py` to launc the CLI

Dependencies:
- Python 3.9+
- OpenAI API key (for `Tagger.py`)
- A valid Infatica account if testing live scraping

---

## Design Patterns

- **Pipeline** - Modules include Input, Scraper, Processor, and Output, chained in sequence
- **Strategy** - GPT binary classifiers called using a common interface
- **Factory** - Centralised filesystem manager is a path-building factory
- **Controller** - CLI decides pipeline construction
- **Observer-like Logging**

---

## 📉 Tradeoffs

| **Decision**                | **Rationale**                             | **Cost**                                       |
|-----------------------------|-------------------------------------------|------------------------------------------------|
| No Selenium                 | It didn't work well on our target sites   | More fragile                                   |
| No database                 | Fast iteration, simple output             | Harder to scale and query                      |
| OpenAI tagging via GPT      | Fast to implement and worked              | Could get expensive                            |
| Single-threaded scraping    | Easier to implement                       | Impracticably slow                             |
| No automatic tests          | Faster new code                           | Slower refactoring of old code                 |

---

## 🔄 Future Improvements

- Better logging and more exception handling
- Async scraping  
- Unit and integration tests  
- Replace CSV-based workflows with a database
- Use a minimal backend like FastAPI to handle automatic upload to the frontend 

---

## 🧑‍💻 Author

Stephen Elliott  
[Sydney, 2023]
