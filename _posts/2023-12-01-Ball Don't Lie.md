---
layout: post
title:  "Ball Don't API: Scraping NBA Data Using the Ball Don't Lie API"
author: Daphne LeSueur
description: This post describes the process used to scrape the BallDon'tLie API to get NBA data
img: klay.jpg
--- 
My first API fetch all on my own :,) .  A pretty easy one to choose, given there's no sign up, no "freemium", and best of all, no API key. The [Ball Don't Lie API](https://www.balldontlie.io/home.html#introduction) is extremely user friendly, espeically for noobs like myself. 

I'm using this API for a school project and in this post I'm going to take you through the steps I took to collect and clean the data. This API contains information about every NBA player and game played since 1946, including live-ish updates every 10 minutes. The nature of this data is harmless; it's basically just a big pile of player stats. Regarding ethical considerations: For my purposes there is nothing unethical about collecting and analyzing this data. While I will be posting my findings in a post here, this project is relatively personal and a large part for my own enjoyment and interest. 

I accessed the Players, Teams, Stats, and Games endpoints and I will give a short description of the process for each of those here. The fifth endpoint available with this API is Season Averages, which contains some interesting metrics but I chose not to use for this project. I will be demonstrating my code using python. The first thing you'll need to do is import `requests` and `pandas`.

```python
import requests
import pandas as pd
```
Here's a [guide to using requests](https://realpython.com/python-requests/) if you're new to this like me, and [one for pandas](https://pandas.pydata.org/docs/user_guide/10min.html) as well. 
 
# Get Players

The players endpoint includes basic information about each player like name, position, team, height, and weight. The main reason I wanted to get this data was to create a table to be able to join player name and position to other tables (like the individual game stats). 

Like with every API, you start by specifying your URL.
```python
players_url = "https://www.balldontlie.io/api/v1/players"
```
Some people like to hard code the base url, then create a new variable as the endpoint and concatenate them like so
```python
base_url = "https://www.balldontlie.io/api/v1/"
players_endpoint = "players"
url = base_url + players_endpoint
```
but personally I prefer to just do it all at once and name a new variable for each endpoint I'm accessing. (I say I prefer like I am seasoned but remember, this is my first API pull all on my own! So my preferences may not be the best or most efficient for a lot of reasons.)

An important thing to know about this API is it will only give you 100 lines per "page". I wanted to get literally every page possible of players, and after some experimenting I found that this came out to 52 pages which means 52 separate pulls. I didn't want to do 52 pulls by hand and concatenate every single one, so I wrote a formula and used a little for loop to do this for me. I will paste the code here and then break it down. 

First and foremost let's initialize the dataframe.
```python
df = pd.DataFrame() # create an empty df
```

Now onto the big boy formula.
```python
def get_page(number):
    param = {"page" : number, "per_page" : 100} # define params based off of page number
    r=requests.get(players_url, params = param)
    # if the status is successful, we want to gather and clean the data
    if r.status_code == 200:
        # gather data
        data = r.json()
        df = pd.DataFrame(data['data'])
        # clean to extract data from within inner dictionary
        df['team_id'] = df['team'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
        df['team_abr'] = df['team'].apply(lambda x: x.get('abbreviation') if isinstance(x, dict) else None)
        df['city'] = df['team'].apply(lambda x: x.get('city') if isinstance(x, dict) else None)
        df = df.drop('team', axis=1)
        # now we want to save to a df
        players_df = players_df.concat(df, ignore_index = True)
    # if the status gives an error, we would like to know what the error is
    else: 
        return print(f'Status code error: {r.status_code}')
```
Hopefully the comments are documentation enough, but in case they aren't I'll go over the key elements of this formula. First you define the params for the page you are wanting to pull. The max number of lines per page is 100. Request the data. 
The line `players_df = pd.DataFrame(data['data'])` may be a little confusing. When you pull from this API, the results end up in a dictionary-like object with one key and value. The key is `'data'` and the value is all of the data that we are looking for so this must be extracted before it can be saved to a DataFrame.
Our if-statement takes us through a big list of commands as long as the status of the pull indicates a success. It will print the error code if it results in an error. 
The commands extract the json text from our pull and parse that into a DataFrame. 
The `team` column of this data results in a dictionary which is not ideal, so I wrote a few lines to extract the information I was interested in from that dictionary which were `id`, `abbreviation`, and `city`. Finally it drops the rest of that `team` column and joins the data into the existing DataFrame. Got it?

It is useful to try doing a single pull by hand first and then using the formula and a loop to pull everything at once. The loop I used looked something like this:
```python
for i in range(0, 51):
    get_page(i)
```
The simpler the better for a rookie like me. Thus concludes the extraction of the Players data!

# Get Teams
The teams endpoints is much simpler, and when you see just how simple you'll wonder why I didn't describe this one first. Weeding out those who are too weak I guess! With only 30 NBA teams, it only requires one pull which I managed to execute with four lines of code. And for my purposes, I didn't need to do any cleaning!
```python
url = "https://www.balldontlie.io/api/v1/teams"
r = requests.get(url)
data = r.json()
teams_df = pd.DataFrame(data['data'])
```

Here is an idea of what your `teams_df` will look. 
```python
teams_df.head(5)
```


|   | id | abbreviation | city | conference | division | full_name | name |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | 
| 0 | 1 | ATL | Atlanta | East | Southeast | Atlanta Hawks | Hawks |
| 1 | 2 | BOS | Boston | East | Atlantic | Boston Celtics | Celtics |
| 2 | 3 | BKN | Brooklyn | East | Atlantic | Brooklyn Nets | Nets |
| 3 | 4 | CHA | Charlotte | East | Southeast | Charlotte Hornets | Hornets |
| 4 | 5 | CHI | Chicago | East | Cnetral | Chicago Bulls | Bulls |


Easy enough right? Like really, really easy.

# Get Games
I wanted to get all the games from the 2022-23 season which ended up being a total of 1,420 games. With the same 100 lines per page constraint, this also requires multiple pulls which I accomplished with a function and a for loop. The function is relatively the same as the last function we wrote. 

```python 
games_url = "https://www.balldontlie.io/api/v1/games"
games_df = pd.DataFrame() # initialize empty data frame
def get_page_game(number):
    para = {"page" : number , "per_page" : 100, "seasons[]" : [2022]} # set parameters according to page number
    r = requests.get(games_url, params = para)
    r.status_code
    data = r.json()
    df = pd.DataFrame(data['data'])
    # clean to extract data from within inner dictionary
    df['home_team_id'] = df['home_team'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
    df['visitor_team_id'] = df['visitor_team'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
    df = df.drop(['home_team', 'visitor_team'], axis=1)
    # concat to the DataFrame
    games_df = games_df.concat(df, ignore_index = True))
for i in range(0, 15):
    get_page_game(i)
```
Here's everything all at once, starting with naming the URL, initializing the DataFrame, defining the function, and running it 16 times with a loop. Again, it would be useful to try this once outside of the function to step through the lines and understand what each does. After than that, loop away! Also, note the additional param of `"seasons[]"`. The season should be specified in an array contained in brackets, in case you want to get more than one season. Check out the documentation for this API for more available parameters. 

# Get Stats
Now, the big kahuna of API pulls. The Stats endpoint gives you each player's individual game stats for each game you specify in the parameters. After pulling this for every one of the 1,420 games in the 2022-23 season, I ended up with over 30,000 lines of data. You specify which game id you want stats for in the parameters, so I decided to pull the stats for each individual game id. First I needed to gather the ids in a list so I could loop through it later. 

```python
game_ids = games_df['id']
game_ids = game_ids.values.tolist()
```
Then I wrote the function to perform each of these pulls. Instead of concatenating the DataFrames inside the function, this time I returned a DataFrame to be concatenated in my loop.

```python
stats_url = "https://www.balldontlie.io/api/v1/stats"
def get_game_stats(gameid):
    para = {"game_ids[]" : gameid}
    r = requests.get(stats_url, params = para)
    data = r.json()
    game_df = pd.DataFrame(data['data'])
    game_df['game_id'] = game_df['game'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
    game_df['player_id'] = game_df['player'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
    game_df['team_id'] = game_df['team'].apply(lambda x: x.get('id') if isinstance(x, dict) else None)
    game_df = game_df.drop(['game', 'player', 'team'], axis = 1)
    return game_df
```

And finally, the loop to go through each one of the 1,420 games. An important note is that too many pulls results in a 429 error of "Too many requests." I had to separate each iteration of the loop with 1 second of time to avoid getting this error.
```python
for id in game_ids:
    game_stat = get_game_stats(id)
    all_stats = pd.concat([all_stats, game_stat], ignore_index=True)
    time.sleep(1)
```
This loop in total took around 30 minutes to run so be prepared to wait!

# Conclusion
The [Ball Don't Lie API](https://www.balldontlie.io/home.html#introduction) is very easy to use and produces LOTS of easily cleanable data. To see what I've done with this data, look out for my next post. 

