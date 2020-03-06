 About data cleansing using Pandas, name 3 way you can modify the data.

- Normalize data types

Sometimes, we reading in a CSv file with a bunch of numbers, some of the numbers will read in as string instead of numeric values, or ....
```
data = pd.read_csv(‘file.csv’, dtype={‘column’: int})
```

- Checking for missing values

```
df.isnull().sum()
```

- With missing values

-> fill in the missing values with a word or symbol
```
df = df.fillna('*') 
```
-> replace it with the average
```
df[‘column’] = df[‘column'].fillna(df['column'].mean())
```
-> use the interpolate function to fill in missing values for a number. This will take the average of the number above and below the missing value in the dataframe.
```
df['column'] = df['column'].fillna(df['column'].interpolate())
```
-> delete the missing rows of data
```
df = df.dropna()
```


- With Non-standard missing values

Replace with `NaN` -> deal with missing values.
```
df = df.replace(['-','na'], np.nan)
```

- Data in the wrong format

Replace `wrong format` to NaN then we can deal with the missing values.


