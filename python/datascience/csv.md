# CSV reader

``` python
import csv

#precision 2

with open("sample.csv") as csvfile:
  samples = list(csv,DictReader(csvfile))
```
## DateTime

``` python
from datetime import datetime as dt
import time

dtnow = dt.fromtimestamp(time.time()).year
dtnow.month
dtnow.year
```

# Pandas approach

Read a comma-separated values (csv) file into DataFrame.

Also supports optionally iterating or breaking of the file into chunks.

Additional help can be found in the online docs for IO Tools.

``` python
pd.read_csv('data.csv')
```

### Returns

  **DataFrame or TextParser**

  *A comma-separated values (csv) file is returned as two-dimensional data structure with labeled axes.*
