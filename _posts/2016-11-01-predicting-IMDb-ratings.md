---
tags:
  - DSI
  - data-science
  - project
  - python
  - regression
  - classification
  - imdb
  - web-scraping
---
What affects the rating of a movie on IMDb?

{% include toc %}

## Introduction
This week we're working with movie data! We'll be getting information from [IMDb](http://www.imdb.com/){:target="_blank"} with some help from the [OMDb API](https://www.omdbapi.com/){:target="_blank"}.

> IMDb is now the world’s most popular and authoritative source for movie, TV and celebrity content. We offer a searchable database of more than 185 million data items including more than 3.5 million movies, TV and entertainment programs and 7 million cast and crew members. (source: [IMDb](http://www.imdb.com/help/show_leaf?about&ref_=hlp_brws){:target="_blank"})

We'll be looking at IMDb ratings and what are some of the things that affect a movie's ratings. Brainstorming before we actually obtain the data, some possible factors may be:

- Who stars in the movie
- Who directed the movie
- Length of the movie
- What genre(s) is the movie

## TL;DR
We got web-scraped data from IMDb with some help from OMDb API, transformed some of the words into features, and tried to predict actual ratings (failed) and/ or predict whether ratings will be higher than 8.5 (didn't do too badly). We found that the director of a movie has a pretty big impact on whether ratings will be higher than 8.5 on IMDb.

## About the data
### How we obtained our data
Hello, BeautifulSoup! No, I didn't use much of it this time around. (I tried, but everything took too long to run. Maybe I'll try it again when I have more time.)

We had a lot of guidance for obtaining the data this week in our labs. Each movie's page on IMDb can be accessed by a unique IMDB ID that starts with "tt" followed by 9 numbers. Using a combination of the `requests` and `re` libraries, we pulled the movie IDs of the [top 250 rated movies](http://www.imdb.com/chart/top){:target="_blank"}.

```python
r2 = requests.get("http://www.imdb.com/chart/top")
id_list = re.findall("tt[0-9]{7,8}", r2.content)

# set removes duplicates
# change it back to a list so we can iterate through it later
id_list = list(set(id_list))
```

Now that we have our IDs, we used the [OMDb API](https://www.omdbapi.com/){:target="_blank"} to pull some basic data of each movie.

```python
# OMDb API URL
api_url = "http://www.omdbapi.com/?i={}&plot=full&r=json"
```
```python
# Create a function to get the data for each movie
# into a format we can work with
def get_content(id_num):
    r = requests.get(api_url.format(id_num))
    tmp = json.loads(r.text)
    return tmp
```
```python
# Populate our data
df = pd.DataFrame([get_content(i) for i in id_list])
```

Here's the data we pulled using the API.

[![df_api]({{ site.url }}{{ site.baseurl }}/images/imdb/df_api.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/df_api.png)

We were then asked to pull data on gross earnings of each movie as well. (Here's where I used `BeautifulSoup`.)

```python
# Create a function to pull Gross Earnings information
# and return np.nan if it's not available
def get_gross(id_num):
    r = requests.get("http://www.imdb.com/title/{}/".format(id_num))
    soup = bs4.BeautifulSoup(r.text, "lxml")
    try:
        for i in soup.findAll("div", class_="txt-block"):
            for j in i.findAll("h4", class_="inline"):
                if "Gross" in j.text:
                    text = j.parent.text.split()[1]
                    num = text.replace(",", "").strip("$")
                    return float(num)
    except:
        return np.nan
```

```python
# Create a column in the dataframe for Gross Earnings
df["Gross_earnings"] = df["imdbID"].map(get_gross)
```

### Description of the data (data dictionary)

|Column |Data type|Description
|---|---|---
|Actors|String|List of top-billed actors
|Awards|String|List of awards won or nominations
|Country|String|Countries where the movie was shown
|Director|String|Director(s) of the movie
|Genre|String|Genre(s) of the movie
|Language |String|Language(s) the movie is available in
|Metascore|String|Score from [metacritic.com](http://www.metacritic.com/){:target="_blank"}
|Plot     |String|Summary of the movie
|Poster |String|URL for the movie poster
|Rated   |String|Viewer advisory rating for the movie
|Released|String|Release date for the movie
|Runtime|String|Length of the movie in minutes
|Title|String|Movie title
|Writer|String|Writers for the movie
|Year|String|Year in which the movie was released
|imdbID|String|IMDb ID of the movie
|imdbRating|String|IMDb rating of the movie
|imdbVotes|String|Number of votes received
|Gross_earnings|Float|Gross earnings of the movie

## Data munging/ data wrangling
As you can see, most of our columns are strings, which is not something we can use in our models (not that I'm aware of at least), so we'll have to clean and work on our data so that everything is numerical.

Columns like `Year`, `imdbRating` etc. should have been either integers or floats, and we converted them using `df["col_name"] = df["col_name"].astype(float)`.

We also want to convert some of the text columns to dummies so that we can use them in our model.

- Oscars won
```python
def oscars_won(i):
    try:
        i_list = i.split()
        if "Oscars." in i.split() and i_list[i_list.index("Oscars.")-2] == "Won":
            return float(i_list[i_list.index("Oscars.")-1])
        else:
            return 0
    except:
        return 0
df2["Oscars_won"] = df2["Awards"].map(oscars_won)
```

- Languages, countries, actors, directors
```python
# Let"s consider the language the movie is available in
# For model simplicity, we'll consider just the top 5 languages
all_languages = []
for i in df2["Language"]:
    lang_list = str(i).split(",")
    all_languages.extend([j.strip() for j in lang_list])
top_5_languages = [i[0] for i in Counter(all_languages).most_common(5)]
for i in top_5_languages:
    df2["Language_"+i] = df2["Language"].map(lambda x: 1 if i in str(x) else 0)
```

Using `TfidfVectorizer`, we also generated word features from our plot summaries.
```python
tfidf = feature_extraction.text.TfidfVectorizer(stop_words="english", ngram_range=(1,1), max_features=1000)
plot_df = pd.DataFrame(tfidf.fit_transform(df6["Plot"]).todense(), columns=tfidf.get_feature_names())
```

## Visualizations
Before we get to modeling, we'll take a look at the relationships within out data.

[![pairplots]({{ site.url }}{{ site.baseurl }}/images/imdb/pairplots.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/pairplots.png)

[![heatmap]({{ site.url }}{{ site.baseurl }}/images/imdb/heatmap.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/heatmap.png)

[![genre_rating]({{ site.url }}{{ site.baseurl }}/images/imdb/genre_rating.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/genre_rating.png)

[![actor_rating]({{ site.url }}{{ site.baseurl }}/images/imdb/actor_rating.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/actor_rating.png)

[![language_rating]({{ site.url }}{{ site.baseurl }}/images/imdb/language_rating.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/language_rating.png)

[![country_rating]({{ site.url }}{{ site.baseurl }}/images/imdb/country_rating.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/country_rating.png)

[![director_rating]({{ site.url }}{{ site.baseurl }}/images/imdb/director_rating.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/director_rating.png)

## Modeling: generating the model and drawing conclusions
So I tried both regression (predicting actual ratings) and classification (predicting whether a movie will have a rating higher than 8.5) with this dataset.

```python
# Regression model
gs3 = model_selection.GridSearchCV(ensemble.GradientBoostingRegressor(random_state=1),
                                  {"n_estimators": np.arange(50,100,10)},
                                  cv=5)
gs3.fit(X_train,yr_train)
```

[![regression]({{ site.url }}{{ site.baseurl }}/images/imdb/regression.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/regression.png)

```python

# Classification model
gs4 = model_selection.GridSearchCV(ensemble.GradientBoostingClassifier(random_state=1),
                                  {"n_estimators": np.arange(50,100,10),
                                   "min_samples_split": np.arange(5,10,1),
                                   "min_samples_leaf": np.arange(5,10,1)},
                                  cv=5)
gs4.fit(X_train,yc_train)
```

[![roc_curve]({{ site.url }}{{ site.baseurl }}/images/imdb/roc_curve.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/roc_curve.png)

With each model we ran our score functions, and regression was terrible. Classification did quite well with an accuracy of about 80%.

We then looked at our features using `RFECV` in `sklearn`.

```python
select = feature_selection.RFECV(gbc2,cv=5)
select.fit(X_train, yc_train)
X_train_new = select.transform(X_train)
```

We got a slight improvement in our score after feature selection:

[![roc_curve_selected]({{ site.url }}{{ site.baseurl }}/images/imdb/roc_curve_selected.png)]({{ site.url }}{{ site.baseurl }}/images/imdb/roc_curve_selected.png)

One of the things that I noted (without the recursive feature elimination) was that:

- Actors don't matter
- Plot summary doesn't matter
- DIRECTORS MATTER!

I happened to run my models a few times (without text features, with actors and plot summary, and with directors) and the only improvement in scores came when directors were added in.

## Next Steps
I wish we could have trained our model on a bigger and more varied training set. (Recall: we only worked on the all-time 250 movies on IMDB for this project.) I did try to get more information - I managed to get more movied IDs but it took too long to pull some of the other information. Maybe I'll try it out when I get a more powerful machine.

## Until Next Time
I hope you enjoyed reading about movies (I'd rather just watch them), and I do hope I'll have time to work on those next steps! Meanwhile, take a look at the detailed code for this project on [Github](https://github.com/jocelyn-ong/ga-dsi/blob/master/projects/projects-weekly/project-06/project-6-jocelyn-ong.ipynb){:target="_blank"}.
