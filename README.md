# ETL-Project

** YOU WILL NEED A GKEY AS WELL AS THE MONGO USER AND PASSWORD ***

The goal of this project was to find the NHL faceoff wins by player then find some more information about the city they play in.

The final output will show the player name, team they play on, the city they play for, the arena they play their home games in, the lat and lang for the city, and the closest restaurant in that city. This way if you see 'Patrice Bergeron' wins the most face offs, you may want to go see him play live! With this you will know what team he plays for, where he plays, and when you go watch him, where can you grab a bite to eat entering the city.

Everything starts in the abbrevname jupyter notebook.
    . I scraped http://www.sportsforecaster.com/nhl/abbreviations to find the table
    . looked through and found the table i needed.
    . The goal here was to find the shortened character set for each team
    . city_abbrev hold the final df that lists the teams and their abbreviation

From there we move to the faceoff__ jupyter notebook.
    . kaggle was used to find the csv
    . https://www.kaggle.com/garyrogers/nhl20192020faceoffdata
    . stored the csv in a variable called faceoff_data_df
    . just got the columns needed.. player, team and win (if they won or lost the faceoff)
    . pulled out only the instances they won
    . grouped it by player and team to get the total amount of faceoff wins per player
    . sorted it so most wins were on top and took the ones that had 30 or more wins
    .*FROM HERE I IMPORT THE city_abbrev TABLE from abbrevname
    . Merge those two df's on the abbreviation

From here we go to the Wiki_table jupyter notebook
    . I was scraping it then realized through inspect that it was a table so pulled the table
    . pulled from wikipedia
    . https://en.wikipedia.org/wiki/List_of_National_Hockey_League_arenas
    . pulled the table i needed
    . split the team(s) column so it would split it word for word and made it a list and turned into a df to minipulate
    . A problem i faced was some teams are 2 words and some are 3; example. "Detroit Red Wings" and "Colorado Avalanche" so I had to put the teams with 3 words into its own dataframe to be called later
    I pulled another table out that consisted of the arena names so I pulled that to be used later

Now its time for the ETL_final jupyter notebook
    . I pulled the team_info df from faceoff__ notebook
    . I then pulled team_name, team_arena, team_name_ df's from Wiki_table notebook
    . the goal with bringing in all these tables was to join them so we could see the player, arena, team, abbreviation, wins, and city so I could make API calls.
    . Once they were all joined together to make the PTA_df (PTA stands for player,team,arena) i could pull the cities out into its own list/df
    . once the city_df__ was made I could do a google API search to find the lat and lang of the city
    . did a for loop to find the lat and lang and put it in a list of dictionaries (city_ltlg)
    . turned that list into a df and dropped duplicates to merge with the PTA_df
    . with those merged to make the PTALL_df (PTALL stands for player,team,arena,lat,lang) i could then do another google API call to find the closest restaurant based off the lat/lang gatherer off the last one.
    . turned it into a list of dictionaries (restaurant_name) then made it into a df, dropped duplicates to merge with the PTALL_df
    . The final table is the final_df dataframe

from there it was just putting it into mongo!
    db = faceoff_data
    colhockey
