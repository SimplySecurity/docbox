## SimplyX

This is our high-quality API. You can use this API to request
domain and email data. to signup go-to https://simplyx.io/login

### API Codes & Errors

In many cases the API follows the HTTP status code IETF standards close as possible with the RFC. In some cases where the request was valid but no response custom REST codes are used.

All successful (200 OK) Reponse objects with **NO** error will use the following:
#### Successful request
```json
{
    "error": false,
    "response_data": {
        .....
    } 
}
```

All successful (200 OK)  Reponse objects **WITH** errors will use the following:
#### Successful request With Errors
```json
{
    "error": true,
    "status": "Custom Error Message Here"
}
```

All failed (4xx) Reponse objects with errors will use the following:
#### Malformed request
```json
{
    "error": false,
    "message": "Custom Error Message Here"
}
```

2xx Success

HTTP Code | Description 
---|---
200 | The request has succeeded. The information returned with the response is dependent on the method used in the request.
201 | The request has been fulfilled and resulted in a new resource being created.

4xx Client Error 

HTTP Code | Description 
---|---
400 | The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications.
401 | The request requires user authentication. The response MUST include a Authenticate header field with proper JWT.


### Login Authentication 

Login to obtain JWT (JSON Web Token) for proper authentication headers.

**ALL** requests require the following JSON properties:

Property | Description
---|---
`email` | (required) the user email used to sign up for API
`password` | (required) the password used to auth email

```endpoint
GET /v1/login wobbles:read
```

#### Example request

```curl
$ curl -X GET -H "Content-Type: application/json" -d \ 
    '{"email": "example.email@gmail.com", "password": "password"}' \
    https://simplyx.io/v1/login
```

#### Example response

```json
{
    "email": "example.example@gmail.com",
    "token": "eyJ0eX.....luDg"
}
```

### Register User 

Register new user using a timed activation code. This API route emails users email a JWT (JSON Web Token) with 5 day expiration date.  

**ALL** emails can only be registred once.

Property | Description
---|---
`full_name` | (required) the user email used to sign up for API
`email_address` | (required) user email used as login / username
`phone_number` | (required) for future two factor auth
`job_type` | (required) type of job user / roll
`password` | (required) the password used to auth email

Job Types |
---|
`IT` | 
`DevOps` | 
`Cyber_Security` | 
`Sales` | 
`Other` | 

```endpoint
POST /v1/register
```

#### Example request

```curl
$ curl -X POST -H "Content-Type: application/json" -d \ 
    '{"email": "example.example@gmail.com", "password": "password"}' \
    https://simplyx.io/v1/register
```

#### Example response

```json
{
    "email": "example.example@gmail.com",
}
```

### Activate User 

Register new user using a timed activation code. This API route takes emails users JWT (JSON Web Token) obtained via SMTP.

**ALL** emails can only be registred once, but you can activate a user multiple times by calling 'https://simplyx.io/v1/register'

Property | Description
---|---
`activation_code` | (required) the user activation JWT 



```endpoint
GET /v1/activation
```

#### Example request

```curl
$ curl -X GET -H "Content-Type: application/json" \
    -d '{"activation_code": "..._XDJ3Qb66qpBmopIUGUMs0mjnSWOri555TzyOwviY1s"}' \ 
    https://simplyx.io/v1/activate
```

#### Example response

```json
{
    "email": "example.example@gmail.com",
}
```

### Query Domain 

Query for Domain via a known domain. This API returns entire data set in BETA, it will allow future calls to reduce load.

***ALL*** request ***MUST*** contain 'Authorization: Bearer' JWT token to reach end-point.

Header Name | Header Value | Description
---|---|---
`Authorization:` | `Bearer SDcsd..sSD` |(required) the user activation JWT 

Opt settings to configure return data:

Property | Description | Default | Valid Value 
---|---|---|---|
`skip` | (opt) to configure how many records to skip from original start limit ex ( limit: 100: skip: 500 = Email records 500-600) | 0 | Unlimited
`limit` | (opt) to configure the ammount of records returned in one query | 5 | 1-100



```endpoint
GET /v1/domain
```

#### Example request

```curl
$ curl -X GET -H "Authorization: Bearer XDJ3Qb....66qpBmo" \
    -H "Content-Type: application/json" -d '{"domain": "domain.com"}' \
    https://simplyx.io/v1/domain
```

#### Example response

```json
{
    "domain": "slack.com",
    "active": true,
    "last_update": 1525850454.678481,
    "email_pattern": "",
    "email_count": 3,
    "emails": [
        {
            "email_address": "feedback@slack.com",
            "first_name": "",
            "last_name": "",
            "name_generated_email": false,
            "verified": false,
            "giberish": false,
            "first_seen": 1525850454.688317,
            "last_seen": 1525850454.68832,
            "sources": [
                "https://slack.com/brand-guidelines"
            ]
        },
        .......
        {
            "email_address": "legal@slack.com",
            "first_name": "",
            "last_name": "",
            "name_generated_email": false,
            "verified": false,
            "giberish": false,
            "first_seen": 1525850454.695294,
            "last_seen": 1525850454.695297,
            "sources": [
                "https://slack.com/terms"
            ]
        }
    ],
    "subdomains": [],
    "webmail": false,
    "mx_record": {
        "Status": 0,
        "TC": false,
        "RD": true,
        "RA": true,
        "AD": false,
        "CD": false,
        "Question": [
            {
                "name": "slack.com.",
                "type": 255
            }
        ],
        "Answer": [
            {
                "name": "slack.com.",
                "type": 1,
                "TTL": 59,
                "data": "13.32.184.100"
            },
            ...........
        ],
        "Comment": "Response from 205.251.197.213."
    },
    "allows_verify": false
}
```





