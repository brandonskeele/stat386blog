---
layout: post
title:  "Leading Home-Run Hitters"
author: Brandon Keele
description: "Who are the MLB's leading home-run hitters? This post will look to gather data from the web to find information about them."
image: "/assets/images/baseball-bat-ball.jpeg"
---

## Introduction

I am a longtime baseball fan. I love following along with the players and teams to see how they are doing and to see which contracts pan out and which ones don't. And it's only fitting that today (as of writing this) is Opening Day for the new season.

In preparation for all of the exciting things that a new season is bound to bring, I wanted to know what the current leaderboard looks like for home-runs. Not only that, but I wanted to potentially get a few additional insights into what causes certain players to hit so many while others just never seem to be able to get the ball past the outfield wall.

Here we will discuss the procedures I used to collect this data, as well as some additional potential that exists to expand this project in the future.

## Sources of Data

In thinking about this project, we run into a problem at the very start in thinking about where to collect data from. The sport of baseball is driven by data. Every bit of data that can be collected about every game, every player, every pitch, every throw, etc. is being stored by somebody in some data warehouse. We will never run out of baseball data to look at. So the problem is not as much about finding a place where the data exists, but more about finding a place where the data can be taken ethically. Partly out of consideration for the question of ethics and partly out of a desire to practice some web scraping techniques, I chose a source that was definitely not the easiest or cleanest in terms of data collection.

The data that I used can be found on the website called [Baseball Almanac](https://baseball-almanac.com).

![Figure]({{site.url}}/{{site.baseurl}}/assets/images/baseball-almanac.png)

This website contains a plethora of information about all things baseball. A quick check at the robots.txt page on the website let me know that there are no restrictions on scraping data from the website so I decided to move forward with this as the source of my data.

Luckily for me, a page has already been compiled on the home-runs leaderboard. Unlickily for me, this page only includes the name of the player and the total number of home runs they have hit. So I would need to find an additional source for the rest of the desired information about these players.

## Collecting the Data

# Beginnings and Problem 1

Because part of the motivation for collecting data in this way in the first place was to practice web scraping, I decided to use the <b>selenium</b> package in Python instead of the <b>requests</b> package or some other tool. As I'll bring up later, this caused me to learn a few more advanced techniques with <b>selenium</b> than I knew at the start of the project, but I consider that a plus and not a minus.

The initial setup was straightforward; I was able to find a consistent way to find all of the player's names and home-run totals on the page and loop through to collect the text data for each. But then I needed to find a place to collect more specific information about all of the same players and then combine it all into a nicely packaged dataset. Again, luckily for me, the Baseball Almanac made things relatively straightforward.

# Solution and Problem 2

Each player's name on the leaderboard was actually an internal link to a biography/stats page for that player. As I collected the name of each player, I could also collect the link attribute for that player. I've included some code below that was helpful. I knew how to grab the text from an element, but I wasn't sure how to grab an attribute of the element. It turned out to be pretty intuitive:

```
player_links.append(player.get_attribute("href"))
```

* "player_links" is the list of links to the player's biographical page
* "player" is the data type housing the HTML element which contains the player's name in the leaderboard table
* "get_attribute" is the method which extracts the value passed into the specified HTML attribute
* "href" is, of course, the attribute that we wanted to extract because it contains the hyperlink to the player's biographical page

Once all of these links had been collected, I was then able to start navigating to the different links and collecting more specific information about all of these home-run hitters. I, of course, could have simply called "get" method off of the webdriver that was initialized at the beginning of the script, navigating to each page in turn, but I wanted to keep the main leaderboard open in the first tab while opening and closing the other pages in new tabs.

# Another Solution and Problem 3