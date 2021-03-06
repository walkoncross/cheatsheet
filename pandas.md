#Pandas Cheat Sheet

##Series

###Creating Series Objects
Expression | Description
---  | ---
`Series([1, 2, 3])` | A Series object
`Series([1, 2, 3], dtype='int64')` | A Series object with the specified data type
`Series([1, 2, 3], index=list('abc'))` | A Series object with the specified index
`Series([1, 2, 3], dtype='int64')` | A Series object with the specified data type
`Series({'foo': 1, 'bar': 2, 'baz': 3})` | A Series object with the keys of the dict as indices
`Series({'a': 1, 'b': 2}, index=list('abc'))` | A Series object with the specified elements and index (the unspecified elements will be `NaN`s)

###Indexing
Expression | Description
---  | ---
`s['a']` | An element of a series with the specified index
`s[['a', 'b']]` | A sub-series with the specified indices
`s['a':'c']` | Slicing by indices (opposed to normal Python slicing, the endpoint is inclusive)
`s[s < 3]` | Filter by the specified criteria

###Series Attributes and Member Functions
Attribute/Function | Description
---  | ---
`s.dtype` | Data type
`s.name` | Name
`s.index` | Index
`s.index.name` | Index name
`s.values` | Values as an array
`s.drop('c')` `s.drop(['c', 'd'])` | Drop elements with the specified index(es)

###Operations and Arithmetic
Expression | Description
---  | ---
`pd.isnull(s)` `pd.notnull(s)` | A Series object with the (not) nullness of the elements
`s + t` `s * t` `...` | Perform operations on the series with the indices aligned (intoduces `NaN`s if there is a difference in indices)

###NumPy's Array Operations on Series
Expression | Description
---  | ---
`s[s > 2]` | The series is indexed with a boolean series and the sub-series with the elements at the True indices are returned (preserves the index)
`s * 2` `np.exp(s)` | Array-like operations with a scalar (preserves the index)

###Sorting and Ranking
Expression | Description
---  | ---
`s.sort_index()` `...` | Returns a new sorted object with the elements sorted by the index
`s.sort_index(ascendind=False)` `...` | Descending sort
`s.order()` | Sort by elements
`s.rank()` | Return a series with the same index as `s` but instead of the original elements of the series it contains the rank of each element if the series would be sorted (in case of ties it assigns the mean rank of the tied group)
`s.rank(method='first')` | Assigns the rank of the first element to each element in the group in case of a tie (available tie resolution methods are `min`, `max`, `mean` and `first`)
`s.rank(ascending=False)` | Sorts in descending order
##DataFrame

###Creating DataFrame Objects
Expression | Description
---  | ---
`DataFrame({'foo': [1, 2, 3], 'bar': [4, 5, 6]})` | A DataFrame object with the columns `foo` and `bar` and 3 rows
`DataFrame(data, columns=['foo', 'bar'])` | A DataFrame object with the specified (order of) columns
`DataFrame(data, index=list('abcde'))` | A DataFrame object with the specified row indices
`DataFrame([{'foo': 1, 'bar': 2}, {'foo': 5, 'bar': 10}])` | A DataFrame object with the list elements as rows and the dict elements as columns
`DataFrame(np.arange(15).reshape((5, 3)), columns=['foo', 'bar', 'baz'])` | A DataFrame object from the 2D array with the specified columns

