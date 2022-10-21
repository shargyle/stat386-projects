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
Here's a look at one of the data tables I'm looking to scrape from the site:
![target table](/assets/images/Web-Scraping-Basketball-Reference/target_table.png)

This table represents the data for just one player, LaMarcus Aldridge. What I'd like to get is data for all 859 active players in the NBA. Because each player's data table is on an individual page, my code will have to hit all 859 pages to do the necessary scraping. So, where can I get a list of all the pages I need to hit? Take a look:
![directory](/assets/images/Web-Scraping-Basketball-Reference/directory.png)

As you can see, this directory page contains links to each player page, categorized by last name. When I click on the link for all those with a last name beginning, here is what I see:
![players list](/assets/images/Web-Scraping-Basketball-Reference/players.png)

On this page, each player's name is a link to their individual page where the target table resides. So, my scraping algorithm will contain three components:
1. Get links to all last name directories.
2. Get links to all individual pages.
3. Get target table from each individual page.

# The Code
### Packages
`pandas`:
`requests`:
`BeautifulSoup`:
### Scraping

# EDA Coming Soon
