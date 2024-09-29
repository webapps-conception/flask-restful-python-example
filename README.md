# flask-restful-python-example
[Documentation Flask-RESTful](https://flask-restful.readthedocs.io/en/latest/)

## Installation
``` bash
pip install flask-restful
```

## Basic use
### Example use
``` bash
$ python api.py
 * Serving Flask app 'api'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 105-130-942
```

### GET the list
``` bash
$ curl http://localhost:5000/todos
{
    "todo1": {
        "task": "build an API"
    },
    "todo2": {
        "task": "?????"
    },
    "todo3": {
        "task": "profit!"
    }
}
```

### GET a single task
``` bash
$ curl http://localhost:5000/todos/todo3
{
    "task": "profit!"
}
```

### DELETE a task
``` bash
$ curl http://localhost:5000/todos/todo2 -X DELETE -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> DELETE /todos/todo2 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> 
< HTTP/1.1 204 NO CONTENT
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:30:03 GMT
< Content-Type: application/json
< Connection: close
< 
* Closing connection 0
```

### Add a new task
``` bash
$ curl http://localhost:5000/todos -d '{ "task":"something new" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /todos HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 26
> 
* upload completely sent off: 26 out of 26 bytes
< HTTP/1.1 201 CREATED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:37:29 GMT
< Content-Type: application/json
< Content-Length: 32
< Connection: close
< 
{
    "task": "something new"
}
* Closing connection 0
```

## Update a task
``` bash
$ curl http://localhost:5000/todos/todo3 -d '{ "task":"something different" }' -X PUT -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> PUT /todos/todo3 HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 32
> 
* upload completely sent off: 32 out of 32 bytes
< HTTP/1.1 201 CREATED
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 11:40:06 GMT
< Content-Type: application/json
< Content-Length: 38
< Connection: close
< 
{
    "task": "something different"
}
* Closing connection 0
```

### Result
``` bash
$ curl http://localhost:5000/todos
{
    "todo1": {
        "task": "build an API"
    },
    "todo3": {
        "task": "something different"
    },
    "todo4": {
        "task": "something new"
    }
}
```


## Use with JWT authentication
### Example usage
``` bash
$ python api-jwt.py
 * Serving Flask app 'api-jwt'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 105-130-942
```

### GET the list
``` bash
$ curl http://localhost:5000/todos
{
  "msg": "Missing Authorization Header"
}
```

>📝 We can see that we no longer have direct access to the `/todos` route.

To access a jwt_required protected view, you must send the JWT with each request:

``` bash
$ curl http://localhost:5000/login -d '{ "username":"test", "password":"test" }' -X POST -H "Content-Type: application/json" -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> POST /login HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 40
> 
* upload completely sent off: 40 out of 40 bytes
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 14:37:44 GMT
< Content-Type: application/json
< Content-Length: 354
< Connection: close
< 
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU"
}
* Closing connection 0
```

With the integration of the Token in the authorization header, you can access the requested resource:


``` bash
$ export JWT="eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU"
$ curl http://localhost:5000/todos -H "Authorization: Bearer $JWT" -H "Content-Type: application/json" -v
*   Trying ::1...
* TCP_NODELAY set
* connect to ::1 port 5000 failed: Connexion refusée
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 5000 (#0)
> GET /todos HTTP/1.1
> Host: localhost:5000
> User-Agent: curl/7.64.0
> Accept: */*
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmcmVzaCI6ZmFsc2UsImlhdCI6MTcyNzYyMDY2NCwianRpIjoiMDVkNzMxOGEtZjdmMC00MzQ5LTk5ZjgtNjk3NTAxMjMwMmE3IiwidHlwZSI6ImFjY2VzcyIsInN1YiI6InRlc3QiLCJuYmYiOjE3Mjc2MjA2NjQsImNzcmYiOiIyYmJjY2JjMi1kZTk0LTQ0OGQtYTEyYS0wZDM4MzQxYjJjNDIiLCJleHAiOjE3Mjc2MjE1NjR9.UBOgw3I_lm48xcnO2hO1CA3XT6D6HDOWxX0knG8bDHU
> Content-Type: application/json
> 
< HTTP/1.1 200 OK
< Server: Werkzeug/2.2.3 Python/3.7.3
< Date: Sun, 29 Sep 2024 14:39:22 GMT
< Content-Type: application/json
< Content-Length: 150
< Connection: close
< 
{
    "todo1": {
        "task": "build an API"
    },
    "todo2": {
        "task": "?????"
    },
    "todo3": {
        "task": "profit!"
    }
}
* Closing connection 0
```