###Indexing
Expression | Description
---  | ---
`d['foo']` `d.foo` | The column `foo` of the frame
`d['bar'] = 16.5` | Assign a value to all elements in the specified column of the frame
`d[d > 10]` | A data frame with the same shape as `d` but with `NaN`s instead of the elements where the specified filtering criteria does not hold
`d[d.foo > 10]` | The rows of the frame where the criteria is true
`d.foo = np.arange(5)` | Assign the elements of the specified list to the elements of the column (assuming that the length of the list is the same as the #rows of the column)
`d.ix['a']` | The row(s) at `index='a'` of the frame
`d.ix[['a', 'c'], 'foo']` | A sub-frame of the frame with the specified rows (`a` and `c`) and the specified column (`foo`)
`d.ix['a':'c', 'foo']` | A sub-frame of the frame with the specified rows (`a` to `c` inclusive) and the specified column (`foo`)
`d.icol(2)` | The elements of the third column as a Series with the original row index preserved
`d.irow(2)` | The elements of the third row as a Series with the column names as indices
`d.xs('c')` | The elements of the row `c` as a Series with the column names as indices
`d.loc[d.bar == 2, 'foo']` | Returns a subset of the column `foo` that can be used to assign a new value to 

###DataFrame Attributes and Member Functions
Attribute/Function | Description
---  | ---
`d.dtypes` | The data types of each column
`d.name` | Name
`d.index` | Indices
`d.index.name` | Index name
`d.columns` | List of columns
`d.columns.name` | Name of the columns list
`d.values` | Values as a 2D array
`d.drop('c')` `d.drop(['c', 'd'])` | Drop elements with the specified index(es)
`d.drop('foo', axis=1)` `s.drop(['foo', 'bar'], axis=1)` | Drop columns with the specified index(es)

###Operations and Arithmetic
Expression | Description
---  | ---
`d + e` `...` | Perform operations on the frames with the row and column indices aligned (intoduces `NaN`s if there is a difference in indices)
`d.add(e)` `d.sub(e)` `d.div(e)` `d.mul(e)`| Flexible arithmetic methods on frames
`d.add(e, fill_value=0)` | Flexible arithmetic methods on frames with a default fill value (instead of introducing `NaN`s)
`d + s` `d.sub(s)` `...`| Arithmetic between a frame and a series (by default it matches the index of the series on the columns of the frame broadcasted down the rows)
`d.add(s, axis=0)` `...`| Arithmetic between a frame and a series (match the index of the series )

###Function Application and Mapping
Expression | Description
---  | ---
`np.abs(d)` `...` | Element-wise `abs` on the frame (works with all NumPy functions)
`f.apply(f)` | Applies the given function `f` row-wise
`f.applymap(f)` | Applies the given function `f` element-wise and returns a frame of the same shape as `d` with the results
`d['c'].map(f)` | Applies `f` on the elements of column `c` element-wise

###Sorting and Ranking
Expression | Description
---  | ---
`d.sort_index()` `...` | Returns a new sorted object with the elements sorted by the index
`d.sort_index(axis=1)` `...` | Sort columns by the index
`d.sort_index(ascendind=False)` `...` | Descending sort
`d.sort_index(by='foo')` | Sort by elements in column `foo`
`d.sort_index(by=['foo', 'bar'])` | Sort by elements in columns `foo` then `bar`

##Index

###Index Types
Type | Description
---  | ---
`Index` | The most general Index type
`Int64Index` | Special Index type for integer values
`MultiIndex` | Hierarchical Index type representing multiple levels of indices on a single axis
`DatetimeIndex` | Nanosecond timestamp Index
`PeriodIndex` | Special index for period (timespan) data

###Creating Index Objects
Expression | Description
---  | ---
`pd.Index(np.arange(10))` | An Index object

###Index Attributes and Member Functions
Attribute/Function | Description
---  | ---
`i.append(j)` | Concatenate with other Index
`i.difference(j)` | Set diff of two Indices as an Index
`i.intersection(j)` | Intersection
`i.union(j)` | Union
`i.isin[1, 2, 3]` | A boolean array indicating whether the Index elements are contained by the passed-in collection
`i.delete(5)` | Remove element from the Index
`i.drop([5, 6])` | Remove elements from the Index
`i.insert(5, 3)` | Insert 5 at position 3
`i.is_monotonic` | Monotonicity
`i.is_unique` | Uniqueness
`i.unique()` | Compute unique sequence

###Reindexing Series and DataFrames
Expression | Description
---  | ---
`s.reindex(['a', 'b', 'c'], fill_value=0)` | Reindex the series
`d.reindex(['a', 'b', 'c'], method='ffill')` | Reindex the frame

###Reindex Function Args
Arg | Description
---  | ---
`index` | The new index sequence (can be an pd.Index type or any sequence-like structure)
`method` | The method to populate the missing elements (can be `ffill`, `pad`, `backfill` or `bfill`)
`fill_value` | Substitute value to use when introducing missing data
`limit` | Maximum size gap to fill
`level` | Match simple index on level of MultiIndex, otherwise select subset of
`copy` | If `True` then always copy underlying data

###Hierarchical Index
Expression | Description
---  | ---
`Series([1, 2, 3, 4], index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]])` | A Series object with a 2-level index
`DataFrame(np.random.randint(0, 10, 15).reshape(5, 3), index=[['foo', 'foo', 'foo', 'bar', 'bar'], ['a', 'b', 'c', 'a', 'b']])` | A DataFrame object with a 2-level row index
`d['a']` | All elements with the main index `a` and any subindex
`s.unstack()` | Unstack a series with a multi-index into a frame (the main index labels will be the row labels and the secondary index labels will be the columns)
`d.stack()` | Stack a frame into a multi-indexed series

##Summarizing and Computing Stats

###Reduction Methods
Method | Description
---  | ---
`d.count()` | Returns a series with the frame's column (by default) indices and the count of the elements in the columns
`d.describe()` | A set of summary stats
`d.min()` `d.max()` | Minimum/maximum
`np.argmin(e[1])` `np.argmax(e[1])` | The index location (i.e. an integer) of the minimal/maximal element
`d.idxmin()` `d.idxmax()` | The index value of the minimal/maximal element
`d.quantile(0.7)` | Quantile
`d.sum()` | Sum
`d.mean()` | Mean
`d.median()` | Median
`d.mad()` | Mean absolute deviation
`d.var()` | Simple variance
`d.std()` | Sample standard deviation
`d.skew()` | Sample skewness (3rd moment)
`d.kurt()` | Sample kurtosis (4th moment)
`d.cumsum()` | Cumulative sum
`d.cummin()` `d.cummax()` | Cumulative min/max
`d.cumprod()` | Cumulative product
`d.diff()` | 1st arithmetic difference
`d.pct_change())` | Percent change

###Reduction Method Args
Arg | Description
---  | ---
`axis` | Which axis to preserve (the default is to preserve columns)
`skipna` | Exclude missing values (the default is True)
`level` | Reduction level for hierarchical MultiIndex

###Other Statistical Methods
Method | Description
---  | ---
`d.cor()` | The correlation of the frame's columns with each other
`d.cov()` | The covariance of the frame's columns with each other
`d.foo.corr(d.bar)` | The correlation of the columns `foo` and `bar`
`d.foo.cov(d.bar)` | The covariance of the columns `foo` and `bar`
`d.corrwith(e.foo)` | The covariance of `d`'s columns with `e.foo`
`pd.value_counts(s)` | Value counts for series elements
`d.apply(pd.value_counts).fillna(0)` | Value counts for each column with zeros instead of `NaN`s

###Handling Missing Values
Expression | Description
---  | ---
`d.is_null()` `s.is_null()` | Element-wise nullness
`d.dropna()` | Drop rows that contain any null values
`d.dropna(how='all')` | Drop rows that contain only null values
`d.dropna(axis=1)` | Drop columns that contain any null values
`d.dropna(thresh=2)` | Drop rows that contain at least 2 null values
`d.fillna(0)` | Fill in null values with zeros
`d.fillna(0, inplace=True)` | Fill in null values in place with zeros
`d.fillna(method='ffill')` | Forward fill null values (i.e. copy the value just before the null into the null element)
`d.fillna(d.mean())` | Fill in null values with the mean of the column

##Data Loading and Storage

###Text Reading Functions
Function | Description
---  | ---
`pd.read_csv('data.txt')` | Load the file file into a Pandas DataFrame (the deafult delimiter is `,`)
`pd.read_table('data.txt')` | Load the file file into a Pandas DataFrame (the deafult delimiter is `\t`)
`pd.read_fwf('data.txt')` | Load the fixed width file into a Pandas DataFrame (no delimiter)

###Text Reading Function Args
Arg | Description
---  | ---
`path` | File path or URL
`sep` `delimiter` | Character sequence or regex as a delimiter between fields
`header` | Row number to use as a header (the default is 0). Should be `None` if there is no header
`index_col` | Column numbers or names to use as a row index
`names` | A list of column names for result (combine with `header=None`)
`skiprows` | Number of rows at the beginning of the file to ignore (or a list of row numbers indexed from 0)
`na_values` | Sequence of values to be replaced with `NaN`
`comment` | Character or char sequence to split off comments from the end of lines
`parse_dates` | Attempt to parse data to Datetime
`converters` | A dict containing the the column names to converter functions
`date_parser` | The function to use for parsing dates
`nrows` | Number of rows to read
`iterator` | Return a `TextParser` object
`chunksize` | The size of file chunks (for iteration)
`skip_footer` | Number of lines to ignore at the end of file
`verbose` | Print parser output info
`encoding` | Text encoding for unicode
`squeeze` | Return a Series for a data file with 1 column
`thousands` | Thousand separator

###Text Writing Functions
Function | Description
---  | ---
`d.to_csv('data.txt')` | Write data to a CSV file

###Text Writing Function Args
Arg | Description
---  | ---
`path_or_buf` | File path
`sep` | Character sequence or regex as a delimiter between fields (the default is `,`)
`na_rep` | The representation of empty values (the default is an empty string)
`float_format` | Format string for floats
`columns` | Columns to write
`header` | Write column names (the default is True)
`index` | Write row names (the default is True)
`index_label` | Column names for index column (the default is `None`)
`mode` | Python file mode (the default is `w`)
`encoding` | String encoding (for Python 2 the default is `ascii`)
`line_terminator` | Newline char (the default is `\n`)
`quoting` | Optional constant from csv module (the default is `csv.QUOTE_MINIMAL`)
`quotechar` |  Character used to quote fields (the default is `"`)
`doublequote` | Control quoting of quotechar inside a field (the default is True)
`escapechar` | Character used to escape sep and quotechar when appropriate (the default is `None`)
`chunksize` | Rows to write at a time
`date_format` | Format string for datetime objects (the default is `None`)
`decimal` | Character recognized as decimal separator (the default is `.`)

###JSON Data I/O
Function | Description
---  | ---
`json.dumps(d)` | Dump d as JSON (standard json module)
`d = json.loads(s)` | Load d form a string (standard json module)
`d.to_json('data.json')` | Write data frame to a JSON file
`d = pd.read_json('data.json')` | Read data frame from a JSON file

###Binary Data I/O
Function | Description
---  | ---
`d.to_pickle('d_pickle')` | Write data frame to a binary file
`d = pd.read_pickle('d_pickle')` | Read data frame from a binary file

##Combining and Merging Data Sets

###Database-style Data Frame Merge
Expression | Description
---  | ---
`pd.merge(d, e, on='key')` | Merge the two data frames on `key` as if they were two relational database tables

###Merge Args
Arg | Description
---  | ---
`key` | What (common) column to use when merging
`left` | Data frame to be used as the left frame
`right` | Data frame to be used as the right frame
`how` | What kind of merge (join) to use (the default is `inner`, other choices are `left`, `right` and `outer`)
`on` | A list of column names to merge on (each of them must be found in both frames)
`left_on` | Columns in the left frame to merge on
`right_on` | Columns in the right frame to merge on
`left_index` | User the row index in the left frame to merge
`right_index` | User the row index in the right frame to merge
`sort` | Sort merged data lexicographically by merge keys (the default is True)
`suffixes` | Tuple of string values to append to column names in case of overlap
`copy` | Copy data to the resulting data structure (the default is True)

###Concatenating Along an Axis
Expression | Description
---  | ---
`pd.concat([d, e], axis=1)` | Concatenate the two data frames along the 1st axis

###Concat Args
Arg | Description
---  | ---
`objs` | A list or a dict of pandas objects to concatenate
`axis` | Axis to concatenate along (the default is 0)
`join` | What to do with axis indices (`inner` means intersection, `outer` means union, the default is `outer`)
`join_axes` | Specific indexes to use for the other `n-1` axes instead of performing intersection/union
`keys` | Values to associate with objects being concatenated
`levels` | Indexes to use as hierarchical index level or levels if keys are passed
`names` | Names for the created hierarchical levels
`verify_integrity` | Check new axis in concatenated object and raise an exception if there is a duplicate
`ignore_index` | Do not preserve indexes along concatenated axis

###Combining Data with Overlap
Expression | Description
---  | ---
`d.combine_first(e)` | Fill in `NaN` values in `d` from `e` (the two frames have to share the same shape)

##Reshaping and Pivoting

###Reshaping with Hierarchical Indexing
Expression | Description
---  | ---
`d.stack()` | Pivots the columns into the rows with hierarchical index producing a series
`d.unstack()` | For a hierarchically indexed series rearranges it back to a data frame with columns (by default the innermost level is unstacked)
`d.unstack(0)` | For a hierarchically indexed series rearranges it back to a data frame with columns by unstacking the specified level

###Pivoting "long" to "wide" Format
Expression | Description
---  | ---
`d.pivot('foo', 'bar')` | Pivots the table with `foo` as the row index and `bar` as the column index (this is equivalent with creating a hierarchical index and setting it with `set_index` and reshaping with `unstack`)

##Data Transformation

###Managing Duplicates
Expression | Description
---  | ---
`d.duplicated()` | Returns a boolean series which tells for each row whether it is a dubplicate in the table
`d.drop_duplicates()` |  Drops duplicate rows keeping the first one from each group
`d.drop_duplicates(['k1'], take_last=True)` | Drops duplicate rows based on the specified list of columns keeping the last one from each group

###Transforming Data Using Function or Mapping
Expression | Description
---  | ---
`d['bar'] = d['foo'].map(f)` | Assign to the column `bar` of the data frame the series that is a result of applying the function `f` to the column `foo`
 `d['bar'] = d['foo'].map(s)` | Assign to the column `bar` of the data frame the series that is a result of applying the dict `s` as a mapping function to the column `foo`
`d['bar'] = d['foo'].map(lambda x: x.lower())` | Assign to the column `bar` of the data frame the series that is a result of applying the lambda function to the column `foo`

###Replacing Values
Expression | Description
---  | ---
`s.replace(100, 0)` | Replace `100` with `0` in the series
`s.replace([100, 200], 0)` | Replace `100` and `200` with `0` in the series
`s.replace([100, 200], [0, 5])` | Replace `100` with 0 and `200` with `5` in the series
`s.replace({'foo': 0, 'bar': 1})` | Replace items based on the specified mapping

###Discretization and Binning
Expression | Description
---  | ---
`cats = pd.cut(s, bins)` | Return categories defined in `bins` and assign each element in `s` into these categories
`cats.codes` | The assigned categories for each element
`cats.categories` | The bin categories
`cats = pd.cut(s, 3, precision=2)` | Assign the elements in `s` into 3 equal size groups with the precision of 2
`pd.qcut(s, 4)` | The quartiles of s as categories
`pd.qcut(s, [0, 0.1, 0.5, 0.9, 1])` | The user-defined quantiles of s as categories

###Permutation and Random Sampling
Expression | Description
---  | ---
`d.take(np.random.permutation(len(d)))` | Random permutation of `d`'s rows
`d.take(np.random.permutation(len(d))[:5])` | Random sample of `d`'s rows without replacement
`d.take(np.random.randint(0, len(d), size=5))` | Random sample of `d`'s rows with replacement

###Computing Indicator and Dummy Variables
Expression | Description
---  | ---
`d[['data']].join(pd.get_dummies(d['key'], prefix='key'))` | Get indicator variables for each possible value of the `key` column and join it to the data frame with the name prefix `key_*`
`pd.get_dummies(pd.cut(values, bins))` | Assign values in `values` into bins in `bins` and introduce indicator variables for bin memberships

##String Manipulation

###String Object Methods
Method | Description
---  | ---
`s.count('foo')` | Number of non-overlapping substring occurrences
`s.startswith(t)` `s.endswith(t)` | Return `True` if `s` starts/end with `t`
`s.join(a)` | Using `s` as a delimiter concatenate `a`'s elements
`s.index(t)` | Return the index of `t`'s first occurrence in `s` (raises `ValueError` if not found)
`s.find(t)` `s.rfind(t)` | Return the index of `t`'s first/last occurrence in `s` (returns `-1` if not found)
`s.replace(t, u)` | Replace all occurrences of `t` in `s` with `u`
`s.strip()` `s.rstrip()` `s.lstrip()` | Trim whitespace characters
`s.split(t)` | Split `s` at every occurrence of `t`
`s.lower()` `s.upper()` | Lowercase/uppercase `s`
`s.rjust(10, c)` `s.rjust(10, c)` | Right/left justify `s` within the given width using `c` as the paddign character

###Regex Methods
Method | Description
---  | ---
`p = re.compile('\s+')` | Compile a regex and store it for later use
`p.findall(s)` `re.findall(p, s)` `r.finditer(s)` `re.finditer(p, s)`| Find all non-overlapping occurrences of the pattern `p` in the string `s`
`p.match(s)` | Match the pattern `p` to the beginning of `s` and optionally segment pattern components into groups
`p.search(s)` | Search for an occurrence of the pattern `p` in the string `s` and return a match object or `None` if not found
`p.split(s)` | Split the string `s` at each non-overlapping occurrence of the pattern `p`
`re.sub(p, s, t)` `p.sub(s, t)` | Replace all occurrences of the pattern `p` with the string `t` in the string `s`
`re.subn(p, s, t, n)` `p.subn(s, t, n)` | Replace the first `n` occurrences of the pattern `p` with the string `t` in the string `s`

###Vectorized String Functions
Method | Description
---  | ---
`s.str.cat(t)` | Concat strings in the series `s` and series `t` element-wise with an optional delimiter
`s.str.contains(str)` | Return a boolean series if each string in the series `s` contains the string `str`
`s.count(str)` | Count the occurrences of `str` for each string in the series `s`
`s.str.startswith(str)` `s.str.endswith(str)` | Return a boolean series if each string in the series `s` starts/ends with `str`
`s.str.findall(str)` | Finds all occurrences of `str` in each element of the series `s`
`s.str.get(5)` | Index into each string in the series `s` and return a series with the resulting characters
`s.str.join(c)` | Join each element in the series `s` using the character `c`
`s.str.len()` | Return a series with the lenghts of each element in the series `s`
`s.str.lower()` `s.str.upper()` | Lower/upper case each string in `s`
`s.str.match(p)` | Match each string in `s` against the pattern `p` and return the list of matches
`s.str.pad(30)` | Add whitespaces to the string ins `s`
`s.str.repeat(3)` | Duplicate string values
`s.str.replace(t, u)` | Replace the occurrences of `t` to `u` in each element of `s`
`s.str.split(str)` | Split each element of `s` by `str`
`s.str.strip()` `s.str.rstrip()` `s.str.lstrip()` | Remove whitespaces from each element of `s`

##Plotting

###Plotting Pandas Objects
Expression | Description
---  | ---
`s.plot()` `d.plot()` | Plot a series or a data frame
`d.plot(kind='bar')` | Bar plot
`d.plot(logy=True)` | Logarithmic Y scale
Most PyPlot stuff | The same

##Data Aggregation and Grouping

###Group By
Expression | Description
---  | ---
`d.groupby(['foo'])` | Group `d` by the distinct values of the column `foo`
`d.groupby(['foo', 'bar'])` | Group `d` by the distinct pairs of the columns `foo` and `bar`
`d['bar'].groupby(['foo'])` | Group column `bar` by the distinct values of the column `foo`
`d['bar'].groupby(['foo']).mean()` | Get the mean of column `bar` by the distinct values of the column `foo`
`d.groupby(d.dtypes, axis=1)` | Group the columns of `d` by the data type
`dict(list(d.groupby(['foo'])))` | Get a dict of key-value pairs where the key is the distinct values of the column `foo` and the value is the grouped data
`d.groupby('foo')[['bar']].mean()` | Compute the means of `bar` grouped by values of `foo`
`d.groupby(mapping, axis=1).sum()` | Group the columns of `d` by `mapping` and compute the sum of all column groups for each row
`d.groupby(lambda x: ord(x) % 2 == 0, axis=1).sum()` | Group the columns of `d` by an anonymous mapping function and compute the sum of all column groups for each row
`d.groupby(level='foo', axis=1).count()` | Group by the distinct values of a level of a hierarchical index

###Data Aggregation
Expression | Description
---  | ---
`d.groupby('foo')[['bar']].mean()` `d.groupby('foo')[['bar']].agg('mean')` | The mean for column `bar` grouped by the distinct values of the column `foo`
`d.groupby('foo').quantile(0.8)` | The 80th percentile for each column grouped by the distinct values of the column `foo`
`d.groupby('foo').agg(f)` | Aggregate each column of `d` with the custom aggregation function `f` grouped by column `foo` (`f` takes an array and returns a value)
`d.groupby('foo')[['bar']].agg(['min', 'mean', 'max'])` | Some descriptive statistics for column `bar` grouped by `foo`
`d.groupby('foo')['bar'].agg([('SUM', 'sum')])` | Set a custom name for the aggregated column

###Aggregation Functions
Function | Description
---  | ---
`count` | Count non-NA values
`sum` | Sum of non-NA values
`mean` | Mean of non-NA values
`median` | Arithmetic median of non-NA values
`std` | Unbiased (n-1 denominator) standard deviation
`var` | Variance
`max` `min` | Max/min of non-NA values
`prod` | Product of non-NA values
`first` `last` | First/last non-NA values