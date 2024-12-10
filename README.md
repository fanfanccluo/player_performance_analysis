# Analyzing the Relationship Between Player's Character Pool Size and Winning Rate

This is the Final Project for DSC80 at UCSD

**Author(s)**:Junhao Yang,Zifan Luo

## Introduction
League of Legends (LoL) is one of the most popular multiplayer online battle arena (MOBA) games globally, with millions of players and a thriving professional esports community. The dataset used in this project, sourced from [Oracle’s Elixir](https://drive.google.com/drive/u/1/folders/1gLSw0RLjBbtaNy0dgnGQDAZOHIgCe-HH), contains detailed match data from professional LoL esports matches throughout 2022. The main goal of our project is to explore the **relationship between a player's character pool size and their winning rate**. By exploring this relationship, we aim to question the common assumption that mastering a diverse range of characters potentially leads to higher match success. Our analysis will provide valuable insights into whether a player's character pool size plays a more significant role in determining match outcomes.

The dataset contains 12 rows and 161 columns, offering comprehensive insights into gameplay metrics, player performance, and team dynamics in professional matches. The first column is the game ID, which uniquely identifies each match. The remaining 160 columns contain various game metrics including individual player statistics, match outcomes, and in-game events.

Here're the relavent columns:
- `position`: The role or position played by the player in the match (e.g., top, jungle, mid, bot, support).
- `playername`: The name of the player participating in the match.
- `palyer_id`: A unique identifier assigned to each player.
- `champion`: The character or champion selected by the player for the match.
- `result`: The outcome of the match for the player or team
- `gameid`: A unique identifier for each match.
- `league`: The league or tournament in which the match was played.
- `teamname`: The name of the team to which the player belongs.
- `teamid`: A unique identifier for each team.
- `gamelength`: The duration of the match(measured in minutes)
- `dpm`:  Damage per minute dealt by the player during the match.
- `eanred gpm`: Gold earned per minute by the player during the match.
- `vspm`: Vision score per minute, reflecting the player's contribution to map vision.
- `pick1`,`pick2`,`pick3`,`pick4`,`pick5`: The sequence of champion picks for the team during the match's draft phase.

## Data Cleaning and EDA
### Data Cleaning
1. First of all, let's take look at the missing values of the dataset.

2. Next step we only get the columns needed for the hypothesis testing and store them in the `df1`
   
4. We  also get the columns needed for the prediciton model and only include rows about teams in `df1`
   
### Univariate Analysis


### Bivariate Analysis



### Interesting Aggregates


## Assessment of Missingness
### NMAR Analysis

### Missingness Dependency




## Hypothesis Testing


## Framing a Prediction Problem


## Baseline Model

## Final Model

## Fairness Analysis


