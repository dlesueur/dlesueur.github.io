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

## What's Going on With these Points?

Like I mentioned, my initial questions were all based on points and their correlation with other metrics. But as soon as I got the data I was curious about the distribution of points.

![point distribution](/Users/daphnelesueur/Documents/dlesueur.github.io/assets/img/Viz/homeVsVisitorPointDistribution.png)

