# Fifa-Project
This project uses Python to clean up and analyze a FIFA/EA Sports FC player dataset. Instead of wading through a messy spreadsheet with hundreds of extra columns, this script focuses on the stats that matter, sets up a quick player lookup tool, and plots out comparisons for ratings and speed, young players, best players, and favorite players. 

---
title: "Untitled"
format: html
---


```{python}


import pandas as pd
import matplotlib.pyplot as plt


# STEP 1: Load the Dataset

# Load the dataset using pandas
df = pd.read_csv('~/Downloads/archive/male_players.csv', low_memory=False)

print("Dataset loaded successfully!")

```


```{python}

# STEP 2: Dataset Summary & Overview

print("--- FIFA PLAYER STATS SUMMARY ---")
print(f"Total players in dataset: {len(df)}")
print(f"Average player overall rating: {df['overall'].mean():.1f}")
print(f"Average player age: {df['age'].mean():.1f}")

# Find top 5 highest rated players
print("\n--- TOP 5 HIGHEST RATED PLAYERS ---")
top_5 = df[['short_name', 'overall', 'club_name', 'age']].sort_values(by='overall', ascending=False).drop_duplicates(subset=['short_name']).head(5)
print(top_5)
```


```{python}

# STEP 3: Simple Player Lookup Function

def check_player(name):
    """Finds a player by name and prints their basic stats."""
    player = df[df['short_name'].str.contains(name, case=False, na=False)]
    print(player[['short_name', 'overall', 'club_name', 'pace', 'shooting']].head(1))

# Testing the lookup function
print("--- TESTING PLAYER LOOKUP FUNCTION ---")
check_player("Mbappé")
check_player("Saka")
```


```{python}

# STEP 4: Favorite Players Comparison

# List of favorite players
favorites = ['K. Mbappé', 'J. Bellingham', 'B. Saka', 'M. Olise']

# Filter dataset for favorite players
fav_df = df[df['short_name'].isin(favorites)].drop_duplicates(subset=['short_name'])

print("--- FAVORITE PLAYERS COMPARISON ---")
print(fav_df[['short_name', 'overall', 'club_name', 'pace', 'shooting']])

# Visual 1: Favorite Players Bar Chart
plt.figure(figsize=(7, 4))
plt.bar(fav_df['short_name'], fav_df['overall'], color='dodgerblue')
plt.title('Favorite Players Overall Rating')
plt.xlabel('Player')
plt.ylabel('Overall Rating')
plt.ylim(0, 100)
plt.show()
```


```{python}

# STEP 5: Speed players (Top 5 Fastest Players)

# Find top 5 fastest players based on Pace rating
fastest = df[['short_name', 'pace', 'club_name']].sort_values(by='pace', ascending=False).drop_duplicates(subset=['short_name']).head(5)

print("--- TOP 5 FASTEST PLAYERS ---")
print(fastest)

# Visual 2: Speed Chart
plt.figure(figsize=(7, 4))
plt.barh(fastest['short_name'], fastest['pace'], color='orange')
plt.title('Top 5 Speed Demons (Pace Rating)')
plt.xlabel('Pace Rating')
plt.xlim(80, 100)  
plt.gca().invert_yaxis()  
plt.show()
```


```{python}

# STEP 6: Wonderkids (Young Players with Highest Potential)

# Filter for players aged 20 or younger and find top potential ratings
young_gems = df[df['age'] <= 20][['short_name', 'age', 'overall', 'potential', 'club_name']]
young_gems = young_gems.sort_values(by='potential', ascending=False).drop_duplicates(subset=['short_name']).head(5)

print(" TOP 5 YOUNG WONDERKIDS ")
print(young_gems)
```

```{python}

# STEP 7: Most Valuable Players (Market Value in Millions)

# Find top 5 players with the highest market value
valuable = df[['short_name', 'value_eur', 'club_name']].sort_values(by='value_eur', ascending=False).drop_duplicates(subset=['short_name']).head(5)

# Convert value to Millions of Euros for easier reading
valuable['value_in_millions'] = valuable['value_eur'] / 1000000

print("--- TOP 5 MOST VALUABLE PLAYERS ---")
print(valuable[['short_name', 'value_in_millions', 'club_name']])

# Visual 3: Value Chart
plt.figure(figsize=(7, 4))
plt.bar(valuable['short_name'], valuable['value_in_millions'], color='mediumseagreen')
plt.title('Top 5 Most Valuable Players (€ Millions)')
plt.xlabel('Player')
plt.ylabel('Market Value (€ Millions)')
plt.show()
```
