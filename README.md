

## Introduction
For our project, we chose to analyze League of Legends data from 2022 gathered from the Oracle Elixir website. The dataset we chose has 149400 rows and 123 columns detailing numerous match histories and contains information such as the league the matches belong to, the game ID, how many gold each team ended up with, what champions they chose to play, and how many turrets each team was able to destroy among many other features of the game. After looking at the dataset, we were interested in answering...

**Question:** Does getting first blood as a support player increase the duration of the game

We were curious about this question because there is a huge consensus in the League of Legends gaming community that the kills going to the support does not hold as much impact as the kills going to any other position on the team. Therefore, we wanted to explore whether or not the support getting the first kill on the team would cause the game to go on longer or not to either support or refute this theory and see whether or not public opinions were actually true or not despite there being such a huge agreement already in the community. To answer this question, we chose to look at the columns 'position', which contains information on what position everyone in the match is (bottom, top, middle, jungle, or support) as well as the columns 'gamelength', which is the duration of the games in seconds and also 'firstblood' that denotes whether or not a player got first blood or not, with 1 being True and 0 being False. In selecting these columns, we had enough information to answer our question and perform hypothesis testing because we could examine the relationship between position and game duration.

## Cleaning and EDA (Exploratory Data Analysis)
### Data Cleaning
To clean our data, we first isolated the columns that we needed: "position", "gamelength", and "firstblood". We then dropped all the rows that had NA in them so that they would not interfere with our analysis or calculations. In the dataset, there are rows that contain the summary statistics for the entire match, identified by the term "team" in the "position" column, so we decided to drop those rows as well since it did not contain the information we wanted, as we were looking for positions, game duration, and first blood for each person, not for the entire match as a whole. Afterwards, we converted the "firstblood" column from a float to a boolean column. This was because we thought it made sense for the column to be a boolean, as there was only 2 choices (True and False) of whether or not a player got first blood or not, and so we decided to convert it to a boolean instead of keeping it as a float column. Our cleaned Dataframe is below.

| position   |   gamelength | firstblood   |
|:-----------|-------------:|:-------------|
| top        |         1713 | False        |
| jng        |         1713 | True         |
| mid        |         1713 | False        |
| bot        |         1713 | True         |
| sup        |         1713 | True         |

### Univariate Analysis

<iframe src="assets/game_durations.html" width=800 height=600 frameBorder=0></iframe>

The majority of the games seem to last between 1400 to 2300 seconds. There seems to be slight skewness to the right due to games that last longer than 3100 seconds.

### Bivariate Analysis

<iframe src="assets/firstblood_by_position.html" width=800 height=600 frameBorder=0></iframe>

Jungle seems to obtain firstblood most often following support, while bottom, middle, and top obtain approximately the same number of firstbloods.

<iframe src="assets/wins_position.html" width=800 height=600 frameBorder=0></iframe>

For each position, a majority of wins are without obtaining firstblood. Jungle seems to have the most wins after obtaining firstblood.

### Interesting Aggregates

|   gamelength |   position |   firstblood |
|-------------:|-----------:|-------------:|
|          921 |         10 |           10 |
|          933 |         10 |           10 |
|          938 |         10 |           10 |
|          953 |         10 |           10 |
|          994 |         10 |           10 |

We grouped our cleaned dataset by gamelength and aggregated the columns (position and firstblood) by count to view the distribution of positions and firstblood. We noticed that for each game length there's exactly 10 positons and 10 firstbloods obtained. This gives us a better idea of how the data is structured and how there should be 10 reports of positions and 10 reports of  whether for not firstblood were obtained by each player for each game.

## Assessment of Missingness
### NMAR Analysis
In our dataset, we believe that the column "dragons (type unknown)" is NMAR (Not Missing At Random). This is because it seems from the data collection process that there is only values in there if there are dragons where the type is unknown. The fact that there are NA values there signifies the fact that for those rows that have NA values in them, there are no dragons in that match where the type is unknown, and so those values are NMAR because the absence of those values means something: that there are no unknown type dragons in the match. Additional information we would want to collect in order to explain the missingness is collecting a new column entitled "dragons known" where it would be a boolean column. In the column, it would have the values "True" if all the types of dragons in the match were known, and "False" if at least one type of dragon in the match was unknown. This would help us see if there is a relationship between that column and the "dragons (type unknown)" column.

