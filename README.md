# League of Legends Esports Void Grubs Impact Analysis
A project studying the impact of securing void grubs in League of Legends Esports games, conducted at UC San Diego.

Author: Andrew Li

## Introduction
### General Introduction
League of Legends (LoL) is a multiplayer online battle arena (MOBA) game created and developed by Riot Games, where two teams of five players fight each other on a massive battlefield called Summoner’s Rift and try to destroy opposite side’s enemy base (AKA the Nexus) using various champions. Released in 2009, it has since become one of the most successful and influential online competitive games of all time, having tens of millions of monthly players and boasting an incredible and thriving competitive scene. 

In LoL, there are “epic” monsters, which are neutral monsters located between the friendly and enemy jungles and which provide powerful buffs to whichever team kills them. In 2024, Riot introduced a new epic monster: the **void grubs**. These spawned, and could only be killed, during the early stages of games, and up to a total of 6 can be killed in a game. When killed, they give gold and experience, and more importantly, allow the team that killed them to deal extra damage over time (DOT) to enemy structures. Killing more than four void grubs also spawns voidmites whenever friendly teammates hit enemy structures, and these voidmites also deal damage to structures. The Nexus, along with the turrets and inhibitors that defend them, are all structures, and thus void grubs became an important objective to secure for teams.

### Central Question
The central question this project is interested in answering is the following: **How does securing the void grubs affect the game course and statistics of the team that secures them?** This project will use data analysis techniques to quantify and discover the relationship that securing void grubs has with various features, such as structures destroyed, gold differentials, kill differences and game outcomes. We will then use these statistics to create a prediction model that will predict the outcome of a game given said statistics. This model could be used to better understand win conditions in League games, helping teams to devise strategies to play at a higher level.

### Columns
The original dataframe is comprised of 116016 rows and 161 columns. Each row represents the game data of a participant, which can either a player on a team or an entire team, and each column stores an important game statistic or metric. For this project, 13 columns will be needed:

|Column                |Description|
|---                |---        |
|`'participantid'`                |Identifier of a participant representing their side, role, and whether they're a team or a player (1-10 denote players and their specific role, 100 and 200 denote a team)|
|`'gameid'`                |Unique identifier for each individual game played|
|`'league'`                |The league the individual game was played in|
|`'side'`                |The side of the map the participant played on (Red or Blue side)|
|`'result'`                |The result the participant recieved during the game (1 for a win, 0 for a loss)|
|`'teamkills'`                |The number of enemy champion takedowns a participant obtained during a game|
|`'teamdeaths'`                |The number of champion deaths a participant obtained during a game|
|`'dragons'`                |The number of elemental drakes and Elder Dragons a participant secured during a game|
|`'barons'`                |The number of Baron Nashor's a participant secured during a game|
|`'void_grubs'`                |The number of void grubs a participant secured during a game|
|`'towers'`                |The number of enemy turrets a participant destroyed during a game|
|`'inhibitors'`                |The number of enemy inhibitors a participant destroyed during a game|
|`'visionscore'`                |The total vision score (a value that reflects the amount of vision controlled) a participant earned during a game.|
|`'totalgold'`                |The total amount of gold earned by a participant during a game|

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
To prepare the dataset for analysis, it must first be cleaned, and the first step was to only include games with a `'participantid'`of 100 or 200, as those are used to denote the entries of teams instead of individual players, and only those entries include all of the relevant data needed (entries for players do not include key information such as total void grubs taken, total towers taken, total team gold earned, etc.).

It was also necessary to include only the relevant columns: `'gameid'`, `'league'`, `'side'`,`'result'`, `'teamkills'`, `'teamdeaths'`, `'dragons'`, `'barons'`, `'void_grubs'`, `'towers'`, `'inhibitors'`, `'visionscore'`, and `'totalgold'`. All of these rows will be needed for future use and analysis. The dataset also includes a few games from 2023, and so it was necessary to remove those rows as those games weren't played during the 2024 season that void grubs were introduced in.

The data set we end up with contains 18798 rows and 13 columns, 10 of which hold quantitative information regarding the overall performance of a team in a game, 2 of which (`'gameid'` and `'league'`) containing categorical information on what specific game the information was from and what league the game was played in, and `'side'` containing categorical information on what side of the map each team played on. Below is the head of the smaller_df dataframe:

