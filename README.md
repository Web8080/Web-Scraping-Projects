# Web-Scraping-Projects

## Table of Contents
- [Project Overview](#project-overview)
- [task 1: Web Scraping Books to Scrape](#optional-project-web-scraping-books-to-scrape)
- [task 2: Web Scraping and Data Extraction](#project-1-web-scraping-and-data-extraction)
- [Requirements](#requirements)
- [Usage](#usage)

## Project Overview
This repository contains several projects demonstrating web scraping techniques using Python. The main projects include scraping data from a simple HTML table and text mining, while the optional project illustrates how to scrape data from a paginated website.

## Task 1: Web Scraping Books to Scrape
### Description
This project demonstrates how to scrape book titles and prices from the [Books to Scrape](http://books.toscrape.com/) website. The code handles pagination by checking for the presence of a "Next" button, allowing the script to scrape multiple pages of data efficiently.

### Features
- Scrapes book titles and prices.
- Handles pagination using a "Next" button.
- Stores scraped data in a Pandas DataFrame.
- Saves the collected data to a CSV file for easy access and analysis.

### Code
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Function to scrape data from a page
def scrape_books(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    
    books = []
    
    for article in soup.find_all('article', class_='product_pod'):
        title = article.h3.a['title']
        price = article.find('p', class_='price_color').get_text(strip=True)
        books.append({'Title': title, 'Price': price})

    return books

# Main logic to iterate over pages
base_url = 'http://books.toscrape.com/catalogue/page-{}.html'
all_books = []
page_number = 1

while True:
    url = base_url.format(page_number)
    books = scrape_books(url)
    all_books.extend(books)

    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    next_button = soup.find('li', class_='next')

    if next_button:
        page_number += 1
    else:
        break

df = pd.DataFrame(all_books)
df.to_csv('books_data.csv', index=False)
print("Data has been saved to 'books_data.csv'")
```

## Task 2 and 3: Web Scraping and Data Extraction
### Description
In this project, data is scraped from a specified URL containing a table of information. The script uses BeautifulSoup to extract relevant data points from the HTML structure and stores the data in a Pandas DataFrame for further analysis.

### Features
- Extracts specific data from an HTML table.
- Utilizes BeautifulSoup for parsing HTML.
- Saves the extracted data in a structured format using Pandas.


## Requirements
- Python 3.x
- Requests
- BeautifulSoup4
- Pandas

## Usage
- For the **Books to Scrape** project, run the script to scrape book titles and prices and save them to a CSV file.


