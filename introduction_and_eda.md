**Introduction and Description of Data**

## Description of Raw Data
## International Football Results from 1872
## Includes the scores for each match
## Type of Match - Friendly or Tournament match
 
## World Cup 2018 Dataset includes the following feature variables for each country participating in the World Cup
## Previous World Cup Appearances
## Previous Titles
## Previous Finals Appearances
## Previous Semi Final Appearances

**2. Feature Engineering**

In order to guide the process of our feature engineering for the FIFA prediction model, we conducted a preliminary exploratory data analysis on international results gathered from 1873 to 2018.
 
When analyzing the international results data, we looked at exploring what were the common types of international matches that were played between countries. As we can see in Figure 2, ‘Friendly’ matches seem to be the most common type of matches played, followed by the FIFA World Cup qualification. The FIFA World Cup matches only seem to the fifth most common matches that are played between countries.

--
Python Code

#Read in Data
results_data = pd.read_csv("international_results.csv")
results_data['date'] = pd.to_datetime(results_data['date'], yearfirst = True)

#Remove rows that included the FIFA 2018 results
result_data = results_data.loc[results_data['date'] < '2018-06-14']

#Create subset dataframe that includes only matches played between 2014 FIFA till 2018 FIFA
results_2014 = result_data.copy().loc[results_data.copy()['date'] > '2014-07-13']

#Create dataframe to get the total tournaments played since 2014
tournaments = results_2014.groupby('tournament')['date'].count()
tournaments = tournaments.to_frame()
tournaments.columns = ['counts']
tournaments = tournaments.sort_values(by = ['counts'], ascending = False)
tournaments = tournaments.head(10)
tournaments = tournaments.reset_index()

#Create barplot to show the which tournaments are most played
fig, ax = plt.subplots(1,1, figsize = (10,6))
sns.barplot(x = 'counts', y = 'tournament', data = tournaments, palette = 'Reds')
ax.set_xlabel("Number of Matches played", fontsize = 12)
ax.set_ylabel("Tournaments", fontsize = 12)
ax.set_title("Types of Tournament Matches played since 2014")




EDA
Difference between Home and Away Match Win Percentage
Methods for our initial data exploration involved classifying the matches played between teams to see if there was a difference when teams played at home compared to away. 
--
Python Code:

#Initializing new columns to create get results
results_2014['home_tie'] = 0
results_2014['home_win'] = 0
results_2014['home_loss'] = 0
results_2014['away_tie'] = 0
results_2014['away_win'] = 0
results_2014['away_loss'] = 0

#Setting condition for the created columns
results_2014.loc[results_2014['home_score'] == results_2014['away_score'], 'home_tie'] = 1
results_2014.loc[results_2014['home_score'] > results_2014['away_score'], 'home_win'] = 1
results_2014.loc[results_2014['home_score'] < results_2014['away_score'], 'home_loss'] = 1
results_2014.loc[results_2014['home_score'] == results_2014['away_score'], 'away_tie'] = 1
results_2014.loc[results_2014['home_score'] < results_2014['away_score'], 'away_win'] = 1
results_2014.loc[results_2014['home_score'] > results_2014['away_score'], 'away_loss'] = 1

#Creating new dataframe home_counts_2014 that gives us the number of matches played at home since 2014
home_counts_2014 = results_2014.groupby('home_team')['home_score'].count()
home_counts_2014 = home_counts_2014.to_frame()
home_counts_2014 = home_counts_2014.reindex()
home_counts_2014.columns= ['matches_played']

#Creating new dataframe away_counts_2014 that gives us the number of matches played at home since 2014
away_counts_2014 = results_2014.groupby('away_team')['away_score'].count()
away_counts_2014 = away_counts_2014.to_frame()
away_counts_2014 = away_counts_2014.reindex()
away_counts_2014.columns= ['matches_played']

#Creating a new data frame to get the grouped by values of wins, losses and ties for matches played at home SINCE 2014
home_total_2014 = results_2014.groupby(['home_team']).sum()
home_total_2014 = home_total_2014[['home_tie', 'home_win', 'home_loss']]
home_total_2014['home_win_percentage'] = home_total_2014['home_win'] / home_counts_2014['matches_played'] *100
home_total_2014['home_loss_percentage'] = home_total_2014['home_loss'] / home_counts_2014['matches_played'] *100
home_total_2014['home_tie_percentage'] = home_total_2014['home_tie'] / home_counts_2014['matches_played'] *100

#Creating a new data frame to get the grouped by values of wins, losses and ties for matches played away SINCE 2014
away_total_2014 = results_2014.groupby(['away_team']).sum()
away_total_2014= away_total_2014[['away_tie', 'away_win', 'away_loss']]
away_total_2014['away_win_percentage'] = away_total_2014['away_win'] / away_counts_2014['matches_played'] *100
away_total_2014['away_loss_percentage'] = away_total_2014['away_loss'] / away_counts_2014['matches_played'] *100
away_total_2014['away_tie_percentage'] = away_total_2014['away_tie'] / away_counts_2014['matches_played'] *100

#Selecting only countries that were in the world cup
home_total_2014 = home_total_2014.loc[home_total_2014.index.isin(wc_countries)]
away_total_2014 = away_total_2014.loc[away_total_2014.index.isin(wc_countries)]
home_counts_2014 = home_counts_2014.loc[home_counts_2014.index.isin(wc_countries)]
away_counts_2014 = away_counts_2014.loc[away_counts_2014.index.isin(wc_countries)]
home_counts_2014['Country'] = home_counts_2014.index
away_counts_2014['Country'] = away_counts_2014.index

#Creating a new dataframe wc_total_2014_2014 that includes the all the countries 
#SINCE 2014 in the world cup and their home and away win percentages

wc_total_2014 = pd.DataFrame(columns = ['Country', 'Home', 'Away'])
wc_total_2014['Country'] = home_total_2014.index
wc_total_2014['Home'] = home_total_2014['home_win'].values / home_counts_2014['matches_played'].values *100

wc_total_2014['Away'] = away_total_2014['away_win'].values / away_counts_2014['matches_played'].values *100
wc_total_wide_2014 = wc_total_2014
wc_total_2014 = pd.melt(wc_total_2014, id_vars = ['Country'], value_vars = ['Away', 'Home'])