| gameid           | side  | league  |   result |   teamkills |   teamdeaths |   dragons |   barons |   void_grubs |   towers | inhibitors   |   visionscore | totalgold   |
|:-----------------|:------|:--------|---------:|------------:|-------------:|----------:|---------:|-------------:|---------:|:-------------|--------------:|:------------|
| LOLTMNT99_132542 | Blue  | TSC     |        1 |          20 |            7 |       2.0 |      1.0 |          0.0 |      9.0 | 1.0          |           186 | 52523       |
| LOLTMNT99_132542 | Red   | TSC     |        0 |           7 |           20 |       1.0 |      0.0 |          6.0 |      1.0 | 0.0          |           141 | 39782       |
| LOLTMNT99_132665 | Blue  | TSC     |        1 |          31 |           20 |       2.0 |      1.0 |          4.0 |      8.0 | 1.0          |           251 | 72355       |
| LOLTMNT99_132665 | Red   | TSC     |        0 |          20 |           31 |       3.0 |      1.0 |          2.0 |      8.0 | 1.0          |           251 | 66965       |
| LOLTMNT99_132755 | Blue  | TSC     |        1 |          24 |            8 |       2.0 |      1.0 |          2.0 |      9.0 | 1.0          |           261 | 68226       |

**NOTE** that this cleaned dataframe will need to be further adjusted and modified for other specific parts of the project.

### Univariate Analysis
In this exploratory data analysis, univariate analysis was performed to examine the distribution of `'totalgold'`, the total amount of gold earned by a team in a game.


<iframe
  src="assets/golddist.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>. 

The histogram shows that the distribution of `'totalgold'` is nearly normal with almost no skew. This indicates that, most of the time, teams end games with `'totalgold'` only a few standard deviations away from the mean `'totalgold'` value, and it is rare for games to end with one team having an extremely low or high `'totalgold'` value.

Univariate analysis was also performed to examine the distribution of `'void_grubs'`, the total number of void grubs taken by a team in a game. 

<iframe
  src="assets/voidgrubsdist.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>. 

The histogram shows that `'void_grubs'` has a trimodal distribution, with the bars of 0, 3, and 6 void grubs being much longer than the ones of 1, 2, 4, and 5 void grubs. This is probably due to the fact that void grubs spawn twice in groups of 3, meaning that, most of the time, teams will try to secure all void grubs in a group, leading to the grouping of void grub secures at multiples of 3. 

### Bivariate Analysis
Bivariate analysis was performed to find the correlation between `'void_grubs'` and `'result'`. A bar graph was created that shows the ratio of wins to the total number of games (win rate) for each number of void grubs taken by a team. 

<iframe
  src="assets/grubswinrate.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>. 

The bar graph shows a distinct positive correlation between void grubs taken and win rate. The win rate of teams increases with the number of void grubs taken they take, indicating that taking more void grubs increases a team's chances of winning the game. 

### Interesting Aggregates
A second dataframe, which was named grouped, was also made to find interesting aggregates. This dataset was created from smaller_df by first grouping on `'void_grubs'` and creating several new columns: `'result_sum'`, the total number of wins teams earned after getting a specific number of void grubs, `'count'`, the count of games where the specific number of void grubs was obtained by a team, `'avg_towers'`, the average number of towers taken in a game by teams that had secured a specific number of void grubs, `'avg_totalgold'`, the average amount of total gold earned by teams that had secured a specific number of void grubs, and `'avg_visionscore'`, the average amount of vision score earned by teams that had secured a specific number of void grubs.

`'result_sum'` was calculated by applying a sum aggregation function on `'result'` from smaller_df.
`'count'` was calculated by applying a size aggregation function on `'void_grubs'` from smaller_df.
`'avg_towers'`, `'avg_totalgold'`, and `'avg_visionscore'` was calculated by applying a mean aggregation function on `'towers'`, `'totalgold'`, and `'visionscore'` respectively, from smaller_df.

Two more columns were added to the new dataframe: `'winrate'`, which was created by dividing the `'result_sum'` column by the `'count'` column and which shows the ratio of games won to total games played for each number of void grubs secured, and `'percent of games'`, which was created by dividing the `'count'` column by its sum and which shows the proportion of games each `'void_grubs'` value represents out of the total number of games.

