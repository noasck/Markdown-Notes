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
