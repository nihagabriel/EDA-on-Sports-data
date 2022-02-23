# EDA-on-Sports-data
Find out the Most successfu team , player and Factors contributing win or loss
#Exploratory Data Analysis of IPL

#Objectives:

##
#    To find the team that won the most number of matches in a season.
 #   To find the team that lost the most number of matches in a season.
  #  Does winning toss increases the chances of victory.
   # To find the player with the most player of the match awards.
  #  #To find the city that hosted the maximum number of IPL matches.
   # To find the most winning team for each season.
   # To find the on-field umpire with the maximum number of IPL matches.

import os
import pandas as pd
ipl_matches_df = pd.read_csv('E:/sweety.doc/SPARK foundation/matches.csv')
ipl_matches_df
ipl_matches_df.info()
ipl_matches_df.describe()

#Observations

#The following inferences can be made from the describe() method:

 #   The .csv file has data of IPL matches starting from the season 2008 to 2019.
  #  The biggest margin of victory for the team batting first(win_by_runs) is 146 runs.
  # # The biggest victory of the team batting second(win_by_wickets) is by 10 wickets.
  # # 75% of the victorious teams that bat first won by a margin of 19 runs.
   # 75% of the victorious teams that bat second won by a margin of 6 wickets.
   # There were 756 IPL matches hosted from 2008 to 2019.

ipl_matches_df.columns

# Let's view the unique values of each column to help us understand the dataset better.

for col in ipl_matches_df:
    print(ipl_matches_df[col].unique())
    
    #the first index that doesn't contain a NaN value 
ipl_matches_df.umpire3.first_valid_index()
ipl_matches_df.isnull().sum()
#The column 'umpire3' has a significant number of NaN values. As the off-field umpire parameter is insignificant, we can drop this column. The other columns that have Nan values are of type object(Pandas equivalent of Python String data type) and are very few in number(<=7).

ipl_matches_df = ipl_matches_df.drop(columns=['umpire3'], axis=1)

#Ensure that the DataFrame is devoid of the 'umpire3' column.

#Exploratory Analysis and Visualization

#Now that our data set has been cleaned up, it's time to do the in-depth analysis and visualization.

#Let's begin by importingmatplotlib.pyplot and seaborn.

import seaborn as sns
import matplotlib
import matplotlib.pyplot as plt
%matplotlib inline

# sns.set_style('darkgrid')
# sns.set_style('whitegrid')
sns.color_palette("Paired")
matplotlib.rcParams['font.size'] = 14
matplotlib.rcParams['figure.figsize'] = (12, 8)
matplotlib.rcParams['figure.facecolor'] = '#00000000'

#The team with most number of wins per season.

teams_per_season = ipl_matches_df.groupby('season')['winner'].value_counts()
teams_per_season

year = 2008
win_per_season_df = pd.DataFrame(columns=['year', 'team', 'wins'])
for items in teams_per_season.iteritems():    
    if items[0][0]==year:
        print(items)
        win_series = pd.DataFrame({
            'year': [items[0][0]],
            'team': [items[0][1]],
            'wins': [items[1]]
        })
        win_per_season_df = win_per_season_df.append(win_series)
        year += 1   
        
win_per_season_df

#The number of wins is a discrete value. Hence, we will plot a barchart(barplot in Seaborn).

sns.barplot('wins', 'team', hue='year', data=win_per_season_df, palette='Paired');

#From the barplot, we can easily make inferences regarding the year in which a particular team has scored the maximum wins(and also the number of wins).
#Observations

    #Mumbai Indians has secured most wins in four seasons(2010, 2013, 2017 and 2019)..

#The venue that hosted the maximum number of matches
venue_ser = ipl_matches_df['venue'].value_counts()
venue_df = pd.DataFrame(columns=['venue', 'matches'])
for items in venue_ser.iteritems():
    temp_df = pd.DataFrame({
        'venue':[items[0]],
        'matches':[items[1]]
    })
    venue_df = venue_df.append(temp_df, ignore_index=True)
plt.title("IPL Venues")
sns.barplot(x='matches', y='venue', data=venue_df);

venue_df

#Observations

#    Eden Gardens has hosted the maximum number of IPL matches followed by Wankhede Stadium and M Chinnaswamy Stadium.
 #   Till 2019, IPL matches were hosted by 40 venues.

#The most successful IPL team

#In a game of sports, every team compete for victory. Hence, the team that has registered the most number of victories is the most successful.

team_wins_ser = ipl_matches_df['winner'].value_counts()

team_wins_df = pd.DataFrame(columns=["team", "wins"])
for items in team_wins_ser.iteritems():
    temp_df1 = pd.DataFrame({
        'team':[items[0]],
        'wins':[items[1]]
    })
    team_wins_df = team_wins_df.append(temp_df1, ignore_index=True)

team_wins_df

plt.title("Total Victories of IPL Teams")
sns.barplot(x='wins', y='team', data=team_wins_df, palette='Paired');

#Observations:

   # Mumbai Indians is the most successful team(as they have won the maximum number of IPL matches -109) followed by Chennai Super Kings and Kolkata Knight Riders.

#Most Valuable Player

#Winning matters the most in a competitive league match. If a player makes the most significant contribution for his team's victory, then he's chosen as the player_of_match.The player who has won the player_of_match title on most occasions is obviously the most valuable player.

