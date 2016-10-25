---
tags:
  - DSI
  - data-science
  - projects
  - python
  - sql
---
<!-- Create a report of your findings and detail the accuracy and assumptions of your model -->
Can we predict who will survive a disaster?

{% include toc %}

## Introduction
<!-- - Do not put pictures too high up (messes with the toc) -->
This week we're looking at a classification problem, and we'll consider various classification models and how they compare to one another. In this week's scenario:

> You're working as a data scientist with a research firm that specializes in emergency management. In advance of client work, you've been asked to create and train a logistic regression model that can show off the firm's capabilities in disaster analysis.

How would this be useful to an emergency management firm? Maybe it can be used to predict the probability of survival in a disaster for efficient resource allocation (very contentious discussion here - we'll not go into that), or maybe it can be used as an after-the-fact review, to see if there were any factors that affected the survival rate.

## About the dataset
We'll be working with the very well-used Titanic dataset. We were told to pull the data from the General Assembly database, but if you'd like to work with the dataset on your own, it is also available on [Kaggle](https://www.kaggle.com/c/titanic/data){:target='_blank'}.

### Data dictionary

|Column|Type|Description
|---|---|---
|PassengerID|Integer|Numerical identifier for each passenger
|Survived|Binary (Integer)|Whether the passenger survived (0 = No; 1 = Yes)
|Pclass|Categorical (Integer)|Passenger Class<br>(1 = 1st; 2 = 2nd; 3 = 3rd);<br>Pclass is a proxy for socio-economic status<br>(1st ~ Upper; 2nd ~ Middle; 3rd ~ Lower)
|Name|String|Passenger name
|Sex|Binary (String)|Sex (male; female)
|Age|Float|Passenger age in years<br>Fractional if age less than one, xx.5 if estimated
|SibSp|Integer|Number of siblings/ spouses aboard<br>(Brother, sister, stepbrother, or stepsister; husband or wife)
|Parch|Integer|Number of parents/ children aboard<br>(Mother or father, son, daughter, stepson or stepdaughter)
|Ticket|String|Ticket Number
|Fare|Float|Amount paid for ticket
|Cabin|String|Cabin number
|Embarked|Categorical (String)|Port of embarkation<br>(C = Cherbourg; Q = Queenstown; S = Southampton)

The above information is obtained from [Kaggle](https://www.kaggle.com/c/titanic/data) and formatted for presentation purposes.

The column "Survived" is our target, and the rest of the columns may be used as features (where data is complete and relevant).

## Exploratory Data Analysis (EDA)

For our EDA, we'll look at the various columns in relation to the column "Survived" and see if we can find any relationships just from visualizations.

Plots:
- Age

  [![alt]({{ site.url }}{{ site.baseurl }}/images/bio-photo.jpg)]({{ site.url }}{{ site.baseurl }}/images/bio-photo.jpg)

- Age and sex

  [image]

- Age and class

  [image]

- Age and port of embarkation

  [image]


We can also take a look at the heatmap of the correlations in our dataset:

[image]

The heatmap tells us the strength of the correlation between "Survived" and each of the other columns, and also tells us if there is any multicollinearity between the other columns.

*(So what if there is? Multicollinearity in our features may affect interpretability of our model, so if we would like to be able to say what affects our predictions, we may want to avoid any multicollinearity issues.)*

Based on our EDA, it seems like age, class, and sex might be good predictors in our model. Port of embarkation may be a good predictor but it seems like most of the passengers got on at Southampton, so we're not sure if this will have a big impact.

## Data wrangling

Now that we've identified some of the features that we may want to use in our model, we'll need to examine each of these columns to see if any preprocessing is required.

### Missing values

Let's look at our dataset to identify any missing values:

```python
df.info()
```

[image]

Age was supposed to be one of our main features, but there are so many missing instances! We don't have a lot of data, so let's try to fill these in.

*(It would have been interesting to look at cabins and location of the cabins and see how this might have affected whether a passenger would survive or not, but there are too many of these missing and no obvious way for us to impute the values, so we'll leave cabin out for now.)*

One way we could fill in the missing values in age is to just fill in the median age in all the blanks. But maybe we can go one step further. Let's see what we can use to find age:

- Class

  [image]

- Salutation

  [image]

- Class and salutation

  [image]

It seems like if we combine class and salutation, we may be able to get a more accurate estimation of age.

Backtrack a few steps: how do we get a passenger's salutation?

[image]

Each "Name" entry is in the format `Last Name, Salutation. First Name`. The salutations all have a period to it, so we'll split the names on spaces, and look for the item that has a period, and return that.

```python
def get_salute(name):
    name_list = name.split(',')
    name_list = name_list[1].strip().split(' ')
    salute = [i for i in name_list if '.' in i]
    return salute[0].strip('.')
```

There were a few salutations which only occurred a few times, so we grouped them into the majority groups, and ended up with the five main salutations. We then found the median age of each salutation and class.

[image]

Each missing age in our dataset is then mapped to this to return an estimated age.

### Dummy variables

In our next preprocessing step, we'll create dummy variables for our categorical data using `pd.get_dummies`.

```python
df = pd.get_dummies(df, columns=["Sex", "Pclass", "Embarked"], drop_first=True)
```

This creates dummy columns for "Sex", "Pclass", "Embarked", and drops the original columns in the dataframe as well as one of the dummy columns for each item.

Using dummies for categorical data removes any 'order' implied in using numerical categories.

### Scaling continuous variables

Next, we'll scale our continuous variables so that they are comparable. We will use `StandardScaler` in `sklearn.preprocessing` for this.

Before we perform the scaling, we'll split our data into train and test sets, so that we can test our model with data that it has not encountered before.

```python
# Split the X and y into training and test sets
X_train, X_test, y_train, y_test = train_test_split(df2[all_features],df2['Survived'],stratify=df2['Survived'], test_size=0.33, random_state=77)

# Create a StandardScaler instance
scaler = StandardScaler()

# fit the scaler to the train data
scaler = scaler.fit(X_train[scale_features])
```

```python
# For each column we want to scale, create a new column with the prefix Scaled_ for the training data
for i in range(len(scale_features)):
    X_train['Scaled_'+scale_features[i]] = [x[i] for x in scaler.transform(X_train[scale_features])]
```

```python
# Now do the same for X_test
for i in range(len(scale_features)):
    X_test['Scaled_'+scale_features[i]] = [x[i] for x in scaler.transform(X_test[scale_features])]
```

Note that we've only trained our scaler on the training data, and that our test data is scaled on the same scale as the training data.

## Modeling: generating the model and drawing conclusions

We'll be looking at four different classification models - logistic regression, K nearest neighbors, 

## Round up
