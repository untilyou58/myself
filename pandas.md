=====================================Pandas Wiki=====================================

- Install Pandas
```
pip install pandas
```

- Installing with Miniconda
```
conda install
```

- Import pandas
```
import pandas as pd
```

- Creating data
In->
```
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```
Out->
```
	Yes	No
0	50	131
1	21	2
```

- Index data
In->
```
pd.DataFrame({'Bob': ['I liked it', 'It was awful.'], 'Sue': ['Pretty good', 'Bland.']}, 
		index=['Product A', 'Product B'])
```
Out->
```
		Bob		Sue
Product A	I liked it.	Pretty good.
Product B	It was awful.
```

- Series
is a sequence of data values. If a DataFrame is a table, a Series is a list. And in fact you can create one with nothing more than a list:
In->
```
pd.Series([1, 2, 3, 4, 5])
```
Out->
```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

- Index with Series
In->
```
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```
Out->
```
2015 Sales    30
2016 Sales    35
2017 Sales    40
Name: Product A, dtype: int64
```

- Reading data files
In->
```
review = pd.read_csv("filename.csv")
review.shape
```
Out->
```
(129971, 14)
```

- Get the first five rows
```
review.head()
```

- The pd.read_csv() function is well-endowed, with over 30 optional parameters you can specify. For example, you can see in this dataset that the CSV file has a built-in index, which pandas did not pick up on automatically. To make pandas use that column for the index (instead of creating a new one from scratch), we can specify an index_col.
```
review = pd.read_csv("filename.csv", index_col=0)
review.head()
```

- Naive accessors 
Native Python objects provide good ways of indexing data. Pandas carries all of these over, which helps make it easy to start with.
```
review.country
review['country']
review['country'][0]
```

- Indexing with pandas

-> Index-based selection
Pandas indexing works in one of two paradigms. The first is index-based selection: selecting data based on its numerical position in the data. `iloc` follows this paradigm.
To select the first row of data in a DataFrame, we may use the following:
```
reviews.iloc[0]
```
Both loc and iloc are row-first, column-second. This is the opposite of what we do in native Python, which is column-first, row-second.

```
reviews.iloc[0]
reviews.iloc[:, 0]
```
On its own, the : operator, which also comes from native Python, means "everything". When combined with other selectors, however, it can be used to indicate a range of values. For example, to select the country column from just the first, second, and third row, we would do:
```
review.iloc[:3, 0]
```
Or, to select just the second and third entries, we would do:
```
review.iloc[1:3, 0]
```
Or, pass a list
```
reviews.iloc[[0, 1, 2], 0]
```
Finally, it's worth knowing that negative numbers can be used in selection. For example here are the last five elements of the dataset.
```
reviews.iloc[-5:]
```

- Label-based selection

To get first entry in review
```
review.loc[0, 'country']
```
`iloc` is conceptually simpler than `loc` because it ignores the dataset's indices. When we use `iloc` we treat the dataset like a big matrix (a list of lists), one that we have to index into by position. `loc`, by contrast, uses the information in the indices to do its work. Since your dataset usually has meaningful indices, it's usually easier to do things using loc instead.

```
reviews.loc[:, ['taster_name', 'taster_twitter_handle', 'points']]
```

- Choose between `loc` and `iloc`
When choosing or transitioning between loc and iloc, there is one "gotcha" worth keeping in mind, which is that the two methods use slightly different indexing schemes.

iloc uses the Python stdlib indexing scheme, where the first element of the range is included and the last one excluded. So 0:10 will select entries 0,...,9. loc, meanwhile, indexes inclusively. So 0:10 will select entries 0,...,10.

Why the change? Remember that loc can index any stdlib type: strings, for example. If we have a DataFrame with index values Apples, ..., Potatoes, ..., and we want to select "all the alphabetical fruit choices between Apples and Potatoes", then it's a lot more convenient to index df.loc['Apples':'Potatoes'] than it is to index something like df.loc['Apples', 'Potatoet] (t coming after s in the alphabet).

This is particularly confusing when the DataFrame index is a simple numerical list, e.g. 0,...,1000. In this case df.iloc[0:1000] will return 1000 entries, while df.loc[0:1000] return 1001 of them! To get 1000 elements using loc, you will need to go one lower and ask for df.loc[0:999].

Otherwise, the semantics of using loc are the same as those for iloc.

- Manipulating the index

Index is not immutabl, we can manipulate the index in any way we see fit.
```
review.set_index("title")
```

- Conditional selection
In->
```
review.country == 'Italy'
```
Out->
```
0          True
1         False
          ...  
