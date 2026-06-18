# Spotify-artist-popularity-

# 🎤 What Makes an Artist Go Viral? A Spotify Data Analysis

![Python](https://img.shields.io/badge/Python-9B59B6?style=for-the-badge&logo=python&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-A569BD?style=for-the-badge&logo=tableau&logoColor=white)

## 📚 Table of Contents
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Data Cleaning](#data-cleaning)
- [Questions and Findings](#questions-and-findings)
- [Limitations](#limitations)
- [Business Recommendations](#business-recommendations)
- [Dashboard](#dashboard)

---

## Project Overview

Using Spotify data from 2010–2023, this project explores what factors are most associated with artist popularity — including follower count, audio features, and genre. The goal is to help artists and labels understand what drives virality on Spotify.

---

## Data Source

[Spotify Dataset 2023](https://www.kaggle.com/datasets/tonygordonjr/spotify-dataset-2023) via Kaggle

- **Raw dataset:** 375,141 rows · 49 columns
- **Columns used:** track_name, name, artist_popularity, followers, track_popularity, danceability, energy, tempo, acousticness, valence, explicit, genre_0, release_year, album_popularity

---

## Data Cleaning

```python
# Filter to 2010 and later
df_clean = df_clean[df_clean['release_year'] >= 2010]

# Remove karaoke tracks
df_clean = df_clean[df_clean['genre_0'] != 'karaoke']

# Remove tracks with 0 followers
df_clean = df_clean[df_clean['followers'] > 0]
```

#### Before vs After Cleaning:

| | Rows |
|---|---|
| **Before** | 375,141 |
| **After** | 273,349 |

![Before Cleaning](charts/before_cleaning.png)
![After Cleaning](charts/after_cleaning.png)

---

## Questions and Findings

**1. Does follower count correlate with artist popularity?**

```python
df_clean['log_followers'] = np.log1p(df_clean['followers'])

sns.scatterplot(data=df_clean.sample(5000),
                x='log_followers',
                y='artist_popularity')
```

#### Answer:
- There is a **strong positive correlation** between followers and artist popularity
- Artists with more followers consistently score higher on Spotify's popularity metric
- Growing an audience is the single biggest driver of popularity score

![Followers vs Popularity](https://github.com/kswl4/Spotify-artist-popularity-/blob/main/followers_vs_popularity.png)

---

**2. What audio features do popular songs share?**

```python
threshold = df_clean['track_popularity'].quantile(0.75)
df_clean['is_popular'] = df_clean['track_popularity'] >= threshold
```

#### Answer:
- **Energy** is the most distinguishing feature — popular tracks are significantly more high energy
- Danceability, valence, and tempo show smaller but consistent differences
- Acoustic tracks tend to be less popular overall

![Audio Features](charts/audio_features.png)

---

**3. Which genres produce the most popular artists?**

```python
genre_popularity = df_genres.groupby('genre_0')['artist_popularity'].mean()
genre_popularity = genre_popularity.sort_values(ascending=False).head(15)
```

#### Answer:
- **Pop, Hip Hop, and Reggaeton** produce the most popular artists on Spotify
- These three genres dominate the top of the popularity rankings
- Niche genres consistently score lower on average popularity

![Genre Popularity](https://github.com/kswl4/Spotify-artist-popularity-/blob/main/genre_popularity.png)

---

## Limitations

- **Popularity is a Spotify metric** — it's calculated by Spotify's algorithm and may not reflect real-world fame or cultural impact
- **Genre tags are imperfect** — many artists are tagged by nationality (e.g. "canadian hip hop") rather than actual genre
- **Correlation, not causation** — having more followers may be a result of popularity, not the cause
- **Snapshot data** — popularity scores change daily; this dataset reflects a single point in time in 2023

---

## Business Recommendations

**1. Artists: Focus on high energy production**
The data consistently shows that high energy tracks perform better. Artists looking to grow on Spotify should prioritize uptempo, energetic production over acoustic or low energy styles.

**2. Labels: Invest in Pop, Hip Hop, and Reggaeton**
These three genres produce the most popular artists by a wide margin. Labels looking to maximize streaming performance should focus their A&R efforts here.

**3. Artists: Prioritize follower growth over streams**
Follower count is the strongest predictor of popularity score. Artists should focus on converting listeners to followers through playlist placements, social media, and consistent releases.

---

## Dashboard

🔗 [View Live Tableau Dashboard](https://public.tableau.com/app/profile/kiasha.williams/viz/WhatmakesanartistGoViralSpotifyAnalysis/Dashboard1)
