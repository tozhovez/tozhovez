# Arithmetic String

**The following is your assignment:**

**The API server you will build will act as a calculator.**

1. **Using any framework you choose, build an API server with a single endpoint named “calculate”. This means that the requests should be routed to http://&lt;server&gt;/calculate.**
2. **This endpoint should be able to receive a POST request with a JSON body in the following form:**

**{“data”: &lt;String&gt;}**

**The string in the data field will be a base64 encoded arithmetic string \(something like 2+2 or 2-2+1\) like the following: Misy \(Which is 2+2 b**ase64 encoded**\).**

1. **The API should now calculate the arithmetic string and return the result in a JSON response as following:**

**{“result”: 4}**

## Setup

Clone the repo, create virtualenv if necessary.

[https://github.com/tozhovez/arithmetic\_string.git](https://github.com/tozhovez/arithmetic_string.git)

Install the app:

```bash
$ cd arithmetic_string/calculator-service

$ pip install -e . 
```

Run app:

```text
$ cd arithmetic_string/calculator-service
$ python main.py 



DEBUG:asyncio:Using selector: EpollSelector
======== Running on http://127.0.0.1:50772 ========
(Press CTRL+C to quit)

```

Open browser:

```text
http://127.0.0.1:50772
```



## 

### Requirements

* [aiohttp](https://github.com/KeepSafe/aiohttp)
* [aiohttp\_jinja2](https://github.com/aio-libs/aiohttp_jinja2)