129969    False
129970    False
Name: country, Length: 129971, dtype: bool
```
This operation produced a Series of True/False booleans based on the country of each record. This result can then be used inside of loc to select the relevant data:
```
reviews.loc[reviews.country == 'Italy']
```
This DataFrame has ~20,000 rows. The original had ~130,000. That means that around 15% of wines originate from Italy.

We also wanted to know which ones are better than average. Wines are reviewed on a 80-to-100 point scale, so this could mean wines that accrued at least 90 points.
```
reviews.loc[(reviews.country == 'Italy') & (reviews.points >= 90)]
```
Suppose we'll buy any wine that's made in Italy or which is rated above average. For this we use a pipe (|):

```
reviews.loc[(reviews.country == 'Italy') | (reviews.points >= 90)]
```
The first is isin. isin is lets you select data whose value "is in" a list of values. For example, here's how we can use it to select wines only from Italy or France:
```
reviews.loc[reviews.country.isin(['Italy', 'France'])]
```
The second is isnull (and its companion notnull). These methods let you highlight values which are (or are not) empty (NaN). For example, to filter out wines lacking a price tag in the dataset, here's what we would do:
```
reviews.loc[reviews.price.notnull()]
```

- Assigning data
```
reviews['critic'] = 'everyone'
```
Or with an iterable of values:

```
reviews['index_backwards'] = range(len(reviews), 0, -1)
```

- Summary functions
```
reviews.points.describe()
reviews.taster_name.describe()
```
For example, to see the mean of the points allotted (e.g. how well an averagely rated wine does), we can use the mean() function:
```
reviews.points.mean()
```
To see a list of unique values we can use the unique() function:
```
reviews.taster_name.unique()
```
To see a list of unique values and how often they occur in the dataset, we can use the value_counts() method:
```
reviews.taster_name.value_counts()
```

- Maps
A map is a term, borrowed from mathematics, for a function that takes one set of values and "maps" them to another set of values. In data science we often have a need for creating new representations from existing data, or for transforming data from the format it is in now to the format that we want it to be in later. Maps are what handle this work, making them extremely important for getting your work done!

map() is the first, and slightly simpler one. For example, suppose that we wanted to remean the scores the wines received to 0. We can do this as follows:
```
review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
```
apply() is the equivalent method if we want to transform a whole DataFrame by calling a custom method on each row.
```
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```
If we had called reviews.apply() with axis='index', then instead of passing a function to transform each row, we would need to give a function to transform each column.

Note that map() and apply() return new, transformed Series and DataFrames, respectively. They don't modify the original data they're called on. If we look at the first row of reviews, we can see that it still has its original points value.

```
reviews.head(1)
```
Pandas provides many common mapping operations as built-ins. For example, here's a faster way of remeaning our points column:
```
review_points_mean = reviews.points.mean()
reviews.points - review_points_mean
```
In this code we are performing an operation between a lot of values on the left-hand side (everything in the Series) and a single value on the right-hand side (the mean value). Pandas looks at this expression and figures out that we must mean to subtract that mean value from every value in the dataset.

Pandas will also understand what to do if we perform these operations between Series of equal length. For example, an easy way of combining country and region information in the dataset would be to do the following:
```
reviews.country + " - " + reviews.region_1
```
These operators are faster than map() or apply() because they uses speed ups built into pandas. All of the standard Python operators (>, <, ==, and so on) work in this manner.

However, they are not as flexible as map() or apply(), which can do more advanced things, like applying conditional logic, which cannot be done with addition and subtraction alone.


- Groupwise analysis

One function we've been using heavily thus far is the value_counts() function. We can replicate what value_counts() does by doing the following:
```
reviews.groupby('points').points.count()
```
groupby() created a group of reviews which allotted the same point values to the given wines. Then, for each of these groups, we grabbed the points() column and counted how many times it appeared. value_counts() is just a shortcut to this groupby() operation.

We can use any of the summary functions we've used before with this data. For example, to get the cheapest wine in each point value category, we can do the following:
```
reviews.groupby('points').price.min()
```
Each group we generate as being a slice of our DataFrame containing only data with values that match. This DataFrame is accessible to us directly using the apply() method, and we can then manipulate the data in any way we see fit. For example, here's one way of selecting the name of the first wine reviewed from each winery in the dataset:
```
reviews.groupby('winery').apply(lambda df: df.title.iloc[0])
```
For even more fine-grained control, you can also group by more than one column. For an example, here's how we would pick out the best wine by country and province:
```
reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])
```
Another groupby() method worth mentioning is agg(), which lets you run a bunch of different functions on your DataFrame simultaneously. For example, we can generate a simple statistical summary of the dataset as follows:
```
reviews.groupby(['country']).price.agg([len, min, max])
```


- Multi-indexes

A multi-index differs from a regular index in that it has multiple levels. For example:
```
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
mi = countries_reviewed.index
type(mi)
```
out->
```
pandas.core.indexes.multi.MultiIndex
```
in general the multi-index method you will use most often is the one for converting back to a regular index, the reset_index()
```
countries_reviewed.reset_index()
```


- Sorting

To get data in the order want it in we can sort it ourselves. The sort_values() method is handy for this.
```
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
```
sort_values() defaults to an ascending sort, where the lowest values go first. However, most of the time we want a descending sort, where the higher numbers go first. That goes thusly:
```
countries_reviewed.sort_values(by='len', ascending=False)
```
To sort by index values, use the companion method sort_index(). This method has the same arguments and default order:
```
countries_reviewed.sort_index()
```
Sort by more than one column at a time:
```
countries_reviewed.sort_values(by=['country', 'len'])
```


- Dtypes

You can use the dtype property to grab the type of a specific column. For instance, we can get the dtype of the price column in the reviews DataFrame:
```
reviews.price.dtype
```
Alternatively, the dtypes property returns the dtype of every column in the DataFrame:
```
reviews.dtypes
```
It's possible to convert a column of one type into another wherever such a conversion makes sense by using the astype() function. For example, we may transform the points column from its existing int64 data type into a float64 data type:
```
reviews.points.astype('float64')
```
A DataFrame or Series index has its own dtype, too:
```
reviews.index.dtype
```

- Missing data

Entries missing values are given the value NaN, short for "Not a Number". For technical reasons these NaN values are always of the float64 dtype.
Select NaN entries you can use pd.isnull() (or its companion pd.notnull()). This is meant to be used thusly:
```
reviews[pd.isnull(reviews.country)]
```
`fillna()` provides a few different strategies for mitigating such data. For example, we can simply replace each NaN with an "Unknown":
```
reviews.region_2.fillna("Unknown")
```
Alternatively, we may have a non-null value that we would like to replace. For example, suppose that since this dataset was published, reviewer Kerin O'Keefe has changed her Twitter handle from @kerinokeefe to @kerino. One way to reflect this in the dataset is using the replace() method:
```
reviews.taster_twitter_handle.replace("@kerinokeefe", "@kerino")
```
The replace() method is worth mentioning here because it's handy for replacing missing data which is given some kind of sentinel value in the dataset: things like "Unknown", "Undisclosed", "Invalid", and so on.


- Renaming 

The first function we'll introduce here is `rename()`, which lets you change index names and/or column names. For example, to change the points column in our dataset to score:
```
reviews.rename(columns={'points': 'score'})
```
rename() lets you rename index or column values by specifying a index or column keyword parameter, respectively. It supports a variety of input formats, but usually a Python dictionary is the most convenient. Here is an example using it to rename some elements of the index.
```
reviews.rename(index={0: 'firstEntry', 1: 'secondEntry'})
```
Both the row index and the column index can have their own name attribute. The complimentary rename_axis() method may be used to change these names. For example:
```
reviews.rename_axis("wines", axis='rows').rename_axis("fields", axis='columns')
```


- Combining

```
canadian_youtube = pd.read_csv("../input/youtube-new/CAvideos.csv")
british_youtube = pd.read_csv("../input/youtube-new/GBvideos.csv")

pd.concat([canadian_youtube, british_youtube])
```
The middlemost combiner in terms of complexity is join(). join() lets you combine different DataFrame objects which have an index in common. For example, to pull down videos that happened to be trending on the same day in both Canada and the UK, we could do the following:
```
left = canadian_youtube.set_index(['title', 'trending_date'])
right = british_youtube.set_index(['title', 'trending_date'])

left.join(right, lsuffix='_CAN', rsuffix='_UK')
```
The lsuffix and rsuffix parameters are necessary here because the data has the same column names in both British and Canadian datasets. If this wasn't true (because, say, we'd renamed them beforehand) we wouldn't need them

