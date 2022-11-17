---
title: "Scraping with soup"
summary: Some boiler code to get started with beatifulsoup
description: Just practicing scraping
date: 2022-11-17
tags: ["scripts", "web-scraping","crawling"]
author: "Aravind S"
url: "/scrapbook/beautifulsoup"
---

### Requirements installation
```bash
$ pip install beautifulsoup4
$ pip install lxml
$ pip install requests
```

### Code
```python
import sys
import requests
from bs4 import BeautifulSoup

print("Select any option:-")
print("1. Search for a team")
print("2. Go to a page")

choice = int(input("Choice: "))

if choice == 1:
    print()
    team_name = input("Search for: ")
    params = {"q": team_name}
elif choice == 2:
    print()
    page_num = int(input("Page num (Value between 1-24): "))
    params = {"page_num": page_num}
else:
    print()
    print("Invalid choice! Please try again")
    sys.exit()

URL = "https://www.scrapethissite.com/pages/forms"
r = requests.get(URL, params=params)

soup = BeautifulSoup(r.text, "lxml")
hockey_section = soup.find("section", id="hockey")
div = hockey_section.find("div", class_="container")
table = div.find("table", class_="table")
tbody = table.tbody
teams = table.find_all("tr", class_="team")

if teams == []:
    print("No results found for given params! Try again")
else:
    for team in teams:
        team_data = team.find_all("td")
        name = team_data[0].text.strip()
        year = team_data[1].text.strip()
        wins = team_data[2].text.strip()
        losses = team_data[3].text.strip()
        ot_losses = team_data[4].text.strip()
        win_percent = team_data[5].text.strip()
        gf = team_data[6].text.strip()
        ga = team_data[7].text.strip()
        net_g = team_data[8].text.strip()
        print()
        print(f'''Team Name: {name}
Year: {year}
Wins: {wins}
Losses: {losses}
OT Losses: {ot_losses}
Win %: {win_percent}
Goals For: {gf}
Goals Against: {ga}
+/-: {net_g}''')
```

***Note***: *The above example uses [scrapethissite.com](https://www.scrapethissite.com), a great place to get started if you're new to webscraping*