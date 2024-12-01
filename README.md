# League of Legends Esports Void Grubs Impact Analysis
A project studying the impact of securing void grubs in League of Legends esports games, conducted at UC San Diego.

Author: Andrew Li

## Introduction
### General Introduction
League of Legends (LoL) is a multiplayer online battle arena (MOBA) game created and developed by Riot Games, where two teams of five players fight each other on a massive battlefield called Summoner’s Rift and try to destroy opposite side’s enemy base (AKA the Nexus) using various champions. Released in 2009, it has since become one of the most successful and influential online competitive games of all time, having tens of millions of monthly players and boasting a thriving competitive scene. 

In LoL, there are “epic” monsters, which are neutral monsters located between the friendly and enemy jungles and which provide powerful buffs to whichever team kills them. In 2024, Riot introduced a new epic monster: the **void grubs**. These spawned, and could only be killed, during the early stages of games, and up to a total of 6 can be killed in a game. When killed, they give gold and experience, and more importantly, allow the team that killed them to deal extra damage over time (DOT) to enemy structures. Killing more than four void grubs also spawns voidmites whenever friendly teammates hit enemy structures, and these voidmites also deal damage to structures. The Nexus, along with the turrets and inhibitors that defend them, are all structures, and thus void grubs became an important objective to secure for teams.

### Central Question
The central question this project is interested in answering is the following: **How does securing the void grubs affect the game course and statistics of the team that secures them?** This project will use data analysis techniques to quantify and discover the relationship that securing void grubs has with various features, such as structures destroyed, gold differentials, kill differences and game outcomes. We will then use these statistics to create a prediction model that will predict the outcome of a game given said statistics. This model could be used to better understand win conditions in League games, helping teams to devise strategies to play at a higher level.

### Columns
The original dataframe is comprised of 116016 rows and 161 columns. Each row represents the game data of a participant, which can either a player on a team or an entire team, and each column stores an important game statistic or metric. For this project, 13 columns will be needed:

|Column                |Description|
|---                |---        |
|`'participantid'`                |Identifier of a participant representing their side, role, and whether they're a team or a player|
|`'gameid'`                |Unique identifier for each individual game played|
|`'side'`                |The side of the map the participant played on (Red or Blue side)|
|`'result'`                |The result the participant recieved during the game|
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

It was also necessary to include only the relevant columns: `'gameid'`, `'side'`,`'result'`, `'teamkills'`, `'teamdeaths'`, `'dragons'`, `'barons'`, `'void_grubs'`, `'towers'`, `'inhibitors'`, `'visionscore'`, and `'totalgold'`. All of these rows will be needed for future use and analysis. The dataset also includes a few games from 2023, and so it was necessary to remove those rows as those games weren't played during the 2024 season that this project is focused on.

It also turns out that the data set doesn't include `'void_grubs'` information for a few regions, due to the dataset obtaining the data for these regions by scraping certain websites that didn't incude the information regarding void grub secures. It was thus necessary to remove the rows where `'void_grubs'` has NaN value.

The data set we end up with contains 16012 rows and 12 columns, 11 of which hold information regarding the overall performance of a team in a game, and 1 of which (`'gameid'`) containing information on what specific game the information was from. Below is the head of the smaller_df dataframe:

| gameid           | side  |   result |   teamkills |   teamdeaths |   dragons |   barons |   void_grubs |   towers | inhibitors   |   visionscore | totalgold   |
|:-----------------|:------|---------:|------------:|-------------:|----------:|---------:|-------------:|---------:|:-------------|--------------:|:------------|
| LOLTMNT99_132542 | Blue  |        1 |          20 |            7 |       2.0 |      1.0 |          0.0 |      9.0 | 1.0          |           186 | 52523       |
| LOLTMNT99_132542 | Red   |        0 |           7 |           20 |       1.0 |      0.0 |          6.0 |      1.0 | 0.0          |           141 | 39782       |
| LOLTMNT99_132665 | Blue  |        1 |          31 |           20 |       2.0 |      1.0 |          4.0 |      8.0 | 1.0          |           251 | 72355       |
| LOLTMNT99_132665 | Red   |        0 |          20 |           31 |       3.0 |      1.0 |          2.0 |      8.0 | 1.0          |           251 | 66965       |
| LOLTMNT99_132755 | Blue  |        1 |          24 |            8 |       2.0 |      1.0 |          2.0 |      9.0 | 1.0          |           261 | 68226       |



