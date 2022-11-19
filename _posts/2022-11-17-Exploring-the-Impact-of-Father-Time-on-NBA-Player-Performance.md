---
layout: post
title:  "Exploring the Impact of Father Time on NBA Player Performance"
date:   2022-11-17
author: Seth Argyle
description: How does an NBA player's statistical performance change throughout their career? Read this blog to view some visuals exploring this question.
image: /assets/images/hourglass.jpg
---

NBA players have careers anywhere from one to twenty-five years long. Their performance changes from year to year due to several factors (e.g. experience, health, role on the team, opportunity, etc.). Naturally, when a player is just starting, it's expected that they will struggle because of the increased competition and new situation; additionally, it's not a surprise that player performance declines when they begin to age into the 30's. Given this ebb and flow during the career of a player, general managers understand that a successful team most often requires contributions from players who are in their "prime". This quest for a player's "prime" years begs an obvious question--when is a player's prime? Can data tell us? Are there different phases or types of prime? That's what I'm set out to explore.

Let me provide an example for how answers to these questions could prove valuable in today's NBA. Rudy Gobert, a famous player who previously played for the Utah Jazz, was recently traded to the Minesotta Timberwolves in exchange for several players. Here's a list of all the players involved in the deal (along with their current age):
- Minnesota recieves:
  - Rudy Gobert (30)
- Utah recieves:
  - Patrick Beverley (34)
  - Malik Beasley (26)
  - Jarred Vanderbilt (23)
  - Walker Kessler (21)


In the deal, Rudy Gobert is clearly the most valueable player--at the moment. However, at age 30, should Minnesota expect Gobert to perform at the same level he has in the last few years? or has he already hit his prime and is on the decline? On Utah's end, can they expect (solely based on age) that Vanderbilt and Beasley will continue to perform at their current, or even improve? for how many years can they hold this expectation? With every trade in the NBA (and in other professional sports leagues), general managers consider the past performance of the players involved and what they believe the players will contribute moving forward. If GM's make the right assumptions (based on their research) in these trade scenarios, they can make a huge positive impact on their team's short- and long-term success (e.g. [Danny Ainge Boston-Brooklyn trade](https://bleacherreport.com/articles/1883640-boston-celtics-ripped-off-brooklyn-nets-with-kevin-garnett-and-paul-pierce-deal)).

Now that I've explained the motivation for this data project, let's dive into the data! The following graphics explore the impact of a player's age on several performance measurements that include the following:
- minutes played per game (MP)
- points per game (PTS)
- total rebounds per game (TRB)
- assists per game (AST)
- blocks per game (BLK)
- steals per game (STL)
- 3pt shots attemped per game (3PA)
- effective field goal percentage (eFG%)
- 3pt percentage (3P%)
- free throw percentage (FT%)


For each chart, age is displayed along the x axis, while the performance metric lies along the y axis. The size of the dot represents player count.

![mpg by age](/assets/images/mp.png)

As a player ages, it seems like their playing time increases. However, it begins to decline at around age 32.

![ppg by age](/assets/images/PTS.png)

Points per game increases over time until about age 29, then becomes sporadic due to small sample size. (I intend on adding more data points.)

![rpg by age](/assets/images/TRB.png)

Rebounding doesn't seem to be influenced very much by age.

![apg by age](/assets/images/AST.png)

Assists per game experience a consistent increase as a player ages, until dropping at about age 33.

![bpg by age](/assets/images/BLK.png)

Blocks per game tend to slowly decline from the very beginning of a player's career (excepting a few outliers among older players--again, small sample size).

![spg by age](/assets/images/STL.png)

Steals rise and then fall, peaking between ages 23-30.

![3papg by age](/assets/images/3PA.png)
https://raw.githubusercontent.com/shargyle/stat386-projects/main/assets/images/3PA.png

Interestingly, as a player ages, he attempts more and more 3's (on average) until a dropoff at about age 33.

![efg% by age](/assets/images/eFGpct.png)

![3pt% by age](/assets/images/3pct.png)

![ft% by age](/assets/images/FTpct.png)

All shooting accuracy metrics seem to increase steadily over the course of a player's career, with no dropoff towards in the latest years.


This exploratory data analysis hints at several trends:
- Shooting accuracy improves over time, as well as assists and 3pt attempts (although these latter two have a slight dropoff).
- Younger players block at higher rates (perhaps because blocking requires more athleticism?).
- Age seems to have little impact on reboudning rate.

What else do you notice in these visuals? What questions still linger? Feel free to include your thoughts in the comment section below.


Here's a link to the [repo](https://github.com/shargyle/NBA-Analysis) containing the data and jupyter notebooks written for the data and visuals.

