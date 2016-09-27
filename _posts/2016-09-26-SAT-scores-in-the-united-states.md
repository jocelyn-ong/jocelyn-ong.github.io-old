Using a combination of Jupyter Notebook, Python and its various libraries, we conduct a basic exploratory analysis of SAT scores in the United States.

![sat-csv-table](https://c1.staticflickr.com/9/8448/29871624861_d2a968ba1c_o.png)

The data in the csv file is described as follows:

![data-dictionary](https://c1.staticflickr.com/9/8075/29327540764_a977c1044d_o.png)

```python
	import pandas as pd
	df = pd.read_csv('sat_scores.csv')
```

Using Pandas, we read our csv file into a data frame - this will make our subsequent steps much easier. Pandas intuitively converts the data in each column to the most likely type, and a quick check will confirm that the columns Rate, Verbal and Math have been processed as integers.

```python
	df.dtypes
```
![dtypes](https://c2.staticflickr.com/6/5647/29871624951_76f01fd215_o.png)

Using a data frame also allows us a broad overview of the numerical columns which covers common statistical measures - mean, standard deviation, minimum, maximum, as well as the quartiles:

```python
	df.describe()
```

![describe-results](https://c1.staticflickr.com/9/8470/29327540804_7b0db47bdd_o.png)

Using Seaborn, we can quickly plot each numerical column against the others - this is useful to observing the relationships between columns.

```python
import seaborn as sns
sns.pairplot(df)
```

![pairplot](https://c2.staticflickr.com/6/5277/29920761866_c80e668102_o.png)

