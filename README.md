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
The original dataframe is comprised of 116016 rows and 161 columns. Each row represents the game data of a participant, which can either a player on a team or an entire team, and each column stores an important game statistic or metric. For this project, only 13 columns will be needed:

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