This is what the grouped pivot table looks like:

<iframe
  src="assets/groupedaggs.html"
  width="800"
  height="250"
  frameborder="0"
></iframe>

What's interesting to see is that not only does winrate increase with number of void grubs taken by teams, but the number of towers taken also noticeably increases with the number of void grubs taken by teams. Even more interesting is that taking void grubs doesn't seem to have much of a noticeable impact on total gold earned after 3 void grubs, and it seems to have almost no impact on average vision score. While the data still heavily implies that taking void grubs significantly impacts the flow of the game and allows teams to get more towers and win games more often, it also shows that taking void grubs may not necessarily affect other features as much.

## Assessment of Missingness
### NMAR Analysis
I believe that the the missingness of many values in the `'xpat25'` column are not missing at random (NMAR). `'xpat25'` records the amount of experience (earned mostly by killing minions, monsters and enemy champions, used to level up your champion in order to increase their power and abilities) a participant has earned by 25 minutes. While several regions don't have `'xpat25'`or other features regarding statistics at specific times simply because the websites for those regions that the dataset obtained the information from didn't contain those statistics, there are regions that record `'xpat25'` data yet still have games where it is missing. 

I believe that those instances of missing data are NMAR because some games don't last until 25 minutes, with one team winning before then. This means that the missingness of these data values depends on themselves. To obtain data that could explain the `'xpat25'` missingness and make it missing at random (MAR) instead, an additional column that tracks whether a game goes to 25 minutes (lets call this hypothetical column `'lasted_to_25'`) could be made which returns whether or not a game lasted to 25 minutes. 

### Missingness Dependency
This section investigates the missingness of `'void_grubs'` in relation to other columns, specifically to see if its missingness depends on them. The first column I used was `'league'`. The second column I used was `'side'`. The significance level used for both permutation tests was 0.05. Both tests used total variation distance (TVD) as the test statistic. Both tests used 1000 shuffles to collect 1000 samples. 

The first permutation test was performed on `'void_grubs'` and `'league'`.

**Null Hypothesis:** The distribution of `'league'` is the same regardless of whether or not `'void_grubs'` is missing.

**Alternative Hypothesis:** The distribution of `'league'` is **NOT** the same regardless of whether or not `'void_grubs'` is missing.

**Test Statistic**  The difference in league distributions between `'void_grubs'` missing (NA) and not missing (non-NA).

Below is the observed distribution of `'league'` in relation to when `'void_grubs'` is missing and not missing.

<iframe
  src="assets/leaguevgmissing.html"
  width="800"
  height="1300"
  frameborder="0"
></iframe>

After performing permutation testing, the **observed TVD** was found to be 0.9905316824471959, and the **p-value** was found to be 0. The plot below shows the empirical distribution of the TVD's, along with the observed TVD. 

<iframe
  src="assets/vgmissingleague.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

At this value, I reject the null hypothesis, as the p-value was found to be less than the significance level. The distribution of `'league'` is **NOT** the same regardless of whether or not `'void_grubs'` is missing, indicating that various regions may have vastly different ratios of non-missing `'void_grubs'`data to missing `'void_grubs'` data compared to others.

The second permutation test was performed on `'void_grubs'` and `'side'`.

**Null Hypothesis:** The distribution of `'side'` is the same regardless of whether or not `'void_grubs'` is missing.

**Alternative Hypothesis:** The distribution of `'side'` is **NOT** the same regardless of whether or not `'void_grubs'` is missing.

**Test Statistic**  The difference in league distributions between `'void_grubs'` missing (NA) and not missing (non-NA).

Below is the observed distribution of `'side'` in relation to when `'void_grubs'` is missing and not missing.


<iframe
  src="assets/sidevgmissing.html"
  width="800"
  height="150"
  frameborder="0"
></iframe>

After performing permutation testing, the **observed TVD** was found to be 0, and the **p-value** was found to be 1. The plot below shows the empirical distribution of the TVD's, along with the observed TVD. 

<iframe
  src="assets/vgmissingside.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

At this value, I fail to reject the null hypothesis, as the p-value was found to be greater than the significance level. The distribution of `'side'` is the same regardless of whether or not `'void_grubs'` is missing, indicating that the `'side'` a team plays on in a game does not affect if `'void_grubs'` gets recorded or not.