mvp_ser = ipl_matches_df['player_of_match'].value_counts()

mvp_ten_df = pd.DataFrame(columns=["player", "wins"])
count = 0
for items in mvp_ser.iteritems():
    if count>9:
        break
    else:
        temp_df2 = pd.DataFrame({
            'player':[items[0]],
            'wins':[items[1]]
        })
        mvp_ten_df = mvp_ten_df.append(temp_df2, ignore_index=True)
        count += 1    

mvp_ten_df

plt.title("Top Ten IPL Players")
sns.barplot(x='wins', y='player', data=mvp_ten_df, palette='Paired');

#Observations:

   # Cris Gayle is the player who won the most player of the match awards and hence is the most valuable player.
    #Six indian players have figured in the top ten IPL players list.

#Team that won the most number of toss

toss_ser = ipl_matches_df['toss_winner'].value_counts()

toss_df = pd.DataFrame(columns=["team", "wins"])


plt.title("How IPL Teams fared in toss?")
sns.barplot(x='wins', y='team', data=toss_df, palette='Paired');

Notebook Image
#Observations:

   # Mumbai Indians has won the most toss(till 2019) in IPL history.
   # All the top teams in IPL are successful in winning toss as well.

#Q1:Does the presence of most valuable players in a team ensures IPL trophy?
#Q1:Does the presence of most valuable players in a team ensures IPL trophy?

#We have figured out the top ten players of IPL using 'Player of the Match' title as the yardstick. It is interesting to note that the top two players - Chris Gayle and AB de Villers have never won an IPL. Of the top ten players, 6 players(RG Sharma, MS Dhoni, DA Warner, SR Watson, SK Raina and G Gambhir) have won the IPL. It exemplifies the importance of the presence of most valuable player(s) in a team.

mvp_ten_df

#Q2: Which umpire has officiated the most number of IPL matches on-field?

umpire1_ser = ipl_matches_df['umpire1'].value_counts()
umpire2_ser = ipl_matches_df['umpire2'].value_counts()

umpires_df = pd.concat([umpire1_ser, umpire2_ser], axis=1)
umpires_df

umpire_ser = umpires_df.sum(axis=1)

umpire_df = pd.DataFrame(columns=["umpire", "matches"])

for items in umpire_ser.iteritems():
    temp_df4 = pd.DataFrame({
        'umpire':[items[0]],
        'matches':[items[1]]
    })
    umpire_df= umpire_df.append(temp_df4, ignore_index=True) 

umpire_df.sort_values('matches', ascending=False).head()

#S. Ravi(Sundaram Ravi) has officiated the most number of IPL matches on-field, followed by former SriLankan international cricketer HDPK Dharmasena.
#Q3: Which team is the most successful team in IPL?

#Nothing succeeds like success. In a game of cricket, winning is everything. We have narrowed down on the list of teams that made most number of wins in each season. The DataFrame win_per_season_df gives the required information. Mumbai Indians has secured most wins in four seasons(2010, 2013, 2017 and 2019).

win_per_season_df

#Mumbai Indians secured the most number of wins(109) in IPL, followed by Chennai Super Kings.

team_wins_df

#Of all the IPL matches played till 2019, Mumbai Indians has emerged victorious in the most number of games. They have secured most wins in four seasons. Hence Mumbai Indians is the most successful team in IPL.
#Q4: Which city has hosted the maximum number of IPL matches?

ipl_matches_df['city'].value_counts()

city_ser = ipl_matches_df['city'].value_counts()

city_df = pd.DataFrame(columns=['city', 'matches'])
for items in city_ser.iteritems():
    temp_df6 = pd.DataFrame({
        'city':[items[0]],
        'matches':[items[1]]
    })
    city_df = city_df.append(temp_df6, ignore_index=True)

plt.title("Cities that hosted IPL matches")
sns.barplot(x='matches', y='city', data=city_df);

#Mumbai has hosted the maximum number of IPL matches.
#Q5: Does winning toss has any advantage?

win_count = 0
for index, value in ipl_matches_df.iterrows():
    if(value['toss_winner']==value['winner']):
        # print(value['winner'])
        win_count += 1

print(f'The number of times the team winning toss have won: {win_count}')
prob = win_count/len(ipl_matches_df)
print('The probability of winning if won the toss: {:.2f}' .format(prob))    

#The probability of winning when the team had won the toss is 52%. So winning toss gives a slight edge over the opponent. However it would be naive to term winning the toss as a greater advantage as there were 363 instances when the team lossing the toss has won the game.


#Inferences and Conclusion

#Let’s summarize the important observations we made during Exploratory Data Analysis:

   # Mumbai Indians is the most successful team in IPL.
    #Mumbai Indians has won the most number of toss.
    T#he Mumbai city has hosted the most number of IPL matches.
    C#hris Gayle has won the maximum number of player of the match title.
    W#inning toss gives a slight edge(52% probability of winning) against the opponents.
    F#ive indian players have figured in the top ten IPL players list.
    #. Ravi(Sundaram Ravi) has officiated the most number of IPL matches on-field.
  #  Eden Gardens has hosted the maximum number of IPL matches.
   # Till 2019, 40 venues have hosted 756 IPL matches.