### Missingness Dependency
In our analysis, we examined the relationships of several columns in relation to each other. One pair we decided to focus on was the relatinship of the column "datacompleteness" on the column "firstblood." Our hypothesis for this test was that the NA values in the "firstblood" column are not dependent on the values in the "datacompleteness" column. We used a permutation test to decide whether or not the NA values in the "firstblood" column were dependent on the values in the "datacompleteness" column (where the values were either "partial" or "complete"), and set the significance level to 0.05. As our test statistic for this permutation test, we used the absolute difference between the number of rows that had "complete" in the "datacompleteness" column and NA values in the "firstblood" column, and the number of rows that had "partial" in the "datacompleteness" column and NA values in the "firstblood" column. Then, for 500 iterations, we shuffled the "datacompleteness" column, randomly assigning rows with either the label "complete" or "partial", and then computed the absolute differences like we did in the test statistic. After we had our array of 500 computed statistics, we computed the p-value of the distribution by comparing the 500 computed statistics to our test statistic to see what proportion of those computed statistics were greater or equal to our original test statistic. We got 0.0 for our p-value, leading us to reject the null hypothesis. Thus, it seems that there was an effect on the values of the "datacompleteness" column on the values in the "firstblood" column.

PLOTLY

Another pair of columns we decided to focus on was the relationship of the column "side" on the column "firstblood." Our hypothesis for this test was that the NA values in the "firstblood" column are not dependent on the NA values in the "side" column. We again used a permutation test to decide whether or not the NA values in the "firstblood" column were dependent on the values in the "side" column. As our test statistic for the permutation test, we used the absolute difference between the number of rows that had no NA values in the "side" column and NA values in the "firstblood" column, and the number of rows that had NA values in the "side" column and NA values in the "firstblood" column, and set the significance value to 0.05. Then, for 500 iterations, we shuffled the "side" column, randomly assigning rows with the values in the "side" column, and then we computed the absolute differences like we did in the test statistic. After we had our array of 500 computed statistics, we computed the p-value of the distribution by comparing the 500 computed statistics to our test statistic to see what proportion of those computed statistics were greater or equal to our original test statistic. We got 1.0 for our p-value, leading us to fail to reject the null hypothesis. Thus, we cannot say for certain that the NA values in the "firstblood" column are dependent on the NA values in the "side" column, and thus they are not dependent on each other as the null hypothesis stated.

## Hypothesis Testing
Our approach to the question: Does getting first blood as a support player increase the duration of the game? is to use a permutation test to see whether or not there is an effect of getting first blood as a support player on game duration. The null hypothesis for this test is that getting first blood as a support player does not increase the duration of the game, and the alternative hypothesis is that getting first blood as a support player does increase the duration of the game. For this test, we set a significance level of p = 0.05 , as it seemed a good enough p-value to balance out the chances of making a Type I or Type II error (we would be able to see the real effect while also balancing out the probability that we mistake no effect for a real effect). For our test statistic, we found the mean game duration length in seconds of all the matches where the support got the first blood. Then, for 500 iterations, we shuffled the columns of “position” - randomly assigning position labels and then taking the mean of the game duration of all the matches where the support got first blood. This was so that we could construct a distribution of samples that would occur under the null hypothesis of there not being an effect of first blood as a support player on mean game duration, and compare it to the test statistic and see whether or not it was a reasonable value to get under the null or not. We then computed the p-value by calculating the proportion of values where the computed sample statistic was greater or equal to the test statistic. We got a p-value of 0.814, which led us to fail to reject the null hypothesis, and so no effect of getting first blood as a support player on game duration was concluded. Thus, we conclude that we cannot say that there is a relationship of getting first blood as a support player on the increase of mean game duration.

PLOTLY


