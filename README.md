# League of Legends Esports Void Grubs Impact Analysis
A project studying the impact of securing void grubs in League of Legends Esports games, conducted at UC San Diego.

Author: Andrew Li

## Introduction
### General Introduction
League of Legends (LoL) is a multiplayer online battle arena (MOBA) game created and developed by Riot Games, where two teams of five players fight each other on a massive battlefield called Summoner’s Rift and try to destroy opposite side’s enemy base (AKA the Nexus) using various champions. Released in 2009, it has since become one of the most successful and influential online competitive games of all time, having tens of millions of monthly players and boasting an incredible and thriving competitive scene. 

In LoL, there are “epic” monsters, which are neutral monsters located between the friendly and enemy jungles and which provide powerful buffs to whichever team kills them. In 2024, Riot introduced a new epic monster: the **void grubs**. These spawned, and could only be killed (AKA secured or obtained), during the early stages of games, and up to a total of 6 can be killed in a game. When killed, they give gold and experience to the player(s) that secured them, and more importantly, allow the team that killed them to deal extra damage over time (DOT) to enemy structures. Killing more than four void grubs also spawns voidmites whenever friendly teammates hit enemy structures, and these voidmites also deal damage to structures. The Nexus, along with the turrets and inhibitors that defend them, are all structures, and thus void grubs became an important objective to secure for teams.

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
|`'teamkills'`                |The number of enemy player (champion) takedowns a participant obtained during a game|
|`'teamdeaths'`                |The number of friendly player (champion) deaths a participant obtained during a game|
|`'dragons'`                |The number of elemental drakes and Elder Dragons a participant secured during a game|
|`'barons'`                |The number of Baron Nashor's a participant secured during a game|
|`'void_grubs'`                |The number of void grubs a participant secured during a game|
|`'towers'`                |The number of enemy towers (turrets) a participant destroyed during a game|
|`'inhibitors'`                |The number of enemy inhibitors a participant destroyed during a game|
|`'visionscore'`                |The total vision score (a value that reflects the amount of vision controlled) a participant earned during a game.|
|`'totalgold'`                |The total amount of gold (used to buy items that increase a player's power) earned by a participant during a game|

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
To prepare the dataset for analysis, it must first be cleaned, and the first step was to only include games with a `'participantid'`of 100 or 200, as those are used to denote the entries of teams instead of individual players, and only those entries include all of the relevant data needed (entries for players do not include key information such as total void grubs taken, total towers taken, total team gold earned, etc.).

It was also necessary to include only the relevant columns: `'gameid'`, `'league'`, `'side'`,`'result'`, `'teamkills'`, `'teamdeaths'`, `'dragons'`, `'barons'`, `'void_grubs'`, `'towers'`, `'inhibitors'`, `'visionscore'`, and `'totalgold'`. All of these rows will be needed for future use and analysis. The dataset also includes a few games from 2023, and so it was necessary to remove those rows as those games weren't played during the 2024 season that void grubs were introduced in.

The data set we end up with contains 18798 rows and 13 columns, 9 of which hold quantitative information regarding the overall performance of a team in a game, and 4 of which, `'gameid'`, `'league'`, `'result'`, and '`side'`, respectively containing categorical information on what specific game the information was from, what league the game was played in, whether the game was a win or loss, and what side of the map each team played on. 

Below is the head of the smaller_df dataframe:

| gameid           | league   | side   |   result |   teamkills |   teamdeaths |   dragons |   barons |   void_grubs |   towers |   inhibitors |   visionscore |   totalgold |
|:-----------------|:---------|:-------|---------:|------------:|-------------:|----------:|---------:|-------------:|---------:|-------------:|--------------:|------------:|
| LOLTMNT99_132542 | TSC      | Blue   |        1 |          20 |            7 |         2 |        1 |            0 |        9 |            1 |           186 |       52523 |
| LOLTMNT99_132542 | TSC      | Red    |        0 |           7 |           20 |         1 |        0 |            6 |        1 |            0 |           141 |       39782 |
| LOLTMNT99_132665 | TSC      | Blue   |        1 |          31 |           20 |         2 |        1 |            4 |        8 |            1 |           251 |       72355 |
| LOLTMNT99_132665 | TSC      | Red    |        0 |          20 |           31 |         3 |        1 |            2 |        8 |            1 |           251 |       66965 |
| LOLTMNT99_132755 | TSC      | Blue   |        1 |          24 |            8 |         2 |        1 |            2 |        9 |            1 |           261 |       68226 |

**NOTE** that this cleaned dataframe will need to be further adjusted and modified for other specific parts of the project.

### Univariate Analysis
In this exploratory data analysis, univariate analysis was performed to examine the distribution of `'totalgold'`, the total amount of gold earned by a team in a game. Gold can be earned in several ways, and is used by players to buy items for their champions that increase their power in certain ways, playing a key role in the game.


<iframe
  src="assets/golddist.html"
  width="800"
  height="350"
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

|   void_grubs |   result_sum |   count |   avg_totalgold |   avg_towers |   avg_visionscore |   winrate |   percent of games |
|-------------:|-------------:|--------:|----------------:|-------------:|------------------:|----------:|-------------------:|
|            0 |         1530 |    3757 |         56493.1 |      5.13734 |           244.966 |  0.40724  |          0.234052  |
|            1 |          723 |    1642 |         57588.2 |      5.47686 |           250.449 |  0.440317 |          0.102293  |
|            2 |          771 |    1656 |         58530   |      5.84662 |           259.394 |  0.46558  |          0.103165  |
|            3 |         1858 |    3717 |         59383.7 |      6.21308 |           257.913 |  0.499865 |          0.23156   |
|            4 |          679 |    1258 |         59125.9 |      6.51192 |           259.874 |  0.539746 |          0.0783703 |
|            5 |          711 |    1209 |         59608   |      7.03805 |           255.983 |  0.588089 |          0.0753177 |
|            6 |         1753 |    2813 |         59235.6 |      7.36687 |           251.125 |  0.623178 |          0.175243  |

What's interesting to see is that not only does winrate increase with number of void grubs taken by teams, but the number of towers taken also noticeably increases with the number of void grubs taken by teams. Even more interesting is that taking void grubs doesn't seem to have much of a noticeable impact on total gold earned after 3 void grubs, and it seems to have almost no impact on average vision score. While the data still heavily implies that taking void grubs significantly impacts the flow of the game and allows teams to get more towers and win games more often, it also shows that taking void grubs may not necessarily affect other features as much. Another possibility as that taking void grubs may still increase the vision score and gold a team earns per minute, but this causes teams to win faster and thus reduces the avg_visionscore and avg_totalgold of teams in those games.

## Assessment of Missingness
### NMAR Analysis
I believe that the the missingness of values in the `'ban1'`, `'ban2'`, `'ban3'`, `'ban4'` and `'ban5'` columns are not missing at random (NMAR). The ban columns record the champions banned by a team during the draft for a game. In League, certain champions are better against, or "counter", other champions. Some champions also synergize extremely well with each other. Certain players are also extremely good at specific champions. Banned champions are not allowed to be played by either side for that game, and so teams use bans in order to kneecap or weaken the enemy draft. As such, banning champions is a essential part of LoL esports.

I believe that the main reason for missing bans is that, sometimes, players don't have a specific champion to ban for certain reasons (team doesn't agree with what champion to ban, player forgets they are the one picking the banned champion until it's too late, internal drama on the team makes one player want to sabotage the team, etc.), and so they choose to not ban anything instead of banning a random champion. Ultimately, champion bans are decided by the player, and because these missing values appear when players make the conscious choice to not ban any champion, the missingness of these values depends on the values themselves, making the missingness NMAR.

One way to make the bans columns missing at random (MAR) would be by adding a hypothetical column (let's call it `'5_bans'`) to the dataset that tracks if all 5 players on a team decided to ban a champion, returning 1 if they did and 0 if one or more players on a team did not choose a ban. 


### Missingness Dependency
This section investigates the missingness of `'void_grubs'` in relation to other columns, specifically to see if its missingness depends on them. The first column I used was `'league'`. The second column I used was `'side'`. The significance level used for both permutation tests was 0.05. Both tests used total variation distance (TVD) as the test statistic. Both tests used 1000 shuffles to collect 1000 samples. 

The first permutation test was performed on `'void_grubs'` and `'league'`.

**Null Hypothesis:** The distribution of `'league'` is the same regardless of whether or not `'void_grubs'` is missing.

**Alternative Hypothesis:** The distribution of `'league'` is **NOT** the same regardless of whether or not `'void_grubs'` is missing.

**Test Statistic**  The difference in league distributions between `'void_grubs'` missing (NA) and not missing (non-NA).

Below is the observed distribution of `'league'` in relation to when `'void_grubs'` is missing and not missing:

| league          |   grubs_missing = False |   grubs_missing = True |
|:----------------|-----------------------:|------------------------:|
| AC              |              0.0043608 |               0         |
| AL              |              0.0175679 |               0         |
| CBLOL           |              0.0327685 |               0         |
| CBLOLA          |              0.0343882 |               0         |
| CDF             |              0.0085971 |               0         |
| CT              |              0.0048592 |               0         |
| EBL             |              0.0176925 |               0         |
| EBLPA           |              0.0031149 |               0         |
| EM              |              0.0497134 |               0         |
| EPL             |              0.0186893 |               0         |
| ESLOL           |              0.0345128 |               0         |
| EWC             |              0.0023673 |               0         |
| GLL             |              0.0174433 |               0         |
| GLLPA           |              0.0047346 |               0         |
| HC              |              0.0138301 |               0         |
| HM              |              0.0174433 |               0         |
| HW              |              0.010466  |               0         |
| IC              |              0.0079741 |               0         |
| LAS             |              0.0345128 |               0         |
| LCK             |              0.0600548 |               0         |
| LCKC            |              0.0636681 |               0         |
| LCO             |              0.0194368 |               0         |
| LCS             |              0.0239223 |               0         |
| LDL             |              0         |               0.411508  |
| LEC             |              0.0366309 |               0         |
| LFL             |              0.0281585 |               0         |
| LFL2            |              0.0200598 |               0         |
| LIT             |              0.0170695 |               0         |
| LJL             |              0.0184401 |               0         |
| LLA             |              0.0265387 |               0         |
| LPL             |              0         |               0.522214  |
| LPLOL           |              0.0176925 |               0         |
| LRN             |              0.0108398 |               0         |
| LRS             |              0.0123349 |               0         |
| LVP SL          |              0.0287815 |               0         |
| MSI             |              0         |               0.0568099 |
| NACL            |              0.0575629 |               0         |
| NEXO            |              0.0124595 |               0         |
| NLC             |              0.0176925 |               0         |
| NLC Aurora Open |              0.0095938 |               0         |
| PCS             |              0.0368801 |               0         |
| PRM             |              0.0285323 |               0         |
| PRMP            |              0.0166957 |               0         |
| TCL             |              0.020309  |               0         |
| TSC             |              0.0129579 |               0         |
| UL              |              0.0176925 |               0         |
| USP             |              0.0047346 |               0         |
| VCS             |              0.031398  |               0         |
| WLDs            |              0.0148268 |               0.0094683 |

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

Below is the observed distribution of `'side'` in relation to when `'void_grubs'` is missing and not missing:

| side   |   grubs_missing = False |   grubs_missing = True |
|:-------|------------------------:|-----------------------:|
| Blue   |                     0.5 |                    0.5 |
| Red    |                     0.5 |                    0.5 |

After performing permutation testing, the **observed TVD** was found to be 0, and the **p-value** was found to be 1. The plot below shows the empirical distribution of the TVD's, along with the observed TVD. 

<iframe
  src="assets/vgmissingside.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

At this value, I fail to reject the null hypothesis, as the p-value was found to be greater than the significance level. The distribution of `'side'` is the same regardless of whether or not `'void_grubs'` is missing, indicating that the `'side'` a team plays on in a game does not affect if `'void_grubs'` gets recorded or not.

## Hypothesis Testing
As mentioned in the introduction, the primary focus of this project is to study how securing void grubs affects the other game statistics of the team that secures them. For this specific part, I am curious regarding the distribution of the number of kills teams earn in games, specifically if teams that take a large number of `'void_grubs'` (5-6) in games earn more kills in said games than teams that only take a small number of `'void_grubs'` (0-4). The number of kills a team earns in a game is recorded in the `'teamkills'` column of our dataset.

A team can only take up to a maximum of 6 `'void_grubs'` per game, and void mites spawn to assist teams in taking structures, which is a very significant buff, only after the team has taken 5-6 `'void_grubs'`. As such, I used the 5-6 range as the amount of `'void_grubs'` taken that will be considered "large", and 0-4 as the amount of `'void_grubs'` taken that will be considered "small".

To investigate the question, I ran a **permutation** test with the following hypotheses. The test statistic used was absolute difference in means and the significance level was 0.05.

**Null Hypothesis**: On average, teams earn the same number of kills in games regardless of how many void grubs they secure in those games.

**Alternative Hypothesis**: On average, teams that secure a large number of void grubs in games earn more kills in those games than teams that secure a small number of void grubs.

**Test Statistic** The difference in means between `'teamkills'` of teams that secured 5-6 `'void_grubs'` in a game and `'teamkills'` of teams that secured 0-4 `'void_grubs'` in a game.

It was decided to run a permutation test because this test is trying to find if the two distributions look like they come from the same population. 

Earlier, in the interesting aggregates section, it was noted that taking more `'void_grubs'` correlated to teams taking more towers. The alternative hypothesis was chosen because it's believed that, with the enhanced turret-taking potential given to teams by taking 5-6 `'void_grubs'`, teams could use the advantages created by destroying enemy turrets (extra gold, ability to threaten into the enemy's side of the map, positioning advantage in epic monster fights, etc.) to strengthen themselves or find extra opportunites that could lead to more enemy champion kills for those teams.  

Because we have a directional hypothesis, difference in means was used instead of absolute difference. Looking at the difference in means allows us to see which games have more or less teamkills for a given team, which will allow us to answer my question.

To run the test, I first divided the data points into the large and small number of `'void_grubs'` groups discussed earlier. The observed statistic was found to be 1.500371688051544. 

After shuffling the `'teamkills'` values 1000 times to get 1000 simulated differences in means, I got a p-value of 0.

Below is a histogram showing the distribution of the test statistics calculated during the hypothesis test:

<iframe
  src="assets/vgteamkills.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

The results are statistically significant, as the p-value I got (0) was less than the significance level (0.05), so we reject the null hypothesis. We will instead accept the alternative hypothesis, which is that, on average, teams that secure a large number of void grubs in games earn more kills in those games than teams that secure a small number of void grubs. This trend may be because of the earlier explanation I gave of how taking more void grubs seems to lead to taking more towers, which leads to more resources and opportunities for a team to utilize which may allow them to earn more kills in a game. However, this is only speculation.

## Framing a Prediction Problem
I plan on building a model that will predict whether or not a team won a game based off only knowing certain game statistics of that team in that game. This will be a **binary classification** model, since we only have to predict 1 of 2 possible outcomes (win or loss). The response variable will be `'result'`, as that tracks whether or not a team won or lost their game, which is exactly what I am trying to predict with my model. 

We would like to find the proportion of correctly classified instances out of total instances (overall correctness), but while there are a nearly equal number of wins and losses in the smaller_df dataset (9401 losses, 9397 wins), we still need to worry about weighing both outcomes equally (fairness). As such, we will record both accuracy and the F1 score.

"At the time of prediction, which is immediately after the game ends, we would know the following information for each team: `'void_grubs'`, `'dragons'`, `'barons'`, `'inhibitors'`, `'towers'`, `'teamkills'`, and `'teamdeaths'`. The `'result'` for a game is also only known for certain after the game has ended, so all of the variables mentioned beforehand are viable for use in training the model."

## Baseline Model
For the baseline model, a random forest classifier was used with the following 4 features: `'void_grubs'`, `'dragons'`, `'barons'`, and `'inhibitors'`. All of these features are quantitative. 

We have already seen how taking more `'void_grubs'` is correlated with higher chances of winning games, along with increasing the values of other features. I believe that taking other epic monsters such as `'dragons'` and `'barons'` may also play a key role in determining a game's outcome. As such, it was decided to use these features to train the model. Taking `'inhibitors'` also plays a key role in winning games, as destroying at least one of them is required to destroy the enemy Nexus along with providing other benefits, so we will also include `'inhibitors'` in the training of the base model.

Because the `'result'` column already uses 0s to record losses and 1s to record wins, it is already in binary data form and thus we don't need to utilize one-hot encoding on it. No further encodings will need to be performed this section.

After fitting the model and seeing how well it performed on testing data, our model recieved an accuracy of 0.9646 and a F1 score of 0.9656. For losses, the F1 score was 0.96, and for wins, the F1 score was 0.97. These scores, all of which were very high and around the 0.96-0.97 range, indicate that not only was the model able to accurately classify nearly all of the data, but that it was also extremely fair, consistently and correctly classifying both losses and wins equally, which means that it would be fair to call this model very good. However, the model still has some improvement space that could make it even better than it is now.

## Final Model 
In my final model, we continued using the features used in the baseline model (`'void_grubs'`, `'dragons'`, `'barons'`, and `'inhibitors'`). However, 2 more features were engineered from smaller_df's base features and also incorporated into the model: `'kill_death_ratio'`, which was created by dividing `'teamkills'` by `'teamdeaths'` for games where `'teamdeaths'` is greater than 0 (to avoid divide by 0 errors), and `'structures_taken'`, which was created by adding  `'towers'` and `'inhibitors'`. Both of those features are quatitative.

`'kill_death_ratio'`:

In League, getting `'teamkills'` (killing enemy players, aka champions) is extremely important, as not only do players get immediate resources such as gold and experience from killing enemy champions, but because champions only resurrect (respawn) after a certain amount of time past their death, players can use this time to gain leads and take control of Summoners' Rift without enemy opposition. In contrast, teams want to avoid getting `'teamdeaths'` (friendly champion deaths to enemy champions), in order to avoid giving extra gold, exp, and map control to the enemy team and players. Because of how important getting `'teamkills'` and avoiding `'teamdeaths'` is in LoL, I believed that a metric that tracks the ratio of `'teamkills'` to `'teamdeaths'` would greatly contribute to improving the prediction power of the model. Teams with a high `'kill_death_ratio'` in games would correlate more with wins in those games, and teams with low `'kill_death_ratio'` in games would correlate more with losses in those games.  

`'structures_taken'`:

I already mentioned how important taking `'inhibitors'` are in League and how that feature would be good for use in prediction. However, taking `'towers'` (turrets) is also extremely important. `'towers'` shoot powerful projectiles at enemy players and minions, protecting each side's lanes and base. Destroying `'towers'` gives gold and extra control of the map to players and teams, and at least 5 enemy `'towers'` (all 3 defending one of the lanes, along with the 2 defending the Nexus) must be destroyed for a team to destroy the enemy Nexus and win that game. Because `'inhibitors'` and `'towers'` are both structures (in fact, besides the Nexuses, they are the only structures in LoL that can be destroyed), and taking both of them are extremely important for winning a game, I believed that a metric that tracks how many of them a team takes in a game would serve as a great feature to add to the model. Teams with higher `'structures_taken'` values would correlate more with wins in those games, and teams with lower `'structures_taken'` values would correlate more with losses in those games.

I chose to continue using a random forest classifier, as the many factors that determine a team's chances of winning or losing a given game often clash with one another, and so using a random forest classifier was seen as the optimal solution. Using GridSearchCV, I found some of the best hyperparameters for the RandomForestClassifier:

* classifier__class_weight: None
* classifier__max_depth: 20
* classifier__min_samples_split: 15
* classifier__n_estimators: 150

After fitting the final model and seeing how well it performed on testing data, our final model recieved an accuracy of 0.98430851 and a F1 score of 0.98438740. For losses, the F1 score was 0.98, and for wins, the F1 score was 0.98. Despite the base model already having high accuracy and F1 scores, the final model was still able to improve on it noticeably, showing slight improvements to every single previous performance metric.

## Fairness Analysis
This fairness analysis will first split the data into 2 groups: **high `'totalgold'`** and **low `'totalgold'`**. Games where the team being analyzed obtained `'totalgold'` values <= 58079 were classified as low `'totalgold'` games, and games where the team being analyzed obtained `'totalgold'` values > 58079 were classified as high `'totalgold'` games. I chose **58079** as the threshold value due to that being the median value of `'totalgold'`.

I chose **F1 score differences** as the evaluation metric, subtracting the F1 score for low `'totalgold'` predictions from those of high `'totalgold'` predictions. This is because want to see if the model evaluates low `'totalgold'` and high `'totalgold'` games equally.

The following statements are the null and alternative hypotheses, test statistic, and significance level:

**Null Hypothesis**: The model is fair. Its F1 scores for lower and higher `'totalgold'` games are roughly the same.

**Alternative Hypothesis**: The model is unfair. Its F1 score for higher `'totalgold'` games are lower than those for lower `'totalgold'` games.

**Test Statistic**: Difference in F1 score between games where a team earned a low `'totalgold'` value and games where a team earned a high `'totalgold'` value.

**Significance-Value**: 0.05.

Below is a graph showing the distribution of the test statistics:

<iframe
  src="assets/modelfairness.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

The observed difference in F1 scores was found to be -0.0162. The permutation test was done with 1000 trials. I got a p-value of 0, which was below the significance level of 0.05, so we reject the null hypothesis. The model does worse at classifying games where teams have high `'totalgold'` values compared to games where teams have low `'totalgold'` values.