# restapi


### Project structure 
```
restapi
│
├── app.js
├── package.json
├── config
│    ├── conf.json [local config file, should not git this]
│    └── database.js
├── public
│    ├── css
│    └── js
└── app
    ├── config
    │    └── passport
    │        └── passport.js
    ├── controllers
    │    ├── auth_controller.js
    │    └── v1_0
    │        └── messages_controller.js
    │── models
    │    ├── index.js
    │    ├── messages.js
    │    └── user.js
    │── routes
    │    ├── auth_routes.js
    │    └── v1_0
    │        └── messages_routes.js
    │── tools
    │    └── helper_methods.js
    └── views
         ├── body_html [folder]
         ├── body_js [folder]
         ├── header [folder]
         ├── pages [folder]
         └── index.ejs
```


## v1 API documentation

### NOTE: 
  * HTTP Status Codes will always be `200 OK`
  * However, the status code and error status can be found here:
  * **Note here:**
        * `"error": true` for important error.

        ```json
        {     
            'meta': {
              'error': true,
              'code': 404,
              'msg': 'Error: Can not get Messages!'
            } 
        }
        ```

 **GET Messages**
----
  Returns json data about received messages

* **URL**

  `/api/v1/messages`

* **Method:**

  `GET`

* **Authentication:**

  None
  
* **Search Params**

  `?limit=2&order=ASC&offset=15`
  * limit (positive int) - max returned records
  * offset (positive int) - skip first xxx records
  * order (ASC or DESC)

* **Error Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 404 <br />
    ```json
        {     
            "meta": {
                "error": true,
                "code": 404,
                "total": 0,
                "applied_filter": {},
                "msg": "Error: Can not get Messages!"
            } 
        }
    ```

* **Success Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 200 <br />
  ```json
    {
        "meta": {
            "error": false,
            "code": 200,
            "total": 2,
            "applied_filter": {
                "limit": 2,
                "order": "ASC",
                "offset": 15
            },
            "msg": "ok"
        },
        "data": [
            {
                "id": 19,
                "user_id": 1,
                "content": "OK = \"</script><script>alert('W');</script>\"",
                "createdAt": "2018-09-30T00:38:23.000Z",
                "palindrome": null,
                "User": {
                    "first_name": "dalin",
                    "last_name": "huang"
                }
            },
            {
                "id": 22,
                "user_id": 1,
                "content": "abcba",
                "createdAt": "2018-09-30T01:38:03.000Z",
                "palindrome": true,
                "User": {
                    "first_name": "dalin",
                    "last_name": "huang"
                }
            }
        ]
    }
  ```



 **GET One Message**
----
  Returns json data about one message

* **URL**

  `/api/v1/messages/:id`

* **Method:**

  `GET`

* **Authentication:**

  None

* **URL Params**

  **Required:**
  `id=[integer]`

* **Error Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 404 <br />
    ```json
        {
            "meta": {
                "error": false,
                "code": 404,
                "total": 0,
                "applied_filter": {},
                "msg": "Message does not exsits"
            },
            "data": null
        }
    ```

* **Success Response:**

  * **Code:** 200 <br />
  ```json
    {
        "meta": {
            "error": false,
            "code": 200,
            "total": 1,
            "applied_filter": {},
            "msg": "OK"
        },
        "data": {
            "id": 23,
            "user_id": 1,
            "content": "ab😌😌😌cba",
            "createdAt": "2018-09-30T01:38:22.000Z",
            "palindrome": true,
            "User": {
                "first_name": "dalin",
                "last_name": "huang"
            }
        }
    }
  ```




 **POST Message**
----
  Returns json data about created message

* **URL**

  `/api/v1/messages/:id`

* **Method:**

  `POST`
  **Required Header:**

  `Authorization : s%3A2HnZqBwicujI7LdI7ESMUb8...`
  `Content-Type  : application/json`
  
  **Required Payload:**
    ```json
        {
            "data": {
                "message": "This msg looks <*&(&%$-=_^#@@!> ok?"
            }
        }
    ```
* **Authentication:**

  `Required login`

* **URL Params**

  **Required:**
  `id=[integer]`

* **Error Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 404 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 404,
            "total": 0,
            "applied_filter": {},
            "msg": "Message does not exsits"
        },
        "data": null
    }
    ```

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 401 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 401,
            "total": 0,
            "applied_filter": {},
            "msg": "Unauthorized: login is required"
        },
        "data": null
    }
    ```

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 400 <br />
  * **Note here:**
        * `"error": true` because the payload is wrong, 
        * Therefore an important error
    ```json
    {
        "meta": {
            "error": true, // flag indicate how serious is the error
            "code": 400,
            "msg": "Error: bad request, check your payload or URL!"
        }
    }
    ```

* **Success Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 201 <br />
  ```json
    {
        "meta": {
            "error": false,
            "code": 201,
            "total": 1,
            "applied_filter": {},
            "msg": "Created"
        },
        "data": {
            "status": 1,
            "id": 46,
            "user_id": 1,
            "content": "This msg looks <*&(&%$-=_^#@@!> ok?",
            "palindrome": false,
            "updatedAt": "2018-10-01T00:11:12.467Z",
            "createdAt": "2018-10-01T00:11:12.467Z"
        }
    }
  ```




 **DELETE One Message**
----
  Returns json data about result of delete one message

* **URL**

  `/api/v1/messages/:id`

* **Method:**

  `DELETE`
  **Required Header:**

  `Authorization : s%3A2HnZqBwicujI7LdI7ESMUb8...`
  
* **Authentication:**

  `Login required`
  `Is owner of the message required`

* **URL Params**

  **Required:**
  `id=[integer]`

* **Error Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 400 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 400,
            "total": 0,
            "applied_filter": {},
            "msg": "Error: Message id should be interger and greater than 0"
        },
        "data": null
    }
    ```

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 404 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 404,
            "total": 0,
            "applied_filter": {},
            "msg": "Message dos not exsits"
        },
        "data": null
    }
    ```

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 401 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 401,
            "total": 0,
            "applied_filter": {},
            "msg": "Unauthorized: login is required"
        },
        "data": null
    }
    ```

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 401 <br />
    ```json
    {
        "meta": {
            "error": false,
            "code": 401,
            "total": 0,
            "applied_filter": {},
            "msg": "Unauthorized"
        },
        "data": null
    }
    ```

* **Success Response:**

  * **HTTP Status Codes:** 200 <br />
  * **Meta - Codes:** 204 <br />
  ```json
    {
        "meta": {
            "error": false,
            "code": 204,
            "total": 1,
            "applied_filter": {},
            "msg": "The message was successfully deleted"
        },
        "data": {
            "id": 47,
            "user_id": 1,
            "content": "This msg looks <*&(&%$-=_^#@@!> ok?",
            "createdAt": "2018-10-01T00:16:37.000Z",
            "palindrome": false,
            "User": {
                "first_name": "dalin",
                "last_name": "huang"
            },
            "status": 0,
            "updatedAt": "2018-10-01T00:40:09.102Z"
        }
    }
  ```





