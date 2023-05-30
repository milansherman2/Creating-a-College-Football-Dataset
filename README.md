# Creating-a-College-Football-Dataset
Data wrangling with Python

<h2>College Football Dataset Creation</h2>
  
  <p> In this project, I identified, cleaned, and wrangled data from three different sources into a single dataset of College Football statistics by team and year. </p>
  
  Three data sources:
1.	College Football Offensive season statistics by team by season (flat file source)
2.	College Football Defensive season statistics by team by season
3.	College Football Final rankings by season
This dataset could be used to determine which offensive or defensive statistics are most predictive of winning the national championship or ending the season ranked in the Top 25.

<b>Flat file source:</b>
<p> https://www.kaggle.com/datasets/braydenrogowski/college-football-offensive-stats-20102020.  
This dataset has 10 years of College Football Offensive Statistics.  It has 1373 observations and 19 features, including team and year, which are the features that are common between the three datasets, although year was generated for the other data sources.</p>

<b>Data dictionary:</b>
- University name: name of school
- Year: which season the statistics are from
- TeamID: a concatenation of university name and year to make a unique identifier
- ATT: passing attempts
- COMP: number of completions
- YARDS: number of passing yards
- CMPPercent: completion percentage
- YPA: yards per attempt
- LNG: longest pass play of the season
- TD: number of passing touchdowns
- INT: number of interceptions
- SACK: number of sacks
- SYL: the total number of yards lost on sacks
- RTG: Passer rating
- R_ATT: number of rushing attempts
- R_AVG: average yards per rushing attempt
- TOTAL_PLAYS: total number of offensive plays during the season
- RUN_PERCENT: percent of plays that were running plays
- PASS_PERCENT: percent of plays that were passing plays

<b>URL for web scraping:</b>
<p>http://www.cfbstats.com/2021/leader/national/team/defense/split01/category10/sort01.html.  
This URL has College Football Defensive Statistics by team for a season. There are a total of 11 seasons available, 10 of which are in common with the flat file source.  This dataset has 130 observations and 9 features per year.  The challenge with this dataset was compiling the 10 years’ worth of data into a single dataframe, as the URL above only has season statistics for one year.  I looped through the URL to replace ‘2021’ with the other years, creating a list of 10 URLs,  downloaded the data for each and merged it into a single dataset.  I concatenated these 10 datasets into a single dataset, so each team is represented in the data 10 times, but with a different year and set of stats for each observation.  Since each year was a separate URL, I created a field for year to be able to join on team and year in the defensive statistics dataset.  
  
<b>Data dictionary:</b>
- Name: name of school/team
- G: number of games for the season
- Rush Yards: number of rushing yards allowed for the season
- Pass Yards: number of passing yards allowed for the season
- Plays: number of offensive plays by the opponent
- Total yards: total number of yards allowed for the season
- Yards/Play: average number of yards allowed per play
- Yards/G: average number of yards allowed per game
  
<b>API data source:</b>
<p>https://api.collegefootballdata.com/api/docs/?url=/api-docs.json#/rankings/getRankings.  This API has college football rankings by year and division (FBS, FCS, Division 2, and Division 3).  The defensive and offensive statistics data is only for FBS division schools, so I filtered to FBS schools when importing the data.  Also, the API queries the data by year, regular season or postseason, and week.  For example, there are generally about 16 weeks in the regular season, so the rankings on any given week are available.  However, I was only interested in the final rankings since the offensive and defensive statistics data are season level statistics, so I iterated on year to get all the years that I needed and added a field for year.  This data has five fields, and each year has 25 observations as the rankings generally only include the top 25 teams in the nation.  Finally, I left joined this data to the above merged data, as only 25 out of 130 teams will have a ranking for a given year (thus, ranking will be null for most teams in a given year).

  <b>Data dictionary:</b>
- Rank: the final ranking for the season
- School: the name of the school/team
- Conference: what conference the school/team is in
- FirstPlaceVotes: the number of first place votes that the team received in the poll that they’re in
- Points: total number of points from all votes (not just first place votes)
  
  <b>Creating a SQLite Database:</b>
Finally, I loaded the merged and cleaned dataset into a local SQLite database that can be queried. This dataset would be ideal for an analysis of college football teams to determine which statistics are most predictive of winning or losing, or for creating a predictive model based on these features.  For example, a few questions that could be asked are:
  - Is offensive or defensive ranking more predictive of final overall ranking?
  - Which statistics are most predictive of a team's defensive ranking?
  - Which statistics are most predictive of a team's offensive ranking?
  - Are the same stats/metrics predictive of a team performing poorly as of a team performing well?
  
  <b>Additional work:</b> 
Another feature that would be useful to include that is not part of this dataset is number of wins.  As a coach/team cannot control their ranking, the most immediate target is games won.  This would also allow for a deeper analysis of what goes into a team's ranking.  For example, analysis of teams with the same number of wins could reveal what is valued by voters, i.e., for teams with the same number of wins, which statistics are most correlated to a team's ranking?

