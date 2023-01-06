# NFL Draft Classification

# **Author**: Josh Palgon

![Vic](./Images/vic.jpg)

## Overview

My New York Jets are looking to gain additional insight into the most important area to improve their team...the NFL draft. I am going to help Joe Douglas and the Jets front office better understand what types of players get drafted in the first and which ones do not.

My data was web scraped from sports-reference.com and merged with the NFL_data Python package. The scraping took some time but once I found out the sites web scraping rules, I was able to set a sleep timer and properly run it all.

Premium positions were extremely important for determining whether a player was drafted in round 1. 4 of the top 5 position ratios of players drafted in the first round were premium positions. Additionally, college conference had a similar effect all of the top 5 college conference ratios come from a power 5 conference.

Every single position had a lower age for round 1 players than non round 1 players. 

## Business Problem

The Jets are trying to break the NFL's longest playoff drought. With the NFL draft being the best way to improve the team, they are looking to understand which type of players get drafted in the first round and which ones do not.

Understanding which players get drafted in the first round or not has several applications but the primary application would be when choosing between several players in the first round. If the front office is having a hard time deciding between multiple players (which happens often) they can select the player who was predicted to be a first round pick versus any players who weren't in hopes they could select that player later in the draft.

## Data

The majority of my data was web scraped from sports-reference.com. This was my first time web scraping which was somewhat problematic for completing this project. There were two prongs to scraping the data from sports-reference. The first prong of getting the list of players for each draft from 2013-2022 was actually done by Savvas Tjortjoglou. Almost all of his worked perfectly but I made a few adjustments to make it run properly and for my time period. Here is the link of his work http://savvastjortjoglou.com/nfl-draft.html. There were a few very important data points I got from the first prong but the bulk of the data and my time lied in the second prong. 

The second prong consisted of scraping each players college stats page with the  link that I got in part of the first prong. Most of my issues came from this prong mainly due to hard to scrape tags that were not the players' primary stat category. For example QBs passing data was fairly easily scrapable but their rushing stats table while tagged the same way did not come that easy. In the end I was not able to get any secondary or beyond category. Once I figured out how to scrape the primary categories for each player I kept running into 429 HTTP Error requests. Right before I was about to give up on this data, I remembered coming across sports-reference's site on their data usage. They actually had a page that gave an explicit description of what they block. It turns out I just needed to run a sleep time of greater than 3 seconds for that error to no longer occur. They do update their page so I proved the link here https://www.sports-reference.com/bot-traffic.html.

Additionally, I supplemented the sportsreference data with the NFL_data Python package for a few extra features.

## Methods and Modeling

My first step in understanding the data was to look at features that had the highest correlation with price. Sqft_living was the top feature followed by grade_value and sqft_living15 (the sqft of living for the nearest 15 neighbors). 

My next step was to run my first linear regression model. I wanted to establish a good baseline model to pull out coefficients. Waterfront ($608,976), Grade ($99,787), View ($68,547) were among my top coefficients after running my baseline model.

I wanted to start my model as simple as possible so I just ran a linear regression on price using sqft_living (R2 = 0.49). Over 4 steps, I added some of the most important features to get an R2 = 0.70.

In my next modeling step I took the log of price and again started with just price (this time logged) using sqft_living (R2 = 0.45). While it started off lower than my first block of models after adding more important features I got an R2 = 0.77.

Next, I One Hot Encoded zip code. With just sqft_living and zip code One Hot Encoded my R2 = 0.72. Without any log transformations I added some of the most important features to this block and got my baseline R2 = 0.80.

I kept doing this same method on different combinations of standard price, transformed price, standard sqfts', transformed sqfts', One Hot Encoded zip code and no One Hot Encoded zip codes.

My best and final model using logged price, transformed sqfts' and One Hot Encoded zip code got an R2 = 0.88.

My final step was to take the predictions of my final model and compare it to the actual prices. I wanted to look at the area where my model projected a higher price than the actual price to see what in the market might be undervalued. I found waterfront, view, and the zip codes: 98010, 98118, 98146, 98122, 98033 to be my most undervalued features. 

## Results

### Price vs. Sqft_living
![Price vs. Sqft_living](./Images/price_vs_sqft.png)

There is a fairly linear relationship between price and sqft_living. Additionally waterfront homes have higher prices for comparable sqft_living.

### Price vs. Has a View
![Price vs. Has a View](./Images/price_vs_view.png)

There is a much wider distributions of price for houses with a view than without a view. Q3 is particularly higher in view than no view. Very strong positive exponential relationship between price and grade_value. This shows the reason grade_value is one of the top features. 

### Model Results
![Model Results](./Images/model.png)

My model is able to capture 0.88 of the variance in the actual price that can be explained by its predicted price.

## Conclusions

My three step process allowed me to find the features with the highest correlation with price, the features that have the highest coefficients with price after running a baseline model, and the features that are undervalued when being compared to my final model. Therefore Honey and Vinegar Real Estate should focus on houses with high sqft_living, grade_values, view, and sqft_living_15 that is on a waterfront, and are in 98010, 98118, 98146, 98122, 98033 zip codes. 

For the next steps I would like to get housing data from these houses and their prices in future years to see if my model accurately predicted what was undervalued. Additionally, I would like to explore the longitude and latitude aspects of the data to specifically plot areas within zip codes to further hone in my recommendation.

## For More Information

Please look at my full analysis in [my Jupyter Notebooks](./Notebooks) or my [presentation](./King_County_Presentation.pdf).

For any additional questions, please contact:

<ul>
    <li>Josh Palgon (jopalgon@gmail.com)</li>
</ul>
