# Visual data analysis in Python
*by mlcourse.ai*

## Initilizing and first steps
Exclude warnings from output:
``` python
import warnings
warnings.filterwarnings('ignore')
```
Setting the seaborn:
``` python
import seaborn as sns
sns.set()
```
Graphics in retina format are more sharp and legible:
``` python
%config InlineBackend.figure_format = 'retina'
```

**Univariate analysis** looks at one feature at a time. When we analyze a feature independently, we are usually mostly interested in the distribution of its values and ignore other features in the dataset.

## Histogrames

### DataFrame built-in

We can call hist method of column or set of columns:
``` python
DataFrame.hist(column=None, by=None, grid=True,
               xlabelsize=None, xrot=None, ylabelsize=None,
               yrot=None, ax=None, sharex=False, sharey=False,
               figsize=None, layout=None, bins=10,
               backend=None, legend=False, **kwargs)
```
