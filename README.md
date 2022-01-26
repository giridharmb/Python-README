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

[Multiple or Variable Arguments To Function](#multiple-or-variable-arguments-to-function)

[Number Padding With Zeros](#number-padding-with-zeros)

[Sending Mails](#sending-mails)

[Nested Dictionary](#nested-dictionary)

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