---
layout: post
title:  "EDA Don't Lie: Exploring the data from the Ball Don't Lie API"
author: Daphne LeSueur
description: This post describes the EDA done based on data from the Ball Don't Lie API. 
img: kobe.jpg
--- 
If you thought pulling the data from the API was fun, you're in for a treat with this EDA walkthrough. We already got the Data part of Exploratory Data Analysis, so at this point we're already a third of the way done. Now we just need to do the actual Exploring and make some Analyses! 

The best way to start EDA is to have some questions in mind that you're wanting to get answers to. Hopefully you asked one or more of these questions before even fetching the data. I'll be honest, before getting the dataset my questions were pretty generic and were all about the correlation between points and many other metrics. We're going to explore those questions here but like any good EDA, I was able to come up with some much more (subjectively) interesting questions throughout the process. 

These points-vs.-other-metrics questions were the driving force that led me to the [Ball Don't Lie API](https://www.balldontlie.io/home.html#introduction). For more details on how and why I chose this specific API, refer to my [post](https://dlesueur.github.io/Ball-Don't-Lie/) all about my data collection process. 

I won't go through every single step of my EDA here, but I'll try to hit the biggest and most interesting points. As always though, you can view my [entire EDA file](https://github.com/dlesueur/386Project/blob/main/EDA.ipynb) in my github repository. 

Another important file to have access to is this [modifyDatasets](https://github.com/dlesueur/386Project/blob/main/modifyDatasets.ipynb) where I executed all of my merges, pivots, melts, etc. It made more sense to me to take care of all of this in a separate file, save my changes to the CSVs I was using as my data, and re load those into my EDA file. Obviously that's just my preference, and if you want to keep an eye on all of that in the same file I'm not going to stop you!

That's more than enough introduction for anyone to understand what's going on in this post, so let's just get into the Exploring and Analyzing, the whole reason we came here in the first place.

# Points Exploration

## What is the distribution of points?

Like I mentioned, my initial questions were all based on points and their correlation with other metrics. But as soon as I got the data I was curious about the distribution of points.

![point distribution](/assets/img/Viz/homeVsVisitorPointDistribution.png)

This histogram shows the difference between home team and visitor team score. Both look pretty relatively normal, with home team score having a little bit higher of a mean. Which is exactly what I would have expected! In fact the mean home team score is 115.64 where mean visitor score is 112.92. Marginally different, but this confirms the hypothesis that I had about these distributions. 

## Which teams have the highest mean points per game?

For my own curiosity I wanted to know which teams were scoring the highest points. The top five average scorers were:

| Team | Average Points Per Game |
| ------ | ----- |
| Scramento Kings | 120.875 |
| Golden State Warriors | 118.029 |
| Atlanta Hawks | 117.667 |
| Utah Jazz | 117.193 |
| Milwaukee Bucks | 116.484 |

If you follow basketball, you know that most of these teams were ranked at least high enough to go to the playoffs (sorry Jazz). If you don't follow basketball (this must be so boring for you so thanks for sticking around), I added their pre-playoff rankings for your convenience. 

| Team | Average Points Per Game | Rank |
| ------ | ----- | ----- |
| Scramento Kings | 120.875 | 3 |
| Golden State Warriors | 118.029 | 6 |
| Atlanta Hawks | 117.667 | 7 |
| Utah Jazz | 117.193 | NR | 
| Milwaukee Bucks | 116.484 | 1 |

I don't know that there's much we can draw from this other than concluding that obviously Sacramento is the highest scoring team, on average. Based on this extremely unscientific addition of team rank to the table, there doesn't seem to be much correlation between points per game and rank.

## Is there a difference between in-season points and postseason points?

The second thing I wanted to look at regarding points was if there is a difference in distribution for in-season and postseason points. Due to the difference in frequency of in-season game and postseason games, it was a little hard to represent these with a histrogram. I chose to use a boxplot instead. 

![points distribution](/assets/img/Viz/postseasonBoxPlot.png)

My hypothesis on this one was that the playoffs would have had a higher mean than in-season games, but I was incorrect. The mean of in-season games is marginally higher, and there is a wider spread distribution (although some of this could be caused by the aforementioned fact that there are way more observations that are in-season games than postseason games). 

I think we can conclude now that there's much more to being 'good' at basketball than how many points you're scoring per game. HOWEVER, if you want to win games you have to score the most points. Is there a correlation between win-loss record and points scored per game?

## What is the correaltion between average points and win/loss record? What about average opponents points and own win/loss record?

![visualization](/assets/img/Viz/ppgVsWinLossRecord.png)
![visualization](/assets/img/Viz/opPPGvsWinLoss.png)

We can see some correlation in both of the plots, the first with positive correlation and the second negative, as could be expected. Interestingly, the plots have a pretty similar shape but mirrored. But, the team that has the highest average points (which we now know is the Kings) clearly doesn't have the highest win-loss record. It looks like scoring the most points per game on average doesn't guarantee the most wins, but we can say that it certainly doesn't hurt. Something that I'm even more sure of from these graphs, though, is that if you're holding your opponent at a lower number of points per game (on average of course), your win percentage is going to be higher. 

Enough looking at points and wins. Let's look at something else. 

# What does height have to do with basketball performance?

If you were tall as a kid (or now I guess) you're familiar with the stereotype that tall kids are better at basketball. Is that true for the pros too?

Let's start by looking at the distribution of heights. How tall are these players really?

It is important to know that not every player has a height recorded, so this data takes only the observations with a recorded height.

![heights](/assets/img/Viz/heights.png)

Along the x axis is height in inches. We have a spread of 72 to 85 inches, with a mean of 79.03. In a more familiar feet-inches unit, thats a range of 6' to just over 7', with an average of about 6'7. 

## Does the tallest player score the most points?

I know I promised we would stop talking about points but I just wanted to look at this really quickly.

![height x points](/assets/img/Viz/heightVsPoints.png)

Interestingly enough, it doesn't seem like the taller players score any more points than the shorter ones. What about shot percentage?

## Does the tallest player have the best shot percentage?

![height x shot %](/assets/img/Viz/heightVsAverage.png)
![height x 3 pts %](/assets/img/Viz/heightVs3Pt.png)

These plots map height and shot percentage for normal field goals and three point shots. It looks like there is an increase in percent of shots made for the taller players when it comes to normal field goals. For three point shots, however, the taller players don't necessarily have the best 3 point shot percentage. And this even shows that some of the talles players have some of the worst 3 point percentages. 

I'm finally done talking about points now. Let's explore how height affects other metrics. 

## Does the tallest player get the most rebounds and blocks?

Instinct would say that taller players have an advantage over shorter ones when it comes to blocks and rebounds. 

![blocks](/assets/img/Viz/blocks.png)
![rebounds](/assets/img/Viz/rebounds.png)

This data confirms our instinct. It appears that taller players get more blocks and rebounds per game. 

## How tall are the players in each position?

![tall](/assets/img/Viz/heightXPosition.png)

This isn't surprising at all. The Centers are on average the tallest players in the league.

# What do each of the Points, Steals, Rebounds, Blocks, and Assists metrics look like for each position?
Looking at height by position made me wonder what each of these metrics were like for each position. In all sports, different positions are known for having different technical skills so I would assume the means for each metric will vary based on position. For our last exploration, let's look at the boxplots for each of these.

### Points
![points](/assets/img/Viz/PointsXPosition.png)

### Steals
![steals](/assets/img/Viz/StealsXPosition.png)

### Rebounds
![rebounds](/assets/img/Viz/ReboundsXPosition.png)

### Blocks
![blocks](/assets/img/Viz/BlocksXPosition.png)

### Assists
![assists](/assets/img/Viz/AssistsXPosition.png)


# Conclusion
With an EDA like the one demonstrated here, there is no real conclusion to the data. But I can have a conclusion to my post! In this EDA I barely scratched the surface of the exploration we can do on this data. Hopefully it's clear that my original questions led to new questions and I was able to create some interesting visualizations. My purposes with this dataset obviously aren't going to lead to anything world changing for the NBA; this is mostly just out of my own personal interest. 

In my next EDA with this dataset I hope to explore the relationship between teams win-loss record and some sort of algorithmic combination of each player's personal stats. Hopefully you too have come up with an idea for your next EDA!


