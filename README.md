# Creating-a-College-Football-Dataset
Data wrangling with Python

<h2>College Football Dataset Creation</h2>
  
  <p> In this project, I identified, cleaned, and wrangled data from three different sources into a single dataset of College Football statistics by team </p>
  
  The three sources that I identified are listed below:
1.	College Football Offensive season statistics by team by season (flat file source)
2.	College Football Defensive season statistics by team by season
3.	College Football Final rankings by season
This dataset could be used to determine which offensive or defensive statistics are most predictive of winning the national championship or ending the season ranked in the Top 25.

  <b>Flat file source:</b>
 https://www.kaggle.com/datasets/braydenrogowski/college-football-offensive-stats-20102020.  
This dataset has 10 years of College Football Offensive Statistics.  It has 1373 observations and 19 features, including team and year.  Those will be the features needed to tie this dataset to the other two (although year will need to be generated for the other data sources). One challenge with this dataset in terms of cleaning is that the column names are not the first row in the dataset, but the second.  The first row has information about the dataset (like which teams did not have data available for which years), so I’ll need to figure out how to remove that row and make the second row the column names.  Other than that, this is pretty clean dataset.  I expect most of the work in terms of data wrangling to be associated with the other two data sources.
Data dictionary:
•	University name: name of school
•	Year: which season the statistics are from
•	TeamID: a concatenation of university name and year to make a unique identifier
•	ATT: passing attempts
•	COMP: number of completions
•	YARDS: number of passing yards
•	CMPPercent: completion percentage
•	YPA: yards per attempt
•	LNG: longest pass play of the season
•	TD: number of passing touchdowns
•	INT: number of interceptions
•	SACK: number of sacks
•	SYL: the total number of yards lost on sacks
•	RTG: Passer rating
•	R_ATT: number of rushing attempts
•	R_AVG: average yards per rushing attempt
•	TOTAL_PLAYS: total number of offensive plays during the season
•	RUN_PERCENT: percent of plays that were running plays
•	PASS_PERCENT: percent of plays that were passing plays
URL for web scraping: http://www.cfbstats.com/2021/leader/national/team/defense/split01/category10/sort01.html.  This URL has College Football Defensive Statistics by team for a season. There are a total of 11 seasons available, 10 of which are in common with my flat file source.  This dataset has 130 observations and 9 features per year.  The challenge with this dataset will be compiling the 10 years’ worth of data into a single dataframe, as the URL above only has season statistics for one year.  I’ve played with it a bit and I’m able to loop through the URL to replace ‘2021’ with the other years I want and create a list of 10 URLs.  But I’ll need to figure out how to iterate through that list and download the data for each and merge it into a single dataset.  I plan to simply concatenate these 10 datasets into a single dataset, so each team will be represented in the data 10 times, but with a different year and set of stats for each observation.  I will also need to add a field for year, as it does not exist in the data since each web page displays the stats for a given year.  It appears that this is how the data in the flat file is structured, so I should be able to merge the two datasets on those two fields (school and year).
Data dictionary:
•	Name: name of school/team
•	G: number of games for the season
•	Rush Yards: number of rushing yards allowed for the season
•	Pass Yards: number of passing yards allowed for the season
•	Plays: number of offensive plays by the opponent
•	Total yards: total number of yards allowed for the season
•	Yards/Play: average number of yards allowed per play
•	Yards/G: average number of yards allowed per game
API: https://api.collegefootballdata.com/api/docs/?url=/api-docs.json#/rankings/getRankings.  This API has college football rankings by year and division (FBS, FCS, Division 2, and Division 3).  The flat file and web scraping data is only for FBS division schools, so I’ll need to figure out if I can filter to FBS schools before I download the data or if I’ll need to download everything and filter afterward.  Also, the API queries the data by year, regular season or postseason, and week.  For example, there are generally about 16 weeks in the regular season, so I could get the rankings on any given week.  However, I am only interested in the final rankings as the offensive and defensive statistics in the flat file and web scraping data are season level statistics.  It appears that I can get the final rankings by filtering to postseason week 1.  Similar to the web scraping data, however, I’ll need to iterate on year to get all the years that I need, and I’ll need to add a field for year as year is one of the query parameters and not present in the data.  This data has five fields, and each year will have 25 observations as the rankings generally only include the top 25 teams in the nation.  As a result, I will need to left join this data to dataframe formed by merging the above two datasets, as only 25 out of 130 teams will have a ranking for a given year.
Data Dictionary:
•	rank: the final ranking for the season
•	school: the name of the school/team
•	conference: what conference the school/team is in
•	firstPlaceVotes: the number of first place votes that the team received in the poll that they’re in
•	points: total number of points from all votes (not just first place votes)

