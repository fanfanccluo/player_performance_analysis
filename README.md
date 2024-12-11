# Analyzing the Relationship Between Player's Character Pool Size and Winning Rate

This is the Final Project for DSC80 at UCSD

**Author(s)**:Junhao Yang,Zifan Luo

## Introduction
League of Legends (LoL) is one of the most popular multiplayer online battle arena (MOBA) games globally, with millions of players and a thriving professional esports community. The dataset used in this project, sourced from [Oracle’s Elixir](https://drive.google.com/drive/u/1/folders/1gLSw0RLjBbtaNy0dgnGQDAZOHIgCe-HH), contains detailed match data from professional LoL esports matches throughout 2022. The main goal of our project is to explore the **relationship between a player's character pool size and their winning rate**. By exploring this relationship, we aim to question the common assumption that mastering a diverse range of characters potentially leads to higher match success. Our analysis will provide valuable insights into whether a player's character pool size plays a more significant role in determining match outcomes.

The dataset contains 150180 rows and 161 columns, offering comprehensive insights into gameplay metrics, player performance, and team dynamics in professional matches.The first column is the 'gameid', which uniquely identifies each match. The remaining 160 columns contain various game metrics including individual player statistics, match outcomes, and in-game events. Among all the rows in the dataframe, each 'gameid' corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams.

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
- `split`: A time period within the competitive season.

## Data Cleaning and EDA
### Data Cleaning
First of all, we only get the columns needed for the hypothesis testing(`position`,`playername`,`playeris`,`champion`,`result`) and rows that only contain players' information and store them in `df1`. Next, we  also get the columns needed for the prediciton model(`gameid`,`league`,`teamname`,`teamid`,`gamelength`,`dpm`,`eanred gpm`,`vspm`,`pick1-5`)and only include rows about teams and store everything in `df2`. 

For `df1`, we remove all the rows that contain `NaN` and add a column `played` showing the number of games played and group the dataset by `playerid` and `playername`. Besides, we also add a column `num_characters` that represents the number of unique characters each player used. Another column `weighted_num_characters`(calculated by shannon_diversity)is added to represent the weighted number of characters of each player. Players that played for more than 20 games are kept in the dataframe. Lastly, an additional column`winning_rate`is calculated to display the winning rate of each player and the dataframe is sorted from the highest `weighted_num_characters`. Below is the final cleaned `df1_temp`(first 5 rows): 


| position   |   played |   result |   num_characters |   weighted_num_characters |   winning rate |
|:-----------|---------:|---------:|-----------------:|--------------------------:|---------------:|
| sup        |       77 |       58 |               22 |                   4.11723 |       0.753247 |
| jng        |       59 |       32 |               15 |                   3.27323 |       0.542373 |
| jng        |       46 |       29 |               14 |                   3.30532 |       0.630435 |
| top        |       77 |       59 |               16 |                   3.66457 |       0.766234 |
| jng        |       35 |       24 |                8 |                   2.77841 |       0.685714 |


For `df2`, we just remove all the rows that contain `NaN`. Here's the final cleaned dataframe:


| gameid           | league   | teamname           | teamid                                  |   gamelength |     dpm |   earned gpm |   vspm | pick1    | pick2     | pick3    | pick4     | pick5        |
|:-----------------|:---------|:-------------------|:----------------------------------------|-------------:|--------:|-------------:|-------:|:---------|:----------|:---------|:----------|:-------------|
| 8401-8401_game_1 | LPL      | Oh My God          | oe:team:f4c4528c6981e104a11ea7548630c23 |         1365 | 1762.02 |      1326.02 | 7.1209 | Jinx     | Jarvan IV | Nautilus | Syndra    | Gwen         |
| 8401-8401_game_1 | LPL      | ThunderTalk Gaming | oe:team:df80f468a3f9a722df056fe9104f052 |         1365 | 1337.01 |      1021.41 | 6.8132 | Xin Zhao | Thresh    | Aphelios | Vex       | Jax          |
| 8401-8401_game_2 | LPL      | Oh My God          | oe:team:f4c4528c6981e104a11ea7548630c23 |         1444 | 2482.52 |      1586.26 | 7.4792 | Jinx     | Xin Zhao  | Rakan    | Rumble    | Corki        |
| 8401-8401_game_2 | LPL      | ThunderTalk Gaming | oe:team:df80f468a3f9a722df056fe9104f052 |         1444 | 1459.65 |      1040.78 | 6.6066 | Lee Sin  | Leona     | Ziggs    | Gangplank | Twisted Fate |
| 8402-8402_game_1 | LPL      | FunPlus Phoenix    | oe:team:33d17f3717f58e12a3da80b377221fb |         1893 | 1719.94 |      1373.19 | 8.6846 | Jinx     | Viego     | Thresh   | Corki     | Graves       |


### Univariate Analysis
We performed Univariate Analysis on `weighted_num_characters` column of `df1_temp` to examine its distribution. 

<iframe
  src="assets/weighted_num_characters_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution appears to be approximately normal, with most values clustering around 3.0–3.5.
There are fewer players with extremely low or high values of weighted_num_characters, indicating that most players maintain a moderate range of champion diversity.

We also look at distribution of `gamelength` of `df2`

<iframe
  src="assets/gamelength.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The distribution is slightly skewed to the right, with most games lasting around 1800–2000 units. There are fewer games that are very short or very long,most matches have standard length.

### Bivariate Analysis
We performed Bivariate Analysis on the `weighted_num_characters`and `winning_rate` of `df1_temp`.

<iframe
  src="assets/scatter1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The points are widely spread, and there doesn’t appear to be a strong positive or negative linear correlation between the two variables. This suggests that having a higher or lower weighted number of characters may do not strongly influence winning rate.

We also performed the Analysis on the `played` and `num_characters` to see the relationship between the number of games played and the variety of characters utilized by the players. 

<iframe
  src="assets/scatter2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There is a weak positive trend where players who have played more games tend to use a slightly larger variety of characters. However, this relationship is not strong, as there is considerable variance in the number of characters used for any given number of games played.

### Interesting Aggregates
We aggregated the `df1_temp` dataset by position. We summed the `number` column and averaged the remaining columns. Here's the outcome:

| position   |   played |   winning rate |   num_characters |   weighted_num_characters |   number |
|:-----------|---------:|---------------:|-----------------:|--------------------------:|---------:|
| bot        |  51.4418 |       0.494947 |          13.4299 |                   3.26211 |      421 |
| jng        |  49.8433 |       0.496166 |          13.0853 |                   3.24513 |      434 |
| mid        |  50.7281 |       0.493927 |          15.3783 |                   3.5254  |      423 |
| sup        |  48.9165 |       0.49916  |          13.7007 |                   3.29368 |      431 |
| top        |  50.9108 |       0.500978 |          14.9859 |                   3.44844 |      426 |

We can tell from this dataframe that the midlane players(`mid`) stand out for their large character pool,highlighting the need for flexibility in this role. Also, toplane players have the highest winning rate, suggesting that success in this position may be influenced by factors such as individual performance.

## Assessment of Missingness
### NMAR Analysis
In our dataset, we believe that the missingness of `patch` column can be seen as Not Missing At Random(NMAR)** because its absence may be inherently tied to the nature of the games being hosted. Specifically, the missingness could arise due to the fact that some games might be played or recorded on patches that have not been officially released or documented at the time of data collection.

### Missingness Dependency
In this part, we're going to test if the missingness of `split`is depend on `league`. The significance level we chose would be 0.05 and the test statistic would be the Total Variance Distance (TVD).
- **Null Hypothesis**: The missingness of `split` does not depend on `leuage`.
- **Alternative Hypothesis**: The missingness of `split` depends on `league`.
First of all, we prepared a dataset to show the missingness of split in different leagues(from the original dataframe):

| league   |   split_missing = False |   split_missing = True |
|:---------|------------------------:|-----------------------:|
| ASCI     |               0         |             0.0221094  |
| CBLOL    |               0.0265052 |             0          |
| CBLOLA   |               0.0235602 |             0          |
| CDF      |               0         |             0.0227069  |
| CT       |               0         |             0.00776815 |

After we performed a permutation test, we found the **observed test statistic(TVD)** for the test is **0.814** and the p-value is **0**. Here's the visualization that shows the empirical distribution of the TVD for the test:

<iframe
  src="assets/tvd_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As the p-value is less than the significance level(0.05), so we can reject the null hypothesis. Thus, the missingness of `split` column actully depends on the `league` column. 

The second permutation test we performed is to determine whether the the missingness
of `split` depends on `position`
- **Null Hypothesis**: missingness of `split` doesn't depend on `position`.
- **Alternative Hypothesis**: missingness of `split` does depend on `position`.
We also created a dataframe that displays the missingness of split according to position and turned it into a pivot table to make it easier to read.

| position   |   split_missing = False |   split_missing = True |
|:-----------|------------------------:|-----------------------:|
| bot        |                0.166667 |               0.166667 |
| jng        |                0.166667 |               0.166667 |
| mid        |                0.166667 |               0.166667 |
| sup        |                0.166667 |               0.166667 |
| team       |                0.166667 |               0.166667 |

The **observed tes statstic(TVD)** was found to be 0 and p-value equals to 1. We can visualize the empirical distribution: 

<iframe
  src="assets/tvd_hist2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As the p-value is bigger than the significance level, so we fail to reject the null hypothesis that the missingness of `split` does not depend on `position`.

## Hypothesis Testing
In the hypothesis testing, we aim to assess whether there is a correlation between the weighted_num_characters and winning rate. 
- **Null Hypothesis**: There is no correlation between weighted_num_characters and winning rate.
- **Alternative Hypothesis**:  There is correlation between weighted_num_characters and winning rate.
- **Test Statistics**: Pearson correlation coefficient (r)
- **Significance Level**:0.05
After performing the permutation test and we calculated the **observed Pearson correlation** to be 0.124 and the p-value is 0. 
We can take a look at the histogram of the correlation distribution:

<iframe
  src="assets/tvd_hist2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is smaller than the significance level 0.05, so we can reject the null hypothesis.This result indicates that the observed correlation is statistically significant, and there is likely a true relationship between the two variables. 

## Framing a Prediction Problem
In the competitive world of League of Legends (LoL), each league, such as LCK (Korea), LCS (North America), and LPL (China), represents not only a geographical region but also a unique identity, defined by its playstyle, champion preferences, and strategic approach. In this last section, we want to build a model which could **predit the league a team belongs to** based on gameplay statistics and performance metrics(`dpm`,`earned gpm`,`gamelength`,`vspm`,`pick1-5`). This is a classification problem, specifically a **multiclass classification** task, as teams can belong to multiple leagues such as LCK, LCS, LPL or LEC. The **response variable** would be `league` as understanding a team's league based on gameplay metrics can provide insights into regional gameplay styles, strategies, and performance characteristics. This prediction can also help in analyzing trends and differences across leagues. The evaluation metric that we chose for our model is **Macro F1 score** as it balances well the two important aspects of classification(precision&recall) and also it handles the class imbalance well,which is critical for our dataset, as the distribution of classes is uneven. It is also well-suited for our multiclassifer probelm as it is calculated by taking the average of the F1 scores for each class in a multi-class problem. By focusing on the Macro F1 score, we ensure that our model evaluates performance fairly across all `league`. 

## Baseline Model
We used **Random Forest Classifier** as our baseline model following 4 features including:
`dpm`,`earned gpm`,`gamelength`,`vspm`.  Among these four features, all of them are quantitative. Since they are all quantitative variables, we did not perform any encoding at this stage. After fitting the model,we evaluated our model using the Macro F1 score, which provides a better measure of performance.The Macro F1 score for our model is **0.309**. This relatively low Macro F1 score tells us that the model's challenges with correctly classifying less-represented leagues, particularly due to a lower recall for these classes, which indicates that many true positives were missed. 
We would consider this model is mot yet 'good' enough for practical use(but at least it's slightly better than random wild guess). Improvements in feature selection&engineering and model optimization are necessary to create a more reliable classifier.

## Final Model
For our final model, we added another 5 features: `pick1`,`pick2`,`pick3`,`pick4`,`pick5`(These features represent the champion picks for a team in a match). Champion picks are a key component of a team’s strategy and are often region-specific. Certain champions may be prioritized or avoided in specific leagues due to meta trends, player preferences, or regional playstyles.
Including these features allows the model to capture strategic differences across leagues, which is critical for predicting a team’s league affiliation. We still used a **Random Forest Classifier**. Ad the added features are categorical variables, so we first **OneHotEncoded** them. And then we performed 5-fold cross-validation using **Macro F1-score** as the evaluation metric and tested the following hyperparameter max_depth values: [2, 3, 5, 7, 10, 20, 50, 100, 200, 500, None]. We found out that the **best-performing max_depth** was **50**, achieving a mean cross-validation Macro F1 score of **0.33**. It was still a pretty low number but definitely a slight imporvement from our baseline model with an imporved Macro F1 score. Let's take a look at the confusion matrix:

<iframe
  src="assets/cm.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As we can tell from the graph, the model struggles to predict LCS and LEC accurately. We need to improve our model so it could perform better on minority classes (e.g., LCS and LEC). Addressing class imbalance through techniques like oversampling or class weighting could improve predictions for these leagues. Additional features specific to LCS and LEC gameplay strategies may help reduce misclassifications.

## Fairness Analysis
We want to see if this model is fair among different groups, especially for teams in LCK, LPL and teams in LEC, LCS.
**GroupX** is LCK and LPL (Major Asian leagues) and **GroupY** is LEC and LCS(Western leagues).
**Evaluation Metric** is accuracy, specifically the difference in accuracy between Group X (LCK and LPL) and Group Y (LEC and LCS).
- **Test Statistic**: Observed accuracy difference between Group X and Group Y
- **Significance Level**: 0.05
  
The followings are our hypothesis:

- **Null Hypothesis**:Our model is unfair. There is a significant difference in the model's accuracy between Group X and Group Y.
- **Alternative Hypothesis**:Our model is fair. There is no significant difference in the model's accuracy between Group X and Group Y.

After conducting the test, we found out that the **Observed accuracy difference** is about 0.6686 and **p-value** is 0. The p-value is smaller than the significance level. Thus, we can **reject** the null hypothesis and conclude that our model is **Fair!**

