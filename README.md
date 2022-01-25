# Python-README

[Merging Dictionaries](#merging-dictionaries)

[Requests Module Warning](#requests-module-warning)

#### [Merging Dictionaries](#merging-dictionaries)

```python
>>> x = { "a" : 123, "b": "hello world", "c" : [1,2,3,4,5,6], "d" : { "a1": 11 , "a2": 22}}
>>>
>>> y = { "b" : "whats up" , "d" : { "a2": 9999}}
>>>
>>> x.update(y)
>>>
>>> x
{'a': 123, 'b': 'whats up', 'c': [1, 2, 3, 4, 5, 6], 'd': {'a2': 9999}}
```

#### [Requests Module Warning](#requests-module-warning)

> How To Get Rid Of This Warning
```python
resp = requests.get('https://IP/url'), verify=False)

Warning: 

/usr/local/lib/python3.6/site-packages/urllib3/connectionpool.py:858:
InsecureRequestWarning: Unverified HTTPS request is being made.
Adding certificate verification is strongly advised.
See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings InsecureRequestWarning) 
```

> Fix
```python
import requests 
from requests.packages.urllib3.exceptions import InsecureRequestWarning 
requests.packages.urllib3.disable_warnings(InsecureRequestWarning) 
```