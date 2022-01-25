[Merging Dictionaries](#merging-dictionaries)

[Requests Module Warning](#requests-module-warning)

[Parallel Processing](#parallel-processing)

[Hashing And Salting](#hashing-and-salting)

[Import And Reload File](#import-and-reload-file)

[Pretty Print JSON](#pretty-print-json)

#### [Merging Dictionaries](#merging-dictionaries)

Example-1:

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

Example-2:

```python
def merge_dicts(dict1, dict2):
    res = {**dict1, **dict2}
    return res

x = { "a" : 123, "b": "hello world", "c" : [1,2,3,4,5,6], "d" : { "a1": 11 , "a2": 22}}
y = { "b" : "whats up" , "d" : { "a2": 9999}}

merged = merge_dicts(x,y)
print(merged)
{'a': 123, 'b': 'whats up', 'c': [1, 2, 3, 4, 5, 6], 'd': {'a2': 9999}}
```

#### [Requests Module Warning](#requests-module-warning)

How To Get Rid Of This Warning

```python
resp = requests.get('https://IP/url'), verify=False)

Warning: 

/usr/local/lib/python3.6/site-packages/urllib3/connectionpool.py:858:
InsecureRequestWarning: Unverified HTTPS request is being made.
Adding certificate verification is strongly advised.
See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings InsecureRequestWarning) 
```

Fix

```python
import requests 
from requests.packages.urllib3.exceptions import InsecureRequestWarning 
requests.packages.urllib3.disable_warnings(InsecureRequestWarning) 
```

#### [Parallel Processing](#parallel-processing)

Example-1

```python
iterable = range(0,1000000)

def func(n):
    # Real hard work here
    return n**2

def execute_func_using_verstack():
    from verstack import Multicore
    import pickle
    worker = Multicore()
    result = worker.execute(func, iterable)
    pickle.dump(result, open('iteration_result.p', 'wb'))

if __name__ == '__main__':
    execute_func_using_verstack()
```

Example-2

```python
iterable = range(0,10)

def return_two_outputs(n):
    # Real hard work here
    return n**2, n**3

from verstack import Multicore
worker = Multicore()
result = worker.execute(return_two_outputs, iterable)
print(result)
[[0, 1, 4, 9, 16, 25, 36, 49, 64, 81], [0, 1, 8, 27, 64, 125, 216, 343, 512, 729]]
```

Example-3

```python
# iterate over multiple iterables
iterable1 = range(0,10)
iterable2 = range(10,20)
iterable3 = range(20,30)

def process_multiple_iterables(lst1, lst2, lst3):
    result_1 = lst1**2
    result_2 = lst2**2
    result_3 = lst3**2
    result = result_1 + result_2 + result_3
    return result

from verstack import Multicore
# notice the multiple_iterables parameter
worker = Multicore(multiple_iterables = True) 
# notice how multiple iterables are passed in a list
result = worker.execute(process_multiple_iterables, [iterable1, iterable2, iterable3])
```

#### [Hashing And Salting](#hashing-and-salting)

```python

from cryptography.fernet import Fernet

#this is your "password"
key = Fernet.generate_key()
print("key : ({})".format(key))

cipher_suite = Fernet(key)
print("cipher_suite : ({})".format(cipher_suite))

encoded_text = cipher_suite.encrypt(b"Hello stackoverflow!")
print("encoded_text : ({})".format(encoded_text))

decoded_text = cipher_suite.decrypt(encoded_text)
print("decoded_text : ({})".format(decoded_text))

'''
Sample Output

# python3.8 hashsalt.py

key : (b'JnzcZrqoeKKWlxNikqgTR7rwi2NLK62OlL9Bc989GBA=')
cipher_suite : (<cryptography.fernet.Fernet object at 0x10d2d9d00>)
encoded_text : (b'gAAAAABh8CnMDZFx5fynIqbqwOlxsRmZKJmLEQ7USvcAfjvR_uF9bLJoGs2DPN_wQTXcmV7XY0zhjtBg9yU13aSqMMoq3u2MFbtvGwTlxo-5n9ybF5_uXlY=')
decoded_text : (b'Hello stackoverflow!')
'''
```

#### [Import And Reload File](#import-and-reload-file)

```python
from importlib import reload;import backup;reload(backup);from backup import *
```

#### [Pretty Print JSON](#pretty-print-json)

```python
import pprint
import json

x = '{ "a" : 123, "b": "hello world", "c" : [1,2,3,4,5,6], "d" : { "a1": 11 , "a2": 22}}'

parsed_json = json.loads(x)

print(json.dumps(parsed_json, indent=4, sort_keys=True))

{
    "a": 123,
    "b": "hello world",
    "c": [
        1,
        2,
        3,
        4,
        5,
        6
    ],
    "d": {
        "a1": 11,
        "a2": 22
    }
}

pprint.pprint(parsed_json)

{'a': 123,
 'b': 'hello world',
 'c': [1, 2, 3, 4, 5, 6],
 'd': {'a1': 11, 'a2': 22}}
```