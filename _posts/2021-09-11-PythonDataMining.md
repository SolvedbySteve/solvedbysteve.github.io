---
title: "Python Data Mining"
date: 2021-09-11
tags: [python, pandas, data mining, web scraping, data engineering, ETL, automation]
header:
  image: "/images/rough.png"
excerpt: "Python, Pandas, Data Mining, Web Scraping, Data Engineering, ETL, Automation"
mathjax: "true"
---

# Diamond in The Rough

Hello there! Once again I'm working in Python to show you how powerful the language is. In this project I will cover how to use a library named Beautiful Soup to scrape data from virtually any site. I will be interacting with a clothing company that I really enjoy shopping with; That company is Diamond Supply Co and I will be looking at their Carolina Blue Nautica Anorak. 

### What you'll see in this project

This project will cover:

- How data can be extracted from a website using a Python library entitled Beautiful Soup.
- Loading the data into Python via Beautiful Soup's web scraping capabilities.
- 

## Import Dependencies

I'll start by importing the libraries needed to complete the project. BeautifulSoup will do the heavy lifting as we will use its datascraping feautures along with the Requests library to load and parse the HTML from the website. The Time library will be used to automate how often our dataframe is updated; DateTime will be used to create timestamps that will be used in the data frame that will be created. The SMTB library will be used to send an email when a certain price threshold is reached. Pandas will be used with the CSV library to create and update a dataframe.

The code for imports should look like this:

```python
from bs4 import BeautifulSoup
import requests
import time
import datetime
import smtplib
import pandas as pd
import csv

```

*I am using Jupyter Lab as an IDE but this will work in other coding envvironments such as VS Code or Brackets.*

## Connecting To The Site
Here I am explaining the variables required to acquire information from the Diamond Supply Co site.

### URL variable
Now I'll connect to the site. The first thing that I'll need is the URL that is associated with the product.

### Headers variable
Next I'll gather the information needed for the "headers" variable. This allows me to access the page, otherwise, I will be directed to a login page because of the JavaScript. You can get the **User-Agent** information needed for the header here [click here](http://httpbin.org/get).

### Page variable
This pulls the actual page using the request library and the **URL** and **headers** variables I have declared earlier.

### Soup variables
The **soup1** variable parses the HTML content of the page and transform it into a readable format; the **soup2** variable uses the "prettify" function to further format the information.

### Creating the title and price variables
I am only interested in the title and price of the item that I have selected to watch. Beautiful Soup uses the HTML id attribute to "fetch" the text for these two page elements. The easiest way to find this is to right click on the desired element and click inspect. A window should open on the screen and as you hover over the elements of the page it should highlight the HTML code associated with that specific element. As you see in the picture below, the price is being higlighted on the screen and the window to the right is focused on the HTML element associated with the item price; Notice that I have highlighted the id which will be used in the price variable below.


![placeholder for price id image](image.jpg)

This process was duplicated for the title variable. Once this is completed, the code block should look similar to this:

```python
URL = 'https://www.diamondsupplyco.com/collections/diamond-x-nautica/products/diamond-supply-co-nautica-polar-fleece-anorak-jacket-diamond-blue'

headers = { "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

page = requests.get(URL, headers=headers)

soup1 = BeautifulSoup(page.content, "html.parser")
soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

title = soup2.find(id='Sku-6544074113096').get_text()

price = soup2.find(id='ProductPrice-6544074113096').get_text()

print(title)
print(price)
```

## Cleaning The Data

If you run the above mentioned code, you will notice that there is a lot of unwanted white space between the item's title and price. To solve this, I used the strip function on the **price** and **title** variables. There's usually more to be done regarding data-cleansing, but since this is a small dataset, this is all that we need to do here.

```python

price = price.strip()[1:]
title = title.strip()

print(title)
print(price)

```

## Timestamp
I am creating a timestamp using the **Datetime** library. This will be used to track when data was collected as my intentions are to track this item over multiple days and track pricing fluctuations.

```python
today = datetime.date.today()
print(today)
```

## Creating CSV

Here I am creating a new csv to track the pricing changes. Later I will write the new data to this csv file.
```python
header = ['Title', 'Price', 'Date']
data = [title, price, today]


with open('DiamondCoDataset.csv', 'w', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(header)
    writer.writerow(data)
```

### Transforming csv into data frame
Using the **Pandas** library, I am transforming the csv file into a data frame which allows me to easily manipulate the data.
```python
df = pd.read_csv(r'C:\Users\adeto\Desktop\dmnd-data-mining\DiamondCoDataset.csv')

df.head()
```
### Appending data to the csv
This will allow me to add information to the csv as it is gathered from the site.
```python
with open('DiamondCoDataset.csv', 'a+', newline='', encoding='UTF8') as f:
    writer = csv.writer(f)
    writer.writerow(data)
```
*You'll notice that this block is commented out in my actual code. If you plan to utilize this code for long term tracking, do not run this block as it will delete previous infomation :)* 

## Combining Code
In an effort to produce code that is easy to follow, I have combined all of the previous code into a single block.

```python
def check_price():
    
    URL = 'https://www.diamondsupplyco.com/collections/diamond-x-nautica/products/diamond-supply-co-nautica-polar-fleece-anorak-jacket-diamond-blue'
    
    headers = { "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.63 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

    page = requests.get(URL, headers=headers)

    soup1 = BeautifulSoup(page.content, "html.parser")
    
    soup2 = BeautifulSoup(soup1.prettify(), "html.parser")

    title = soup2.find(id='Sku-6544074113096').get_text()

    price = soup2.find(id='ProductPrice-6544074113096').get_text()

    price = price.strip()[1:]
    title = title.strip()


    today = datetime.date.today()
    
   

    header = ['Title', 'Price', 'Date']
    data = [title, price, today]

    with open('DiamondCoDataset.csv', 'a+', newline='', encoding='UTF8') as f:
        writer = csv.writer(f)
        writer.writerow(data)
```

## Automating Data Collection
This function uses a while loop in conjunction with the time function to gather data from the site and write it to the CSV. I have this set to fetch data once a day but this can be configured to your needs. This is what makes this different than just copying the data into Excel.

## Additional Functionality
Here I am adding a function that will send me an email when the price of the item goes below a certain point. I am using the **SMTP** library to make this possible. The only thing missing is my password, which will not be provided (lol).


 
## Conclusion
 This was another fun project and it's one that can be built upon by making the requests more complex and changing how often the data is collected. Stay tuned for more Data Science projects!
 

    
 

