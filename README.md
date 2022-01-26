[Merging Dictionaries](#merging-dictionaries)

[Requests Module Warning](#requests-module-warning)

[Parallel Processing](#parallel-processing)

[Hashing And Salting](#hashing-and-salting)

[Import And Reload File](#import-and-reload-file)

[Pretty Print JSON](#pretty-print-json)

[Flask API Using SSL on localhost](#flask-api-using-ssl-on-localhost)

[PIP Config](#pip-config)

[Multi Processing Pool With Queue](#multi-processing-pool-with-queue)

[Read And Write JSON Files](#read-and-write-json-files)

<hr/>

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

#### [Flask API Using SSL on localhost](#flask-api-using-ssl-on-localhost)

FYI : Use A Random Pass/Key Like `cert@1234_abcdef` (in all the 'openssl' commands below)

Generate Certificates

```bash
openssl genrsa -out CA.key -des3 2048

openssl req -x509 -sha256 -new -nodes -days 3650 -key CA.key -out CA.pem
```

Create A File `localhost.ext`, with following contents:

```
authorityKeyIdentifier = keyid,issuer
basicConstraints = CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = localhost
IP.1 = 127.0.0.1
```

```bash
openssl genrsa -out localhost.key -des3 2048

openssl req -new -key localhost.key -out localhost.csr

openssl x509 -req -in localhost.csr -CA CA.pem -CAkey CA.key -CAcreateserial -days 3650 -sha256 -extfile localhost.ext -out localhost.crt

openssl rsa -in localhost.key -out localhost.decrypted.key
```

> Important 
- Go To Browser Settings >
- Security/Certifcates > 
- Import 'CA.pem' (Which Was Generated Above, `To Trust The CA`)

Flask API : 'runapp.py' 

```python
import ssl
import json
import requests
from flask import Flask, request
from flask import jsonify
from flask import Response
from flask import make_response
import logging
import logging.handlers
import pprint
import syslog

syslog.syslog("this is a test message")

app = Flask(__name__)

@app.route('/')
def hello():
    return 'Hello, World!'

@app.route("/api/v1/getData", methods=['GET'])
def get_data():
    if request.method == "GET":
        syslog.syslog("request : GET : /api/v1/getData")
        response = make_response()
        data = { "a" : 123, "b": "hello world", "c" : [1,2,3,4,5,6], "d" : { "a1": 11 , "a2": 22}}
        response = jsonify(data)
        response.headers.add("Access-Control-Allow-Origin", "*")
        response.headers.add('Access-Control-Allow-Credentials', 'true')
        response.headers.add('Access-Control-Allow-Methods', '*')
        return response

if __name__ == "__main__":
    syslog.syslog("starting api...")
    context = ('localhost.crt', 'localhost.decrypted.key')
    app.run(debug=True, port=8000, host='127.0.0.1', ssl_context=context)
```

Start The Flask App

```bash
python3.8 runapp.py

 * Serving Flask app 'runapp' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: on
 * Running on https://127.0.0.1:8000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 104-795-116
127.0.0.1 - - [25/Jan/2022 13:09:00] "GET / HTTP/1.1" 200 -
```

Now, you should be able to access the URL `https://localhost:8000/api/v1/getData`

#### [PIP Config](#pip-config)

File `~/.pip/pip.conf`

```ini
[global]
trusted-host = pypi.python.org
               pypi.org
               files.pythonhosted.org
```

#### [Multi Processing Pool With Queue](#multi-processing-pool-with-queue)

File : `multiproc.py`

```python
from multiprocessing import Process, Queue
import time
import sys

def reader_proc(queue):
    ## Read from the queue; this will be spawned as a separate Process
    while True:
        msg = queue.get()         # Read from the queue and do nothing
        if (msg == 'DONE'):
            break

def writer(count, queue):
    ## Write to the queue
    for ii in range(0, count):
        queue.put(ii)             # Write 'count' numbers into the queue
    queue.put('DONE')

if __name__=='__main__':
    pqueue = Queue() # writer() writes to pqueue from _this_ process
    for count in [100000, 200000, 400000]:

        ### reader_proc() reads from pqueue as a separate process

        reader_p = Process(target=reader_proc, args=((pqueue),))
        reader_p.daemon = True
        reader_p.start()        # Launch reader_proc() as a separate python process

        _start = time.time()
        writer(count, pqueue)    # Send a lot of stuff to reader()
        reader_p.join()          # Wait for the reader to finish
        print("Sending {0} numbers to Queue() took {1} seconds".format(count, (time.time() - _start)))
```

Output:

```
# python3.8 multiproc.py

Sending 100000 numbers to Queue() took 1.3959307670593262 seconds
Sending 200000 numbers to Queue() took 3.0190162658691406 seconds
Sending 400000 numbers to Queue() took 7.687782049179077 seconds
```

FYI >

```
----------------------------------------------------------------------------
                  | Multi-args   Concurrence    Blocking     Ordered-results
----------------------------------------------------------------------------
Pool.map          | no           yes            yes          yes
Pool.map_async    | no           yes            no           yes
Pool.apply        | yes          no             yes          no
Pool.apply_async  | yes          yes            no           no
Pool.starmap      | yes          yes            yes          yes
Pool.starmap_async| yes          yes            no           no
```

#### [Read And Write JSON Files](#read-and-write-json-files)

```python
import json

def get_json_contents(file_name):
    try:
        with open(file_name) as json_file:
            data = json.load(json_file)
            return data
    except Exception as ex:
        print("error : could not read file ({}} : {}".format(file_name, ex))


def write_json_contents(json_data, output_file):
    try:
        with open(output_file, 'w') as output_file:
            json.dump(json_data, output_file, indent=4, sort_keys=True)
    except Exception as ex:
        print("error : could not write file ({}} : {}".format(output_file, ex))


def main():

    ## step-1: write the json file

    file_name = 'data1.json'
    json_data = { "a" : 123, "b": "hello world", "c" : [1,2,3,4,5,6], "d" : { "a1": 11 , "a2": 22}}
    write_json_contents(json_data, file_name)

    print("\njson data written to file '{}'...\n".format(file_name))

    ## step-2: read the json file

    read_content = get_json_contents(file_name)

    if read_content is not None:
        print("\ncontents of the file '{}':\n".format(file_name))
        print(json.dumps(read_content, indent=4, sort_keys=True))

if __name__ == "__main__":
    main()
```

After running the above program, contents of the file 'data1.json':

```json
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
```