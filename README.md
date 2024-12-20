[args and kwargs](#args-and-kwargs)

[Merging Dictionaries](#merging-dictionaries)

[Zip Tuple And List](#zip-tuple-and-list)

[Requests Module Warning](#requests-module-warning)

[Parallel Processing](#parallel-processing)

[Hashing And Salting](#hashing-and-salting)

[Import And Reload File](#import-and-reload-file)

[Pretty Print JSON](#pretty-print-json)

[Flask API Using SSL on localhost](#flask-api-using-ssl-on-localhost)

[PIP Config](#pip-config)

[Multi Processing Pool With Queue](#multi-processing-pool-with-queue)

[Read And Write JSON Files](#read-and-write-json-files)

[Multiple or Variable Arguments To Function](#multiple-or-variable-arguments-to-function)

[Number Padding With Zeros](#number-padding-with-zeros)

[Sending Mails](#sending-mails)

[Nested Dictionary](#nested-dictionary)

[Upgrade PIP](#upgrade-pip)

[MongoDB Operations](#mongodb-operations)

[Multiprocessing Pool](#multiprocessing-pool)

[SSL Certificate Error](#ssl-certificate-error)

[GCP StackDriver Logging](#gcp-stackdriver-logging)

[PostgreSQL Bulk Insert-Read-Export-CSV](#postgresql-bulk-insert-read-export-csv)

[OpenStack Live Migrations](#openstack-live-migrations)

[BigQuery Inserter](#bigquery-inserter)

[Python 3-10 On Ubuntu 18-04](#python-3-10-on-ubuntu-18-04)

<hr/>

#### [args and kwargs](#args-and-kwargs)

Example For : `*args` and `**kwargs`

```python
def sum_values_args(*args):
    total_sum = 0
    for element in args:
        total_sum += element
    return total_sum


def print_details(**kwargs):
    for key, value in kwargs.items():
        print("key : {} , value : {}".format(key, value))


def do_sum_print_details(*args, **kwargs):
    total_sum = 0
    for element in args:
        total_sum += element

    print("total_sum : {}".format(total_sum))

    for key, value in kwargs.items():
        print("key : {} , value : {}".format(key, value))


def main():
    total_sum = sum_values_args(1, 2, 3, 4, 5)
    print("total_sum : {}".format(total_sum))

    name = "joe"
    age = 77
    print_details(name=name, age=age, country="USA", state="California")

    print("--------------------------------------------------------------------------")

    name = "linus"
    age = 55
    do_sum_print_details(10, 20, 30, name=name, age=age, country="USA", state="Texas")


if __name__ == "__main__":
    main()
```

Output

```bash
# python3.8 argskwargs.py
total_sum : 15
key : name , value : joe
key : age , value : 77
key : country , value : USA
key : state , value : California
--------------------------------------------------------------------------
total_sum : 60
key : name , value : linus
key : age , value : 55
key : country , value : USA
key : state , value : Texas
```

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
#### [Zip Tuple And List](#zip-tuple-and-list)

```python
>>> input_tuple
((2, 2), (4, 3), (5, 5))
>>> res
['a', 'b', 'c']
>>> list(zip(input_tuple, res))
[((2, 2), 'a'), ((4, 3), 'b'), ((5, 5), 'c')]
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

After running the above program, contents of the file `data1.json`:

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

#### [Multiple or Variable Arguments To Function](#multiple-or-variable-arguments-to-function)

```python
#!/usr/local/bin/python3.7

import argparse

parser = argparse.ArgumentParser(description='Arg Parser')

parser.add_argument('--arg_1', action="store", dest="arg1", default="false")
parser.add_argument('--arg_2', action="store", dest="arg2", default="my_arg_2")
parser.add_argument('--arg_3', action="store", dest="arg3", default="my_arg_3")

def my_func(*args, **kwargs):  
    for item in args:
        print("*args item : {}".format(item))
    for key, value in kwargs.items():
        print ("key => {} , value => {}".format(key, value))

def main():
    results = parser.parse_args()
    print("ARG1 : {}".format(results.arg1))
    print("ARG2 : {}".format(results.arg2))
    print("ARG3 : {}".format(results.arg3))

    my_func(1,2,3,4, [5,6,7], {"a":"11","b":"22"}, item1="123", item2="456", dict1={"xx":"zz"})

if __name__ == "__main__":
    main()
```

Output:

```bash
ARG1 : false
ARG2 : my_arg_2
ARG3 : my_arg_3
*args item : 1
*args item : 2
*args item : 3
*args item : 4
*args item : [5, 6, 7]
*args item : {'a': '11', 'b': '22'}
key => item1 , value => 123
key => item2 , value => 456
key => dict1 , value => {'xx': 'zz'}
```

#### [Number Padding With Zeros](#number-padding-with-zeros)

For Strings

```python
>>> n = '4'
>>> print(n.zfill(3))
004
```

For Numbers

```python
>>> n = 4
>>> print(f'{n:03}') # Preferred method, python >= 3.6
004
>>> print('%03d' % n)
004
>>> print(format(n, '03')) # python >= 2.6
004
>>> print('{0:03d}'.format(n))  # python >= 2.6 + python 3
004
>>> print('{foo:03d}'.format(foo=n))  # python >= 2.6 + python 3
004
>>> print('{:03d}'.format(n))  # python >= 2.7 + python3
004
```

#### [Sending Mails](#sending-mails)

*How To Send HTML Contents Programatically*

File: `custommailsend.py`

```python
import smtplib
from email.message import EmailMessage
from email.mime.text import MIMEText
import argparse
import sys


def send_mail(to_email=[],
              cc_users=[],
              bcc_users=[],
              subject=None,
              htmlfile=None,
              smtp_server='smtp.my-company.com',
              from_email=None):

    if len(to_email) == 0:
        print("please provide 'to_email' list")
        sys.exit(1)

    if subject is None:
        print("please provide 'subject'")
        sys.exit(1)

    if htmlfile is None:
        print("please provide 'htmlfile'")
        sys.exit(1)

    msg = EmailMessage()

    msg['Subject'] = subject
    msg['From'] = from_email
    msg['To'] = to_email
    msg['Cc'] = cc_users
    msg['Bcc'] = bcc_users

    # msg.set_content(message)

    with open(htmlfile, "r", encoding='utf-8') as f:
        htmltext=f.read()

    msg.set_content(htmltext, subtype='html')

    print(msg)
    server = smtplib.SMTP(smtp_server)
    server.set_debuglevel(1)
    # server.login(username, password)  # user & password
    server.send_message(msg)
    server.quit()
    print('successfully sent the mail.')

def main():

    # Create the parser
    parser = argparse.ArgumentParser()
    # Add an argument
    parser.add_argument('--subject', type=str, required=True)
    parser.add_argument('--smtpserver', type=str, required=True)
    parser.add_argument('--htmlfile', type=str, required=True)
    parser.add_argument('--emailfrom', type=str, required=True)
    parser.add_argument('--emailto', type=str, required=True)
    parser.add_argument('--cc', type=str, required=False)
    parser.add_argument('--bcc', type=str, required=False)
    # Parse the argument
    args = parser.parse_args()
    # Print "Hello" + the user input argument

    subject = args.subject
    if subject is None or subject == '':
        print("please provide 'subject'")
        sys.exit(1)

    smtpserver = args.smtpserver
    if smtpserver is None or smtpserver == '':
        print("please provide 'smtpserver'")
        sys.exit(1)

    htmlfile = args.htmlfile
    if htmlfile is None or htmlfile == '':
        print("please provide 'htmlfile'")
        sys.exit(1)

    emailfrom = args.emailfrom
    if emailfrom is None or emailfrom == '':
        print("please provide 'emailfrom'")
        sys.exit(1)

    emailto = args.emailto
    if emailto is None or emailto == '':
        print("please provide 'emailto'")
        sys.exit(1)

    to_users = str(emailto).replace(' ', '').split(',')

    cc_users = []
    cc_args = args.cc
    if cc_args is not None:
        cc_users = cc_args.replace(' ', '').split(',')

    bcc_users = []
    bcc_args = args.bcc
    if bcc_args is not None:
        bcc_users = bcc_args.replace(' ', '').split(',')


    send_mail(to_users, cc_users, bcc_users, subject, htmlfile, smtpserver, emailfrom)

if __name__ == "__main__":
    main()
```

Usage:

```bash
python3.8 custommailsend.py \
--subject 'Testing : Please Ignore This Message' \
--smtpserver smtp.my-company.com \
--htmlfile file-contents.txt \
--emailfrom User-1@my-company.com \
--emailto 'User-2@my-company.com' \
--cc 'User-CC-1@my-company.com,User-CC-2@my-company.com' \
--bcc 'User-BCC@my-company.com'
```

In the above example, we are sending contents of HTML file `file-contents.txt`

Contents of the file `file-contents.txt`

```html
<html>
<head>
<style>
table, th, td {
  border: 1px solid #c6c6c6;
  border-collapse: collapse;
}
th, td {
  padding-top: 10px;
  padding-bottom: 20px;
  padding-left: 30px;
  padding-right: 40px;
}
</style>
</head>

<body>
<table border="1px">
<tr>
<td>Testing Instance-1</td>
<td>Testing Instance-2</td>
</tr>
<tr>
<td>Testing Instance-3</td>
<td>Testing Instance-4</td>
</tr>
</body>
</html>
```

#### [Nested Dictionary](#nested-dictionary)


```bash
sudo pip3.8 install nested_dict
```

```python
import nested_dict as nd

nest = nd.nested_dict()

nest['outer1']['inner1'] = 'v11'
nest['outer1']['inner2'] = 'v12'
nest['outer2'] = [1,2,3]

nest.to_dict()

{'outer1': {'inner1': 'v11', 'inner2': 'v12'}, 'outer2': [1, 2, 3]}
```

#### [Upgrade PIP](#upgrade-pip)

```bash
python3.8 -m pip install --upgrade pip

python2.7 -m pip install --upgrade pip
```

#### [MongoDB Operations](#mongodb-operations)

```bash
pip3.8 install pymongo
```

```python
import pymongo
import json
import os
import argparse
import sys
import pdb
from bson.objectid import ObjectId
from functools import wraps
import requests
from requests.packages.urllib3.exceptions import InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)


# mongodb connection timeout in milliseconds
ServerSelectionTimeoutMS = 5000

def main():
    #------------------------------------------------------------------------
    parser = argparse.ArgumentParser()

    valid_operations = [
        'insert_record',
        'insert_many_records',
        'query_records',
        'record_create_or_update',
        'record_update_if_exists',
        'record_replace_if_exists',
        'records_delete',
        'drop_collection'
    ]

    parser.add_argument('--operation', type=str, required=True, help=json.dumps(valid_operations))

    parser.add_argument('--mongohost', type=str, required=True, help='Ex: my-host1234.example.com')
    parser.add_argument('--mongouser', type=str, required=True, help='Ex: user1')
    parser.add_argument('--mongopass', type=str, required=True, help='Ex: pass123')
    parser.add_argument('--mongodb', type=str, required=True, help='Ex: database1')
    parser.add_argument('--mongocollection', type=str, required=True, help='Ex: collection1')
    parser.add_argument('--queryfilter', type=str, required=False, help='Ex: key1')
    parser.add_argument('--data', type=str, required=False, help='Ex: \'{"x": "1", "y": "2"}\'')

    args = parser.parse_args()

    operation = args.operation

    if operation is None or operation == "":
        print("Missing Input , Please 'operation' Ex: --operation '<value>'")
        sys.exit(1)

    if operation not in valid_operations:
        print("Please provide a valid operation : --operation '<value>' , where <value> is one of these:")
        print(json.dumps(valid_operations, indent=4, sort_keys=True))
        sys.exit(1)

    ########################################################################################

    mongohost = args.mongohost
    if mongohost is None or mongohost == "":
        print("Missing Input , Please 'mongohost' Ex: --mongohost '<value>'")
        sys.exit(1)

    mongouser = args.mongouser
    if mongouser is None or mongouser == "":
        print("Missing Input , Please 'mongouser' Ex: --mongouser '<value>'")
        sys.exit(1)

    mongopass = args.mongopass
    if mongopass is None or mongopass == "":
        print("Missing Input , Please 'mongopass' Ex: --mongopass '<value>'")
        sys.exit(1)

    mongodb = args.mongodb
    if mongodb is None or mongodb == "":
        print("Missing Input , Please 'mongodb' Ex: --mongodb '<value>'")
        sys.exit(1)

    mongocollection = args.mongocollection
    if mongocollection is None or mongocollection == "":
        print("Missing Input , Please 'mongocollection' Ex: --mongocollection '<value>'")
        sys.exit(1)

    mongocollection = args.mongocollection
    if mongocollection is None or mongocollection == "":
        print("Missing Input , Please 'mongocollection' Ex: --mongocollection '<value>'")
        sys.exit(1)

    ########################################################################################

    if operation == "record_create_or_update":
        queryfilter = args.queryfilter
        if queryfilter is None or queryfilter == "":
            print("Missing Input , Please 'queryfilter' Ex: --queryfilter '<value>' , Ex: --queryfilter '{\"name\": \"user1\" , \"age\": \"25\"}'")
            sys.exit(1)

        data = args.data
        if data is None or data == "":
            print("Missing Input , Please 'data' Ex: --data '<value>' , Ex: --value '{\"k1\": \"v1\" , \"k2\": \"v2\"}'")
            sys.exit(1)

        create_or_update_record(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter, data=data)


    if operation == "record_update_if_exists":
        queryfilter = args.queryfilter
        if queryfilter is None or queryfilter == "":
            print("Missing Input , Please 'queryfilter' Ex: --queryfilter '<value>', Ex: --queryfilter '{\"name\": \"user1\" , \"age\": \"25\"}'")
            sys.exit(1)

        data = args.data
        if data is None or data == "":
            print("Missing Input , Please 'data' Ex: --data '<value>' , Ex: --value '{\"k1\": \"v1\" , \"k2\": \"v2\"}'")
            sys.exit(1)

        record_update_if_exists(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter, data=data)

    if operation == "record_replace_if_exists":
        queryfilter = args.queryfilter
        if queryfilter is None or queryfilter == "":
            print("Missing Input , Please 'queryfilter' Ex: --queryfilter '<value>' , Ex: --queryfilter '{\"name\": \"user1\" , \"age\": \"25\"}'")
            sys.exit(1)

        data = args.data
        if data is None or data == "":
            print("Missing Input , Please 'data' Ex: --data '<value>' , Ex: --value '{\"k1\": \"v1\" , \"k2\": \"v2\"}'")
            sys.exit(1)

        replace_records_if_exists(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter, data=data)


    if operation == "insert_record":
        data = args.data
        if data is None or data == "":
            print("Missing Input , Please 'data' Ex: --data '<value>' , Ex: --value '{\"k1\": \"v1\" , \"k2\": \"v2\"}'")
            sys.exit(1)
        insert_record(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, data=data)


    if operation == "insert_many_records":
        data = args.data
        if data is None or data == "":
            print("Missing Input , Please 'data' Ex: --data '<value>' , Ex: --value '{\"k1\": \"v1\" , \"k2\": \"v2\"}'")
            sys.exit(1)
        insert_many_records(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, data=data)


    if operation == "query_records":
        queryfilter = args.queryfilter
        if queryfilter is None or queryfilter == "":
            print("Missing Input , Please 'queryfilter' Ex: --queryfilter '<value>' , Ex: --queryfilter '{\"name\": \"user1\" , \"age\": \"25\"}'")
            sys.exit(1)
        query_records(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter, print_output=True)


    if operation == "records_delete":
        queryfilter = args.queryfilter
        if queryfilter is None or queryfilter == "":
            print("Missing Input , Please 'queryfilter' Ex: --queryfilter '<value>' , Ex: --queryfilter '{\"name\": \"user1\" , \"age\": \"25\"}'")
            sys.exit(1)
        records_delete(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter)

    if operation == "drop_collection":
        drop_collection(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection)


########################################################################################

def get_json_from_str(my_str):
    return json.loads(my_str)

def is_str_json(input_string=''):
    try:
        my_dict = json.loads(input_string)
        if type(my_dict) == dict:
            return True
        else:
            return False
    except Exception:
        return False

def is_str_list(input_string=''):
    try:
        my_dict = json.loads(input_string)
        if type(my_dict) == list:
            return True
        else:
            return False
    except Exception:
        return False

########################################################################################

'''
        valid JSON : query-filer + data
'''

def valid_json_filter_and_data(f):
    @wraps(f)
    def decorated_func(*args, **kwargs):
        queryfilter = kwargs.get('queryfilter')
        data = kwargs.get('data')

        if queryfilter is None or queryfilter == "":
            result = {"response": "queryfilter:missing"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return

        if data is None or data == "":
            result = {"response": "data:missing"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return

        if not is_str_json(queryfilter):
            result = {"response": "queryfilter:invalid_json"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
        elif not is_str_json(data):
            result = {"response": "data:invalid_json"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
        else:
            return f(*args, **kwargs)
    return decorated_func

'''
        valid JSON : query-filer
'''

def valid_json_filter(f):
    @wraps(f)
    def decorated_func(*args, **kwargs):
        queryfilter = kwargs.get('queryfilter')

        if queryfilter is None or queryfilter == "":
            result = {"response": "queryfilter:missing"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return

        if not is_str_json(queryfilter):
            result = {"response": "queryfilter:invalid_json"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
        else:
            return f(*args, **kwargs)
    return decorated_func

'''
        valid JSON : data
'''

def valid_json_data(f):
    @wraps(f)
    def decorated_func(*args, **kwargs):
        data = kwargs.get('data')
        if data is None or data == "":
            result = {"response": "data:missing"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return

        if not is_str_json(data):
            result = {"response": "data:invalid_json"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
        else:
            return f(*args, **kwargs)
    return decorated_func

'''
        valid JSON (list) : data
'''

def valid_json_list_data(f):
    @wraps(f)
    def decorated_func(*args, **kwargs):
        data = kwargs.get('data')
        if data is None or data == "":
            result = {"response": "data:missing"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return

        if not is_str_list(data):
            result = {"response": "data:invalid_json_list"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
        else:
            return f(*args, **kwargs)
    return decorated_func


'''
        valid mongoDB connection
'''

def check_for_valid_mongo_connection(mongohost='127.0.0.1', mongouser='user1', mongopass='test'):
    try:
        conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
        conn.server_info()
        return True
    except Exception:
        return False


def valid_mongo_connection(f):
    @wraps(f)
    def decorated_func(*args, **kwargs):
        mongohost = kwargs.get('mongohost')
        mongouser = kwargs.get('mongouser')
        mongopass = kwargs.get('mongopass')
        if check_for_valid_mongo_connection(mongohost, mongouser, mongopass):
            return f(*args, **kwargs)
        else:
            result = {"response": "conn:mongodb_connection_invalid"}
            print(json.dumps(result, indent=4, sort_keys=True))
            return
    return decorated_func

########################################################################################

def is_none_or_empty(my_data):
    if my_data is None or my_data == "":
        return True
    else:
        return False

########################################################################################

@valid_mongo_connection
@valid_json_filter_and_data
def create_or_update_record(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', queryfilter='{"key":"key1"}', data='{"x": 1, "y": "2", "key": "key1"}'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    queryfilter_json = get_json_from_str(queryfilter)
    data_json = get_json_from_str(data)
    result = collection.update_many(queryfilter_json, {'$set': data_json}, upsert=True)
    my_dict = {"result.matched_count": result.matched_count, "result.modified_count": result.modified_count}
    print(json.dumps(my_dict, indent=4, sort_keys=True))
    return my_dict

@valid_mongo_connection
@valid_json_filter_and_data
def record_update_if_exists(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', queryfilter='{"key":"key1"}', data='{"x": 1, "y": "2", "key": "key1"}'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    queryfilter_json = get_json_from_str(queryfilter)
    data_json = get_json_from_str(data)
    result = collection.update_many(queryfilter_json, {'$set': data_json})
    my_dict = {"result.matched_count": result.matched_count, "result.modified_count": result.modified_count}
    print(json.dumps(my_dict, indent=4, sort_keys=True))
    return my_dict


@valid_mongo_connection
@valid_json_filter_and_data
def replace_records_if_exists(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', queryfilter='{"key":"key1"}', data='{"x": 1, "y": "2", "key": "key1"}'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    queryfilter_json = get_json_from_str(queryfilter)
    data_json = get_json_from_str(data)

    backend_records = query_records(mongohost=mongohost, mongouser=mongouser, mongopass=mongopass, mongodb=mongodb, mongocollection=mongocollection, queryfilter=queryfilter, print_output=False)

    results = {}

    results["record_results"] = []
    results["total_results"] = {}

    total_result_modified_count = 0
    total_result_matched_count = 0

    for backend_record in backend_records:
        record_id = backend_record["_id"]
        objInstance = ObjectId(record_id)
        result = collection.replace_one({"_id": objInstance}, data_json)
        my_dict = {"result.modified_count": result.modified_count, "result.matched_count": result.matched_count, "_id": record_id}
        total_result_modified_count = total_result_modified_count + result.modified_count
        total_result_matched_count = total_result_matched_count + result.matched_count
        results["record_results"].append(my_dict)

    results["total_results"]["result.modified_count"] = total_result_modified_count
    results["total_results"]["result.matched_count"] = total_result_matched_count

    print(json.dumps(results, indent=4, sort_keys=True))

@valid_mongo_connection
@valid_json_data
def insert_record(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', data='{"x": 1, "y": "2", "key": "key1"}'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    data_json = get_json_from_str(data)
    result = collection.insert_one(data_json)
    my_dict = {"result.inserted_id": str(result.inserted_id)}
    print(json.dumps(my_dict, indent=4, sort_keys=True))
    return my_dict

@valid_mongo_connection
@valid_json_list_data
def insert_many_records(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', data='[{"x": 1, "y": "2"}, {"x2": "11", "y2": "22"}]'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    data_json = get_json_from_str(data)
    result = collection.insert_many(data_json)
    inserted_ids = result.inserted_ids
    my_dict = {"result.inserted_ids": [str(x) for x in inserted_ids]}
    print(json.dumps(my_dict, indent=4, sort_keys=True))
    return my_dict

@valid_mongo_connection
@valid_json_filter
def query_records(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', queryfilter='{"key":"key1"}', print_output=True):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    queryfilter_json = get_json_from_str(queryfilter)
    result = collection.find(queryfilter_json)
    all_documents = []
    for document in result:
        document["_id"] = str(document["_id"])
        all_documents.append(document)
    if print_output:
        print(json.dumps(all_documents, indent=4, sort_keys=True))
    return all_documents

@valid_mongo_connection
@valid_json_filter
def records_delete(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection', queryfilter='{"key":"key1"}'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    queryfilter_json = get_json_from_str(queryfilter)
    result = collection.delete_many(queryfilter_json)
    print(json.dumps({"total_records_deleted": result.deleted_count}, indent=4, sort_keys=True))

@valid_mongo_connection
def drop_collection(mongohost='127.0.0.1', mongouser='user1', mongopass='test', mongodb='db1', mongocollection='testCollection'):
    conn = pymongo.MongoClient('mongodb://{}:{}@{}:27017/'.format(mongouser, mongopass, mongohost), serverSelectionTimeoutMS=ServerSelectionTimeoutMS)
    db = conn[mongodb]
    collection = db[mongocollection]
    collections = db.list_collection_names()

    if mongocollection not in collections:
        print(json.dumps({"collection": mongocollection, "action": "collection_missing"}, indent=4, sort_keys=True))
        return
    try:
        collection.drop()
        print(json.dumps({"collection": mongocollection, "action": "dropped"}, indent=4, sort_keys=True))
    except Exception:
        print(json.dumps({"collection": mongocollection, "action": "error:not_dropped"}, indent=4, sort_keys=True))


########################################################################################


if __name__ == "__main__":
    main()
```

Help

```bash
# python3.8 dbops.py -h
usage: dbops.py [-h] --operation OPERATION --mongohost MONGOHOST --mongouser MONGOUSER --mongopass MONGOPASS --mongodb MONGODB --mongocollection MONGOCOLLECTION [--queryfilter QUERYFILTER] [--data DATA]

optional arguments:
  -h, --help            show this help message and exit
  --operation OPERATION
                        ["insert_record", "insert_many_records", "query_records", "record_create_or_update", "record_update_if_exists", "record_replace_if_exists", "records_delete", "drop_collection"]
  --mongohost MONGOHOST
                        Ex: my-host1234.example.com
  --mongouser MONGOUSER
                        Ex: user1
  --mongopass MONGOPASS
                        Ex: pass123
  --mongodb MONGODB     Ex: database1
  --mongocollection MONGOCOLLECTION
                        Ex: collection1
  --queryfilter QUERYFILTER
                        Ex: key1
  --data DATA           Ex: '{"x": "1", "y": "2"}'
```

Please set the following environment variables

- $MONGO_HOST
- $MONGO_USER
- $MONGO_PASS

Usage Examples

```
# python3.8 dbops.py \
--operation insert_record \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--data '{"key_1" : "value_1"}'

{
    "result.inserted_id": "61f1d555b522e660fd519cfa"
}

# python3.8 dbops.py \
--operation insert_many_records \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--data '[{"key_1" : "value_1"}, {"key2":"value2"}]'

{
    "result.inserted_ids": [
        "61f1d5addba4ba7d7b165812",
        "61f1d5addba4ba7d7b165813"
    ]
}

# python3.8 dbops.py \
--operation query_records \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_11"}'

[
    {
        "_id": "61f1d5ffa78f1bfff0437b85",
        "key_11": "value_11"
    }
]

# python3.8 dbops.py \
--operation record_create_or_update \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_11"}' \
--data '{"key_22": 5000}'

{
    "result.matched_count": 1,
    "result.modified_count": 1
}


# python3.8 dbops.py \
--operation query_records \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_11"}'

[
    {
        "_id": "61f1d5ffa78f1bfff0437b85",
        "key_11": "value_11",
        "key_22": 5000
    }
]


# python3.8 dbops.py \
--operation record_update_if_exists \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_XXX"}' \
--data '{"key_XX":"value_XX"}'

{
    "result.matched_count": 0,
    "result.modified_count": 0
}


# python3.8 dbops.py \
--operation record_update_if_exists \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_11"}' \
--data '{"key_XX":"value_XX"}'

{
    "result.matched_count": 1,
    "result.modified_count": 1
}


# python3.8 dbops.py \
--operation records_delete \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection \
--queryfilter '{"key_11":"value_11"}'

{
    "total_records_deleted": 1
}


# python3.8 dbops.py \
--operation drop_collection  \
--mongohost $MONGO_HOST \
--mongouser $MONGO_USER \
--mongopass $MONGO_PASS \
--mongodb testdb \
--mongocollection testCollection

{
    "action": "dropped",
    "collection": "testCollection"
}
```

#### [Multiprocessing Pool](#multiprocessing-pool)

```python
import time
from timeit import default_timer as timer
from multiprocessing import Pool, cpu_count
from random import randrange
from datetime import datetime

def power(x, n):
    time.sleep(1)
    res = x ** n
    curr_time = str(datetime.now())
    print("{} | x => ({}) , n => ({}) , res => ({})".format(curr_time, x, n, res))
    return res

def main():
    total_count_of_inputs = 50
    input_tuple = ()
    for x in range(total_count_of_inputs):
        num_x = randrange(2, 20)
        num_n = randrange(2, 20)
        input_tuple += ((num_x, num_n),)

    # Sample
    # input_tuple = ((2, 2), (4, 3), (5, 5))

    start = timer()
    print(f'starting computations on {cpu_count()} cores')

    result = []
    with Pool() as pool:
        res = pool.starmap(power, input_tuple)
        print("\nres:\n")
        print(res)
        result = list(zip(input_tuple, res))
    end = timer()
    print(f'elapsed time: {end - start}')

    print("\nresult:\n")
    print(result)

if __name__ == '__main__':
    main()
```

Output:

```
# python3.8 mppoolworker.py
starting computations on 12 cores
2022-01-26 20:59:08.267539 | x => (11) , n => (14) , res => (379749833583241)
2022-01-26 20:59:08.267544 | x => (11) , n => (19) , res => (61159090448414546291)
2022-01-26 20:59:08.267544 | x => (16) , n => (15) , res => (1152921504606846976)
2022-01-26 20:59:08.269798 | x => (5) , n => (3) , res => (125)
2022-01-26 20:59:08.269798 | x => (10) , n => (14) , res => (100000000000000)
2022-01-26 20:59:08.274745 | x => (13) , n => (4) , res => (28561)
2022-01-26 20:59:08.277775 | x => (13) , n => (5) , res => (371293)
2022-01-26 20:59:08.280798 | x => (12) , n => (3) , res => (1728)
2022-01-26 20:59:08.286349 | x => (16) , n => (8) , res => (4294967296)
2022-01-26 20:59:08.286394 | x => (11) , n => (5) , res => (161051)
2022-01-26 20:59:08.286407 | x => (7) , n => (4) , res => (2401)
2022-01-26 20:59:08.290418 | x => (8) , n => (16) , res => (281474976710656)
2022-01-26 20:59:09.272180 | x => (4) , n => (11) , res => (4194304)
2022-01-26 20:59:09.272211 | x => (13) , n => (17) , res => (8650415919381337933)
2022-01-26 20:59:09.272232 | x => (3) , n => (16) , res => (43046721)
2022-01-26 20:59:09.274072 | x => (15) , n => (5) , res => (759375)
2022-01-26 20:59:09.274071 | x => (15) , n => (12) , res => (129746337890625)
2022-01-26 20:59:09.277878 | x => (11) , n => (11) , res => (285311670611)
2022-01-26 20:59:09.280798 | x => (8) , n => (19) , res => (144115188075855872)
2022-01-26 20:59:09.284801 | x => (17) , n => (16) , res => (48661191875666868481)
2022-01-26 20:59:09.288006 | x => (16) , n => (8) , res => (4294967296)
2022-01-26 20:59:09.288006 | x => (7) , n => (6) , res => (117649)
2022-01-26 20:59:09.288006 | x => (18) , n => (14) , res => (374813367582081024)
2022-01-26 20:59:09.290831 | x => (7) , n => (14) , res => (678223072849)
2022-01-26 20:59:10.277272 | x => (9) , n => (10) , res => (3486784401)
2022-01-26 20:59:10.277440 | x => (2) , n => (9) , res => (512)
2022-01-26 20:59:10.277463 | x => (5) , n => (14) , res => (6103515625)
2022-01-26 20:59:10.277463 | x => (2) , n => (16) , res => (65536)
2022-01-26 20:59:10.277465 | x => (15) , n => (18) , res => (1477891880035400390625)
2022-01-26 20:59:10.285478 | x => (4) , n => (15) , res => (1073741824)
2022-01-26 20:59:10.286749 | x => (10) , n => (8) , res => (100000000)
2022-01-26 20:59:10.289427 | x => (5) , n => (11) , res => (48828125)
2022-01-26 20:59:10.289422 | x => (15) , n => (6) , res => (11390625)
2022-01-26 20:59:10.289399 | x => (4) , n => (13) , res => (67108864)
2022-01-26 20:59:10.289433 | x => (8) , n => (15) , res => (35184372088832)
2022-01-26 20:59:10.292328 | x => (19) , n => (5) , res => (2476099)
2022-01-26 20:59:11.282410 | x => (17) , n => (2) , res => (289)
2022-01-26 20:59:11.282442 | x => (11) , n => (6) , res => (1771561)
2022-01-26 20:59:11.282443 | x => (9) , n => (19) , res => (1350851717672992089)
2022-01-26 20:59:11.282418 | x => (19) , n => (8) , res => (16983563041)
2022-01-26 20:59:11.282418 | x => (12) , n => (12) , res => (8916100448256)
2022-01-26 20:59:11.289843 | x => (13) , n => (14) , res => (3937376385699289)
2022-01-26 20:59:11.289843 | x => (18) , n => (15) , res => (6746640616477458432)
2022-01-26 20:59:11.294432 | x => (19) , n => (5) , res => (2476099)
2022-01-26 20:59:11.294432 | x => (11) , n => (16) , res => (45949729863572161)
2022-01-26 20:59:11.294454 | x => (14) , n => (13) , res => (793714773254144)
2022-01-26 20:59:11.294455 | x => (13) , n => (19) , res => (1461920290375446110677)
2022-01-26 20:59:11.297435 | x => (12) , n => (18) , res => (26623333280885243904)
2022-01-26 20:59:12.285949 | x => (15) , n => (16) , res => (6568408355712890625)
2022-01-26 20:59:13.290360 | x => (10) , n => (7) , res => (10000000)

res:

[379749833583241, 4194304, 61159090448414546291, 8650415919381337933, 1152921504606846976, 43046721, 100000000000000, 129746337890625, 125, 759375, 28561, 285311670611, 371293, 144115188075855872, 1728, 48661191875666868481, 4294967296, 4294967296, 161051, 117649, 2401, 374813367582081024, 281474976710656, 678223072849, 3486784401, 289, 512, 8916100448256, 65536, 1771561, 6103515625, 16983563041, 1477891880035400390625, 1350851717672992089, 1073741824, 6746640616477458432, 100000000, 3937376385699289, 67108864, 793714773254144, 11390625, 2476099, 48828125, 1461920290375446110677, 35184372088832, 45949729863572161, 2476099, 26623333280885243904, 6568408355712890625, 10000000]
elapsed time: 6.259645751

result:

[((11, 14), 379749833583241), ((4, 11), 4194304), ((11, 19), 61159090448414546291), ((13, 17), 8650415919381337933), ((16, 15), 1152921504606846976), ((3, 16), 43046721), ((10, 14), 100000000000000), ((15, 12), 129746337890625), ((5, 3), 125), ((15, 5), 759375), ((13, 4), 28561), ((11, 11), 285311670611), ((13, 5), 371293), ((8, 19), 144115188075855872), ((12, 3), 1728), ((17, 16), 48661191875666868481), ((16, 8), 4294967296), ((16, 8), 4294967296), ((11, 5), 161051), ((7, 6), 117649), ((7, 4), 2401), ((18, 14), 374813367582081024), ((8, 16), 281474976710656), ((7, 14), 678223072849), ((9, 10), 3486784401), ((17, 2), 289), ((2, 9), 512), ((12, 12), 8916100448256), ((2, 16), 65536), ((11, 6), 1771561), ((5, 14), 6103515625), ((19, 8), 16983563041), ((15, 18), 1477891880035400390625), ((9, 19), 1350851717672992089), ((4, 15), 1073741824), ((18, 15), 6746640616477458432), ((10, 8), 100000000), ((13, 14), 3937376385699289), ((4, 13), 67108864), ((14, 13), 793714773254144), ((15, 6), 11390625), ((19, 5), 2476099), ((5, 11), 48828125), ((13, 19), 1461920290375446110677), ((8, 15), 35184372088832), ((11, 16), 45949729863572161), ((19, 5), 2476099), ((12, 18), 26623333280885243904), ((15, 16), 6568408355712890625), ((10, 7), 10000000)]
```

#### [SSL Certificate Error](#ssl-certificate-error)

For Example, If You See The Below Error

```
python2.7# speedtest
Retrieving speedtest.net configuration...
Cannot retrieve speedtest configuration
ERROR: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:726)>

self._sslobj.do_handshake()
   SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:590)
```

Try To See If This Fixes The Issue

```bash
export PYTHONHTTPSVERIFY=0
python your_script

or

PYTHONHTTPSVERIFY=0 python your_script
```

`alternatively`

```python
import os, ssl
if (not os.environ.get('PYTHONHTTPSVERIFY', '') and getattr(ssl, '_create_unverified_context', None)):
    ssl._create_default_https_context = ssl._create_unverified_context

### OR JUST THIS

import ssl
ssl._create_default_https_context = ssl._create_unverified_context
```

If You Are Using `requests` Module, Then May Be Try This

```python
import requests 
from requests.packages.urllib3.exceptions import InsecureRequestWarning 
requests.packages.urllib3.disable_warnings(InsecureRequestWarning) 
```

#### [GCP StackDriver Logging](#gcp-stackdriver-logging)

Dependencies:

`requirements.txt`

```
google-cloud-logging==3.0.0
google-cloud-bigquery==2.32.0
google-cloud-storage==2.1.0; python_version == '3.6'
google-cloud-storage==2.1.0; python_version >= '3.7'
google-cloud-pubsub==2.9.0
```

```bash
pip3.8 install -r requirements.txt
```

Path To GCP Service Account

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/home/user1/gcp_service_account.json
```

```python
import os
# Imports the Google Cloud client library
from google.cloud import logging

# in case you have not exported 'GOOGLE_APPLICATION_CREDENTIALS'
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = "/home/user1/gcp_service_account.json"

# Instantiates a client
logging_client = logging.Client()

# The name of the log to write to
log_name = "myApp-Logs"
# Selects the log to write to
logger = logging_client.logger(log_name)

# The data to log
text = "Hello, world!"

# Writes the log entry
logger.log_text(text)

for i in range(100):                                                            
    logger.log_struct({'key':'a', 'val':'b'})    

print("Logged: {}".format(text))
```

INFO/DEBUG Logging

```python
import logging
import google.cloud.logging
client = google.cloud.logging.Client()

# Custom formatter returns a structure, than a string
class CustomFormatter(logging.Formatter):
    def format(self, record):
        logmsg = super(CustomFormatter, self).format(record)
        return {'msg': losgmsg, 'args':record.args}

# Setup handler explicitly -- different labels
handler = client.get_default_handler()
handler.setFormatter(CustomFormatter())

# Setup logger explicitly with this handler                                     
logger = logging.getLogger()
logger.setLevel(logging.INFO)
logger.addHandler(handler)

for i in range(100):
    logger.info('hello!', {'key':'a', 'val':'b'})
```

#### [PostgreSQL Bulk Insert-Read-Export-CSV](#postgresql-bulk-insert-read-export-csv)

```python
import psycopg2
import argparse
import sys
import random
import uuid
from datetime import datetime

from pathlib import Path
home = str(Path.home())

'''
sudo apt-get install build-essential
sudo apt-get install python3.8-dev
sudo apt-get install libpq-dev
sudo apt-get install postgresql
sudo pip3.8 install psycopg2-binary
sudo pip3.8 install psycopg2
'''

def generate_random_string():
    random_str = ''.join(random.choice('0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ') for i in range(20))
    return random_str


def drop_and_create_table(hostname='127.0.0.1', database='testdb', user='testuser', password='testpassword'):
    sql_query1 = '''drop table if exists table_1'''

    sql_query2 = '''CREATE TABLE IF NOT EXISTS table_1 (
          uuid TEXT PRIMARY KEY NOT NULL,
          currentdate TEXT NOT NULL,
          randomstr TEXT NOT NULL
        )'''
    conn = None
    try:
        conn = psycopg2.connect(database=database, user=user, password=password, host=hostname, port='5432')
        cur = conn.cursor()
        cur.execute(sql_query1)
        conn.commit()
        print("table 'table_1' dropped !")
        cur.execute(sql_query2)
        conn.commit()
        print("table 'table_1' created !")
        cur.close()
    except Exception as ex:
        print("could not create table 'table_1' : {}".format(ex))
    finally:
        if conn is not None:
            conn.close()


def bulk_insert_data(hostname='127.0.0.1', database='testdb', user='testuser', password='testpassword'):
    all_rows_data = []
    counter = 0
    num_columns = 3  # which is 'uuid', 'current_date', 'random_str'

    # generate random data
    for i in range(0, 1000):
        print("counter : {}".format(counter))
        uuid_str = str(uuid.uuid4())
        current_date = str(datetime.now())
        random_string = generate_random_string()
        row_data = (
            uuid_str,
            current_date,
            random_string,
        )
        all_rows_data.append(row_data)
        counter = counter + 1

    # using mogrify, insert all the rows generated above into the DB
    mogrify_str = "(" + ("%s," * num_columns).rstrip(",") + ")"

    conn = None
    try:
        conn = psycopg2.connect(database=database, user=user, password=password, host=hostname, port='5432')
        cur = conn.cursor()
        queryargs = ','.join(cur.mogrify(mogrify_str, i).decode('utf-8') for i in all_rows_data)
        cur.execute("INSERT INTO table_1  VALUES " + (queryargs))
        conn.commit()
        cur.close()
        print("total of ({}) records were inserted into the table 'table_1'".format(len(all_rows_data)))
    except Exception as ex:
        print("could not insert all the rows into the table 'table_1' : {}".format(ex))
    finally:
        if conn is not None:
            conn.close()


def export_table_to_csv(hostname='127.0.0.1', database='testdb', user='testuser', password='testpassword', filename='output.csv'):
    conn = None
    try:
        conn = psycopg2.connect(database=database, user=user, password=password, host=hostname, port='5432')
        cur = conn.cursor()
        sql_query = "select * from table_1"
        outputquery = 'copy ({0}) to stdout with csv header'.format(sql_query)
        with open(filename, 'w') as f:
            cur.copy_expert(outputquery, f)
        cur.close()
        conn.close()
        print("all the rows from table 'table_1' were saved to file ({})".format(filename))
    except Exception as ex:
        print("could not export table 'table_1' to csv file ({})' : {}".format(filename, ex))
    finally:
        if conn is not None:
            conn.close()


def fetch_all_rows(hostname='127.0.0.1', database='testdb', user='testuser', password='testpassword'):
    conn = None
    try:
        conn = psycopg2.connect(database=database, user=user, password=password, host=hostname, port='5432')
        cur = conn.cursor()
        query = "select uuid, currentdate, randomstr from table_1"
        cur.execute(query)
        rows = cur.fetchall()
        for row in rows:
            print("uuid        : {}".format(row[0]))
            print("durrentdate : {}".format(row[1]))
            print("randomstr   : {}".format(row[2]))
            print("--------------------------------------------------")
    except Exception as ex:
        print("could not insert all the rows into the table 'table_1' : {}".format(ex))
    finally:
        if conn is not None:
            conn.close()


if __name__ == "__main__":

    list_of_operations = ["drop_and_create_table", "bulk_insert_data", "export_csv", "fetch_all_rows"]

    parser = argparse.ArgumentParser()
    parser.add_argument("-s", "--server", help="PostgreSQL Host")
    parser.add_argument("-d", "--database", help="Database")
    parser.add_argument("-u", "--user", help="Username")
    parser.add_argument("-p", "--password", help="Password")
    parser.add_argument("-o", "--operation", help="Operation, Valid List of Operations: {}".format(list_of_operations))
    parser.add_argument("-w", "--writefile", help="CSV (Export) File Name")
    parser.add_argument("-t", "--table", help="PG SQL Table Name")

    args = parser.parse_args()

    if not args.operation:
        print("Please provide a valid operation ! List of valid operations : {}".format(list_of_operations))
        sys.exit(1)

    if args.operation not in list_of_operations:
        print("Please provide a valid operation ! List of valid operations : {}".format(list_of_operations))
        sys.exit(1)

    if not args.server:
        print("Please provide a valid hostname for PG SQL !")
        sys.exit(1)

    if not args.database:
        print("Please provide a valid database for PG SQL!")
        sys.exit(1)

    if not args.user:
        print("Please provide a valid username for PG SQL !")
        sys.exit(1)

    if not args.password:
        print("Please provide a valid password for PG SQL !")
        sys.exit(1)

    if args.operation == "drop_and_create_table":
        drop_and_create_table(hostname=args.server, database=args.database, user=args.user, password=args.password)

    if args.operation == "bulk_insert_data":
        bulk_insert_data(hostname=args.server, database=args.database, user=args.user, password=args.password)

    if args.operation == "fetch_all_rows":
        fetch_all_rows(hostname=args.server, database=args.database, user=args.user, password=args.password)

    if args.operation == "export_csv":
        if not args.writefile:
            print("Please provide a valid outpost (export) CSV file name !")
            sys.exit(1)
        export_table_to_csv(hostname=args.server, database=args.database, user=args.user, password=args.password, filename=args.writefile)


'''
----[Python Script]----

# python3.8 bulkinsert.py -h
usage: bulkinsert.py [-h] [-s SERVER] [-d DATABASE] [-u USER] [-p PASSWORD] [-o OPERATION] [-w WRITEFILE] [-t TABLE]

optional arguments:
  -h, --help            show this help message and exit
  -s SERVER, --server SERVER
                        PostgreSQL Host
  -d DATABASE, --database DATABASE
                        Database
  -u USER, --user USER  Username
  -p PASSWORD, --password PASSWORD
                        Password
  -o OPERATION, --operation OPERATION
                        Operation, Valid List of Operations: ['drop_and_create_table', 'bulk_insert_data', 'export_csv', 'fetch_all_rows']
  -w WRITEFILE, --writefile WRITEFILE
                        CSV (Export) File Name
  -t TABLE, --table TABLE
                        PG SQL Table Name
                        
----[DB]----

metadata=> \d table_1
               Table "public.table_1"
   Column    | Type | Collation | Nullable | Default
-------------+------+-----------+----------+---------
 uuid        | text |           | not null |
 currentdate | text |           | not null |
 randomstr   | text |           | not null |
Indexes:
    "table_1_pkey" PRIMARY KEY, btree (uuid)

metadata=> select count(*) from table_1;
 count
-------
     0
(1 row)

----[Bulk Insert Data]----

# python3.8 bulkinsert.py --server my-pgsql-host.company.com --database $TEST_DB --user $TEST_USER --password $TEST_PASS --operation bulk_insert_data

counter : 0
counter : 1
counter : 2
counter : 3
counter : 4
counter : 5
counter : 6
counter : 7
counter : 8
...
...
...
counter : 993
counter : 994
counter : 995
counter : 996
counter : 997
counter : 998
counter : 999
total of (1000) records were inserted into the table 'table_1'


----[Fetch All Rows]----

# python3.8 bulkinsert.py --server my-pgsql-host.company.com --database $TEST_DB --user $TEST_USER --password $TEST_PASS --operation fetch_all_rows

uuid        : 20ba7978-8628-40ce-92b5-a2dc1dbe18a6
durrentdate : 2022-03-31 13:40:34.105504
randomstr   : ZTSPMRYC3ZYPHWU630XG
--------------------------------------------------
uuid        : c2f68eee-bc10-4816-af03-9681d4a85b4b
durrentdate : 2022-03-31 13:40:34.105554
randomstr   : W34I871O7V8IZ719EN0P
--------------------------------------------------
uuid        : a0f09791-355f-4feb-bbde-8c09745e23c9
durrentdate : 2022-03-31 13:40:34.105587
randomstr   : 3PWMOOR2GG3IA92XKGST
--------------------------------------------------
uuid        : dfcbda51-eda7-483b-8c11-ca14f5d59833
durrentdate : 2022-03-31 13:40:34.105620
randomstr   : UT72MK3M8ZHP11Z0MJU6
--------------------------------------------------
...
...
...
uuid        : f8e3691f-276a-4d59-a4e0-df2426211cf9
durrentdate : 2022-03-31 13:40:34.134973
randomstr   : IHIXZRBUC6A4IQQM3YF1
--------------------------------------------------

----[Export To CSV]----

# python3.8 bulkinsert.py --server my-pgsql-host.company.com --database $TEST_DB --user $TEST_USER --password $TEST_PASS --operation export_csv --writefile output.csv

all the rows from table 'table_1' were saved to file (output.csv)

Verify CSV:

# cat output.csv
uuid,currentdate,randomstr
20ba7978-8628-40ce-92b5-a2dc1dbe18a6,2022-03-31 13:40:34.105504,ZTSPMRYC3ZYPHWU630XG
c2f68eee-bc10-4816-af03-9681d4a85b4b,2022-03-31 13:40:34.105554,W34I871O7V8IZ719EN0P
a0f09791-355f-4feb-bbde-8c09745e23c9,2022-03-31 13:40:34.105587,3PWMOOR2GG3IA92XKGST
dfcbda51-eda7-483b-8c11-ca14f5d59833,2022-03-31 13:40:34.105620,UT72MK3M8ZHP11Z0MJU6
...
...
...
61368551-a494-46a5-ba5f-617748fb32f6,2022-03-31 13:40:34.134816,7I9FLDFEWNRJYF4CMET0
2a0a83bc-ca2e-4bad-9d92-63d0d98331ff,2022-03-31 13:40:34.134843,XN9TLLJ4U5FD1B7AZFPU
8400703f-dffa-449a-9c7a-945eee001104,2022-03-31 13:40:34.134868,EUK5835Y7JRJ3SS7C4H8
62508838-901c-4fcc-84fa-d81e36a57d85,2022-03-31 13:40:34.134898,GEUAONSJUYY9S9MAMTG9
6c7f5a12-fd89-412e-aa37-d2a71c8b7a3d,2022-03-31 13:40:34.134923,76UL1GTJUE3O2NZNKUMF
facea397-a88b-44ff-bf05-67794cb3b552,2022-03-31 13:40:34.134948,JMY2L5L7CWIJN0KWAR0N
f8e3691f-276a-4d59-a4e0-df2426211cf9,2022-03-31 13:40:34.134973,IHIXZRBUC6A4IQQM3YF1
'''
```

#### [OpenStack Live Migrations](#openstack-live-migrations)

```python
#!/opt/homebrew/bin/python3.9

import string
import random
import uuid
import json
import time
from multiprocessing.dummy import Pool as ThreadPool
from multiprocessing import Queue
from multiprocessing import Process
import threading

N = 7

my_queue = Queue()

def get_random_str():
    res = ''.join(random.choices(string.ascii_uppercase + string.digits, k=N))
    return res


def get_migration_jobs():
    jobs = []
    for i in range(0, 20):
        src_hv = "host-{}-{}.prod.walmart.com".format(get_random_str(), i)
        dest_hv = "host-{}-{}.prod.walmart.com".format(get_random_str(), i)
        vm_uuid = str(uuid.uuid4())
        my_job = (src_hv, dest_hv, vm_uuid,)
        jobs.append(my_job)
    jobs.append(("-1","-1","-1"))
    return jobs


def live_migrate(job):
    (hv_source, hv_dest, vm_uuid) = job
    result = {vm_uuid: False}
    try:
        print("(IN_PROGRESS) Live Migrating : {} from {} to {}...".format(vm_uuid, hv_source, hv_dest))
        time.sleep(random.uniform(2.0, 4.0))
        print("(DONE) Live Migration Done : {} from {} to {}.".format(vm_uuid, hv_source, hv_dest))
        result = {vm_uuid: True}
    except Exception as ex:
        result = {vm_uuid: False}
    finally:
        my_queue.put(result)
        close_queue = True
        return result


def main():
    my_jobs = get_migration_jobs()
    print(json.dumps(my_jobs, indent=4, sort_keys=True))

    result_of_migrations = perform_live_migration(4)
    print(json.dumps(result_of_migrations, indent=4, sort_keys=True))


def fetch_results_from_queue():
    try:
        while True:
            result = my_queue.get()
            if list(result)[0] == "-1":
                break
            print("Data From Queue : {}".format(result))
    except Exception as ex:
        print("error : could not fetch data from queue : {}".format(ex))


def perform_live_migration(thread_pool_size):

    monitoring_thread = threading.Thread(target=fetch_results_from_queue, args=())
    monitoring_thread.start()

    tasks = get_migration_jobs()
    pool = ThreadPool(thread_pool_size)
    results = pool.map(live_migrate, tasks)
    pool.close()
    pool.join()
    monitoring_thread.join()
    print("Done With All Migrations !")
    return results

if __name__ == "__main__":
    main()
```

#### [BigQuery Inserter](#bigquery-inserter)

- 2 Columns
  - data (JSON)
  - scrape_ts (Scrape TimeStamp , Partitioned Every 1 Hour)
- Write JSON Data to "data" column
- GCP Project Data
  - PROJECT_ID > `my-gcp-project`
  - DATASET_ID > `my_dataset`
  - TABLE_ID > `my_table`
- Auto Create `my_table` > If it does not exist
- Pre-Requisite : Make sure dataset `my_dataset` already exists

#### How To Run ?

```bash
python3.9 bq_write.py
```

File : `bq_write.py`

```python
from google.cloud import bigquery
from google.api_core import exceptions
from typing import Union, List, Dict, Any
import json
from datetime import datetime
import pytz
import os

'''
python3.9 -m pip install google-cloud-bigquery pytz
'''

'''
# Set environment variable for authentication
export GOOGLE_APPLICATION_CREDENTIALS="/home/user/my-gcp-project.json"
'''

class BigQueryHandler:
    def __init__(self, project_id: str, dataset_id: str, table_id: str):
        """Initialize BigQuery handler"""
        self.client = bigquery.Client(project=project_id)
        self.table_ref = f"{project_id}.{dataset_id}.{table_id}"

    def create_table_if_not_exists(self) -> bool:
        """Create a table with JSON data column and hourly partitioned timestamp if it doesn't exist"""
        try:
            # Check if table exists
            try:
                self.client.get_table(self.table_ref)
                print(f"Table {self.table_ref} already exists")
                return False
            except exceptions.NotFound:
                # Define schema
                schema = [
                    bigquery.SchemaField("data", "JSON", mode="REQUIRED"),
                    bigquery.SchemaField("scrape_ts", "TIMESTAMP", mode="REQUIRED")
                ]

                # Create table configuration
                table = bigquery.Table(self.table_ref, schema=schema)

                # Set hourly time partitioning on scrape_ts
                table.time_partitioning = bigquery.TimePartitioning(
                    type_=bigquery.TimePartitioningType.HOUR,
                    field="scrape_ts"
                )

                # Create the table
                self.client.create_table(table)
                print(f"Created table {self.table_ref}")
                return True

        except Exception as e:
            print(f"Error creating table: {str(e)}")
            raise

    def insert_data(self, data: Union[Dict[str, Any], List[Dict[str, Any]]]) -> bool:
        """Insert data into the BigQuery table"""
        try:
            # Convert single dict to list
            if isinstance(data, dict):
                data = [data]

            current_time = datetime.now(pytz.UTC)

            # Prepare rows for insertion
            rows = []
            for item in data:
                # Convert datetime to string format that BigQuery expects
                row = {
                    "data": json.dumps(item),
                    "scrape_ts": current_time.strftime('%Y-%m-%d %H:%M:%S.%f %Z')
                }
                rows.append(row)

            # Get table reference and insert rows
            table = self.client.get_table(self.table_ref)
            errors = self.client.insert_rows_json(table, rows)

            if not errors:
                print(f"Successfully inserted {len(rows)} rows")
                return True
            else:
                print(f"Insertion errors: {errors}")
                return False

        except Exception as e:
            print(f"Error inserting data: {str(e)}")
            return False

    def batch_insert(self, data_list: List[Dict[str, Any]], batch_size: int = 1000) -> bool:
        """Insert data in batches"""
        try:
            for i in range(0, len(data_list), batch_size):
                batch = data_list[i:i + batch_size]
                if not self.insert_data(batch):
                    return False
                print(f"Processed batch {i//batch_size + 1}")
            return True

        except Exception as e:
            print(f"Batch insertion error: {str(e)}")
            return False

def generate_sample_data(num_records: int = 10) -> List[Dict[str, Any]]:
    """Generate sample data for testing"""
    return [
        {
            "id": i,
            "name": f"Product {i}",
            "price": round(10 + i * 1.5, 2),
            "metadata": {
                "category": f"Category {i % 3}",
                "in_stock": i % 2 == 0
            }
        }
        for i in range(num_records)
    ]

def main():
    # Configuration
    PROJECT_ID = "my-gcp-project"
    DATASET_ID = "my_dataset"
    TABLE_ID = "my_table"

    try:
        # Initialize handler
        handler = BigQueryHandler(PROJECT_ID, DATASET_ID, TABLE_ID)

        # Create table if it doesn't exist
        handler.create_table_if_not_exists()

        # Generate and insert sample data
        print("\nInserting single record...")
        single_record = {
            "id": 0,
            "name": "Test Product",
            "price": 99.99,
            "metadata": {
                "category": "Test Category",
                "in_stock": True
            }
        }
        handler.insert_data(single_record)

        print("\nInserting multiple records...")
        sample_data = generate_sample_data(5)
        handler.insert_data(sample_data)

        print("\nPerforming batch insertion...")
        large_dataset = generate_sample_data(150)
        handler.batch_insert(large_dataset, batch_size=50)

    except Exception as e:
        print(f"An error occurred: {str(e)}")

if __name__ == "__main__":
    main()
```

#### [Python 3-10 On Ubuntu 18-04](#python-3-10-on-ubuntu-18-04)

# Installing Python 3.10 on Ubuntu 18.04

This guide provides step-by-step instructions for installing Python 3.10 on Ubuntu 18.04 from source. Since Python 3.10 isn't available in the default Ubuntu 18.04 repositories, we'll build it from source code.

## Prerequisites

Before beginning the installation, ensure you have sufficient privileges to install packages and the required disk space (approximately 500MB).

## Installation Steps

### 1. Install Required Dependencies

```bash
sudo apt update
sudo apt install -y build-essential zlib1g-dev libncurses5-dev libgdbm-dev \
libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```

### 2. Download and Extract Python Source Code

```bash
wget https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz
tar -xf Python-3.10.13.tgz
cd Python-3.10.13
```

### 3. Configure and Build Python

```bash
./configure --enable-optimizations
# Replace '4' with your number of CPU cores
sudo make -j 4
sudo make altinstall
```

Note: We use `make altinstall` instead of `make install` to prevent replacing the default Python binary.

### 4. Install pip

```bash
curl -sS https://bootstrap.pypa.io/get-pip.py | python3.10
```

### 5. Verify Installation

```bash
python3.10 --version
```

## Making Python 3.10 the Default (Optional)

To set Python 3.10 as the default Python 3 interpreter:

```bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.10 1
```

To switch between different Python versions:

```bash
sudo update-alternatives --config python3
```

## Uninstallation

If you need to remove Python 3.10:

```bash
sudo rm -rf /usr/local/lib/python3.10
sudo rm /usr/local/bin/python3.10
sudo rm /usr/local/bin/pip3.10
```

## Troubleshooting

### SSL Issues

If you encounter SSL-related errors:

```bash
# Install SSL dependencies
sudo apt-get install -y libssl-dev openssl

# Reconfigure Python with SSL
cd Python-3.10.13
./configure --enable-optimizations --with-openssl=/usr
sudo make -j 4
sudo make altinstall
```

### Development Headers

If you need Python development headers:

```bash
sudo apt-get install python3.10-dev
```

## Using with Ansible

To use this Python version with Ansible, specify it in your inventory file:

```ini
[your_host]
hostname ansible_python_interpreter=/usr/local/bin/python3.10
```

## Package Installation

After installation, you can install Python packages using:

```bash
python3.10 -m pip install package_name
```