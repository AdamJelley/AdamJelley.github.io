---
title: Football Match Prediction
summary: A project to predict the outcomes of football matches.
tags:
date: "2020-04-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: My football match prediction webapp running live on a Sunday evening in November 2019
  focal_point: Smart

links:
# - icon: twitter
#   icon_pack: fab
#   name: Follow
#   url: https://twitter.com/georgecushen
url_code: "https://github.com/AdamJelley/football-predictions"
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---

My football match prediction webapp running live on a Sunday evening in November 2019. This is a picture of an early version, but unfortunately is the only picture I still have... I later improved the performance and UI and got to around 70% accuracy over win/lose/draw predictions, but eventually came up against the hard truth that football games have a substantial component of randomness that is impossible to predict before the match begins, no matter how much you improve your algorithm and data!

# Project Overview
The aim of this project was to predict the outcome of football matches. The final version provided up to date predictions of all Premier League and Championship games, but is no longer running due to the cost of the API data and hosting the webapp.

# Data
The data comes from [api-football](https://www.api-football.com/), for which the various data feeds are well documented [here](https://www.api-football.com/documentation#documentation-v239-api-architecture).

In the current version, we used the match fixtures API to get historical match results for the Premier League and Championship for the last 10 years, as well as to get the upcoming fixtures for the next week.

# Flow

The first python recipe ([compute_Leagues](recipe:compute_Leagues)) is used to get the available leagues and their corresponding IDs from the API. This data is cleaned and filtered down to the leagues of interest to give the [Leagues_prepared_filtered](dataset:Leagues_prepared_filtered) dataset.

These league IDs are then used to get all the corresponding [historical fixtures](dataset:Fixtures_prepared) (from beginning of 2010 season to yesterday inclusive), as well as the [upcoming fixtures](dataset:Upcoming_Fixtures_prepared) (from today for the next week inclusive), from the API.

We then use a custom developed [plugin](recipe:compute_Team_Elo_Ranks) to compute the Elo ratings (wikipedia: [Elo Ratings](https://en.wikipedia.org/wiki/Elo_rating_system)) for each team over the fixture history. Elo ratings originate from chess but also provide an accurate way of ranking football teams over time. We extract the most recent Elo ratings for each team from the history using [SQL](recipe:compute_Latest_Team_Elo_Ranks) which can be [joined](recipe:compute_Upcoming_Fixtures_EloFeatures) to the upcoming fixtures.

We trained a simple [logistic regression ](saved_model:STX882mM) algorithm on these ranks from the fixtures history to predict the outcome of the game. This model is then used to score the  [upcoming fixtures](dataset:Upcoming_Fixtures_prepared) to get the model [predictions](dataset:Upcoming_Fixtures_EloFeatures_scored) for the next week of fixtures. We also evaluate the model on the historical fixtures so we can trace the accuracay of [historical predictions](dataset:Historical_Fixtures_Evaluated) as well.

# Automation

There are currently 5 scenarios: 4 of which are run daily in sequence starting at 0200 UTC, one of which is run weekly on a Sunday at 0400 UTC (in addition to one just to re-build the entire flow from scratch) which are used to automate the project:

 - **Compute Latest Ranks (Daily, 0200 UTC)**
   This updates the historical fixtures table with the latest results, recalculates the Elo ranks for the entire history and then extracts the most recent Elo rank for each team.

-  **Get Upcoming Fixtures (Daily, after Compute Latest Ranks completes)**
   This gets the upcoming fixtures for the next week including the current day.

-  **Predict Historical Fixtures (Daily, after Get Upcoming Fixtures completes)**
   This uses the model to evaluate all historical fixtures (get predictions and compare them against the result to see if they were true or false).

-   **Predict Upcoming Fixtures (Daily, after Predict Historical Fixtures completes)**
   This uses the model to predict all upcoming fixtures.

-  **Retrain Model (Weekly, Sunday 0400 UTC)**
   This retrains the model with the complete history of fixtures (including the latest week of fixture results).

# Webapp
<p class="text-center">
<a href="https://dsproj1.dataiku.com/public-webapps/football-predictions">Web Application</a>
</p>

The predictions for the upcoming fixtures and also for the historical fixtures are served via a basic Flask [webapp](web_app:JidtqGs). The webapp provides an interface with buttons to get either the upcoming or historical fixtures. These buttons use JS to access a python backend to get the requested data.

# Dashboard

The predictions are also served via a [dashboard](dashboard:Hsi5bIw). The dashboard has three slides:

 1. The first slide shows the current team rankings, the upcoming fixture predictions and the historical fixture predictions.

 2. The second slide shows an overview of the model, including training information, model performance metrics, the confusion matrix and the prediction density distributions.

 3. The third slide contains the web application described above.
