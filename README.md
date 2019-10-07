# web_scrape_101

NY Time article documenting all of Trump's lies he has told publicily since taking the oath of office.
https://www.nytimes.com/interactive/2017/06/23/opinion/trumps-lies.html

The code snippets are written in a way to show you exactly what the commands are doing. The first half of this walkthrough shows you how to extract the data from one result rather than the full set. In doing this we are figuring out how to do it for one result and can then translate it into a loop for the whole data set. 


In order to read the HTML from the article into Python we need to use `requests`.
`$ pip install requests`

in `pis`

```python
>>> import requests
>>> r = requests.get('https://www.nytimes.com/interactive/2017/06/23/opinion/trumps-lies.html')
>>> print(r.text[0:500])
```
This will return the first 500 chars of the HTML file - which shows us that the request is working properly.
Next we are going to parse the HTML using the `beatuifulsoup` library. 

`$ pip install beautifulsoup4`

### Getting started...

```python
>>> from bs4 import BeautifulSoup
>>> soup = BeautifulSoup(r.text, 'parser.html')
>>> results = find_all('span', attrs={'class':'short-desc'}
>>> len(results)
>>> (out) 116
```

So here we can see that we have scraped 116 lies and stored them in a beautifulsoup object called `results`, however they are still in the form of raw HTML.

### Extract the date...

```python
>>> first_result = results[0]
>>> first_result.find('strong')
>>> (out) <strong>Jan. 21</strong>
>>> first_result.find('strong').text
>>> (out) 'Jan. 21\xao'
>>> first_result.find('strong').text[0:-1]
>>> (out) 'Jan. 21'
>>> first_result.find('strong').text[0:-1] + ', 2017'
>>> (out) 'Jan. 21, 2017'
```

### Extract the lie...

```python
>>> first_result
>>> first_result.contents
>>> first_result.contents[1]
>>> first_result.contents[1][1:-2]
>>> (out) "I wasn't a fan of Iraq. I didn't want to go into Iraq."
```

### Extract the explanation...

```python
>>> first_result.contents[2]
>>> first_result.find('a')
>>> first_result.find('a').text[1:-1]
>>> (out) "He was for an invasion before he was against it."
```

### Extract the URL...

```python
>>> first_result.find('a')
>>> first_result.find('a')['href']
>>> (out) https://www.buzzfeed.com...
```

### Building the dataset

```python
>>> records = []
>>> for result in results
>>> date = result.find('strong').text[0:-1] + ', 2017'
>>> lie = result.contents[1][1:-2]
>>> explanation = result.find('a').text[1:-1]
>>> url = result.find('a')['href']
>>> records.append((date, lie, explanation, url))
>>> len(records)
>>> (out) 116
```


