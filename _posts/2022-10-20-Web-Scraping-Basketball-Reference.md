---
layout: post
title:  "Web Scraping Basketball Reference"
date:   2022-10-20
author: Seth Argyle
description: In this post, you'll how to scrape player data from basketball-reference.com using Python.
image: /assets/images/basketball.jpg
---

I first became interested in statistics and data science because of my love for basketball. In the NBA, data is ubiquitous, and it's becoming more influential in managers' and coaches' decision making. Each basketball game has a resulting "box score", a table displaying descriptive numbers about the game at a team and player level (e.g. points scored, field goals made, rebounds, time played). Over the course of a season, a player's performance can be measured and tracked through the aggregation of these box scores. [Basketball Reference](https://www.basketball-reference.com/players/) stores this insightful data for each player that has ever played in the NBA.

I'll explain how to scrape this data from the web by doing the following:
- discussing unique obstacles presented by the website strucutre and how to overcome these issues.
- dispalying code that scrapes the data and organizes it into a `pandas` dataframe.

# Web Data
### Target Table
Here's a look at one of the data tables I'm looking to scrape from the [site](https://www.basketball-reference.com/players/a/aldrila01.html):

<img src="/assets/images/Web-Scraping-Basketball-Reference/target_table.png" alt="target table" width="550" height="350">

This table represents the data over all available seasons for just one player, LaMarcus Aldridge. What I'd like to get is data for all 859 active players in the NBA. Because each player's data table is on an individual page, my code will have to hit all 859 pages to do the necessary scraping. So, where can I get a [list](https://www.basketball-reference.com/players/) of all the pages I need to hit? Take a look:

<img src="/assets/images/Web-Scraping-Basketball-Reference/directory.png" alt="directory" width="550" height="350">

As you can see, this directory page contains links to each player page, categorized by last name. When I click on the [link](https://www.basketball-reference.com/players/a/) for all those with a last name beginning with A, here is what I see:

<img src="/assets/images/Web-Scraping-Basketball-Reference/players.png" alt="players list" width="550" height="350">

On this page, each player's name is a link to their individual page where the target table resides. So, my scraping algorithm will contain three components:
1. Get links to all last name directories.
2. Get links to all individual pages.
3. Get target table from each individual page.

# The Code
### Packages
`pandas`: to build pandas dataframe with player data
`requests`: to import website html code/info
`BeautifulSoup`: to navigate html code to desired data
```
import pandas as pd
import requests
from bs4 import BeautifulSoup
```
### Scraping
##### Get links to all last name directories:
```
# get all last name directories
players_url = 'https://www.basketball-reference.com/players/'
r = requests.get(players_url)
bs = BeautifulSoup(r.text)

letter_links = bs.find('ul', {'class': 'page_index'}).find_all('a')
letter_links = ['https://www.basketball-reference.com' + link.get('href') for link in letter_links if len(link.text) == 1]
```
##### Get links to all individual pages:
```
# get all player links from last name directories
player_links = []

for letter_link in letter_links:
    r = requests.get(letter_link)
    bs = BeautifulSoup(r.text)
    player_tags = bs.find('tbody').find_all('th', {'scope': 'row', 'class': 'left'})
    player_links += ['https://www.basketball-reference.com' + player_tag.find('a').get('href') for player_tag in player_tags if player_tag.find('strong')]
```
##### Get target table from each individual page, make dataframe:
```
# build dataframe of all players' stats by season

appended_data = []

for player_link in tqdm(player_links):
    dfs = pd.read_html(player_link)
    df = dfs[1]

    r = requests.get(player_link)
    bs = BeautifulSoup(r.text)
    name = bs.find("div", {"id": "info", "class": "players"}).find("h1").text
    name = name.split('\n')[1]

    df['Name'] = name
    df['Exp'] = df.index + 1

    appended_data.append(df)

nba = pd.concat(appended_data)
```
Here's what the head of your dataframe will look if run correctly:
<img src="/assets/images/Web-Scraping-Basketball-Reference/df.png" alt="dataframe head" width="550" height="125">

# EDA Coming Soon
