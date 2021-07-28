# Named Tuple
## Description
The NamedTuple is another class, under the **collections** module. Like the dictionary type objects, it contains keys and that are mapped to some values. In this case we can access the elements using keys and indexes.

## Example
``` python
import collections as col
#create employee NamedTuple
Employee = col.namedtuple('Employee', ['name', 'city', 'salary'])
#Add two employees
e1 = Employee('Asim', 'Delhi', '25000')
e2 = Employee('Bibhas', 'Kolkata', '30000')
#Access the elements using index
print('The name and salary of e1: ' + e1[0] + ' and ' + e1[2])
#Access the elements using attribute name
print('The name and salary of e2: ' + e2.name + ' and ' + e2.salary)
#Access the elements using getattr()
print('The City of e1 and e2: ' + getattr(e1, 'city') + ' and ' + getattr(e2, 'city'))

```

# Web scrapping with Beautiful Soup 4
### Some information
Beautiful Soup is a Python library for pulling data out of HTML and XML files. It works with your favorite parser to provide idiomatic ways of navigating, searching, and modifying the parse tree. It commonly saves programmers hours or days of work.
- **Python’s html.parser** - Not as fast as lxml, less lenient than html5lib.
- **lxml’s HTML parser** - Very fast. External C dependency.
- **html5lib** - Extremely lenient. Parses pages the same way a web browser does. **Very slow**.



### Installation
We need to install following packages: ``` beautifulsoup4 ```, ```lxml```, ```faster_than_requests```.

----------------

# Quick usage

``` python
soup = BeautifulSoup(html, 'lxml')
```
