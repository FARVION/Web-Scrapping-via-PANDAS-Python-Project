# 🕷️ Web Scraping with BeautifulSoup & Pandas

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.0%2B-150458?style=for-the-badge&logo=pandas&logoColor=white)
![BeautifulSoup](https://img.shields.io/badge/BeautifulSoup4-4.12%2B-green?style=for-the-badge)
![Requests](https://img.shields.io/badge/Requests-2.31%2B-orange?style=for-the-badge)

A Python project that scrapes structured data from Wikipedia using **BeautifulSoup** and **Requests**, then cleans, stores, and exports it using **Pandas** — turning raw HTML into a ready-to-use CSV dataset.

---

## 📌 Project Overview

This project scrapes the **[List of Largest Companies in the United States by Revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue)** from Wikipedia. It parses the HTML table, extracts company data row by row, loads it into a Pandas DataFrame, and exports the result to a `.csv` file for further analysis.

---

## 📊 Data Extracted

| Column | Description |
|---|---|
| Rank | Company's revenue rank |
| Name | Company name |
| Industry | Business sector |
| Revenue (USD millions) | Annual revenue |
| Revenue growth | Year-over-year growth |
| Employees | Number of employees |
| Headquarters | Company location |

---

## 🛠️ Tech Stack

- **Python 3.8+**
- **BeautifulSoup4** — HTML parsing
- **Requests** — HTTP requests with custom headers
- **Pandas** — Data manipulation and CSV export

---

## 📁 Project Structure
```
web-scraping-pandas/
│
├── web_scraper.py          # Main scraping script
├── Web_Scraped_Data.csv    # Output CSV (generated on run)
├── requirements.txt        # Project dependencies
└── README.md               # Project documentation
```

---

## ⚙️ Installation

**1. Clone the repository**
```bash
git clone https://github.com/your-username/web-scraping-pandas.git
cd web-scraping-pandas
```

**2. Create a virtual environment (recommended)**
```bash
python -m venv venv
source venv/bin/activate       # macOS/Linux
venv\Scripts\activate          # Windows
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

---

## 📦 Requirements
```text
beautifulsoup4==4.12.3
requests==2.31.0
pandas==2.1.4
lxml==4.9.4
```

---

## 🚀 Usage
```bash
python web_scraper.py
```

The script will:
1. Send an HTTP GET request to the Wikipedia page
2. Parse the HTML using BeautifulSoup
3. Extract table headers and row data
4. Load the data into a Pandas DataFrame
5. Export the DataFrame to `Web_Scraped_Data.csv`

---

## 🧠 Core Code
```python
from bs4 import BeautifulSoup
import requests
import pandas as pd

url = 'https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue'

headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 "
                  "(KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"
}

page = requests.get(url, headers=headers)
soup = BeautifulSoup(page.text, 'html.parser')

table = soup.find_all('table')[0]
world_titles = table.find_all('th')
world_table_titles = [title.text.strip() for title in world_titles]

df = pd.DataFrame(columns=world_table_titles)

column_data = table.find_all('tr')
for row in column_data[1:]:
    row_data = row.find_all('td')
    individual_row_data = [data.text.strip() for data in row_data]
    if len(individual_row_data) == len(world_table_titles):
        df.loc[len(df)] = individual_row_data

df.to_csv('Web_Scraped_Data.csv', index=False)
print("✅ Data scraped and saved to Web_Scraped_Data.csv")
```

---

## 📸 Sample Output
```
   Rank                Name              Industry  Revenue (USD millions)
0     1             Walmart              Retail                   611289
1     2              Amazon  Retail/Technology                   513983
2     3          Apple Inc.          Technology                   394328
3     4  UnitedHealth Group          Healthcare                   324162
4     5  Berkshire Hathaway          Financials                   302089
```

---

## 🔧 Potential Improvements

- [ ] Add error handling for network failures and HTML structure changes
- [ ] Schedule automated runs with `cron` or `APScheduler`
- [ ] Store data in a SQLite or PostgreSQL database
- [ ] Visualize results using Matplotlib or Seaborn
- [ ] Build a Streamlit dashboard for interactive exploration

---

