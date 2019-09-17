# **KI Labs Challenge Calendar API**

This is an API that has the purpose of enabling an interviewer or candidate to know when a meeting can be arranged, between one candidate and one or more interviewers

### Candidate / Interviewer
   * [Create a profile](#signup)
   * [Add available hours to the schedule](#addavailablehours)
   * [Get the profile by the identifier](#getbyid)
   * [Get all profiles](#allprofiles)
   * [Delete profile](#deleteprofile)

### Interview
   * [Obtain the interview possible schedule](#interviewpossibleschedule)

### Exceptions
   * [Invalid Parameter Exception](#invalidparameterexception)
   * [Body Wrong Or Missing Exception](#bodywrongormissingexception)
   * [Entity Not Found Exception](#entitynotfoundexception)


# Candidates / Interviewers

The actions to perform are the exact same, the only thing that differs from candidate to interviewers is the URI, which will be discriminated in the URI section, considering that the candidates' URI always comes first, making the interviewers' come second.

## <a name='signup'></a> Create a profile

Create a profile for either a candidate or interviewer.
        
 * **HTTP Method** to be used: **_POST_**
 * **_Hostname_**: _localhost_
 * **Port**: 8080
 * **_URI_**: **api/candidates || api/interviewers**
 * **_Headers_**: Content-Type: application/json
 * **_Body_**: 

```json
{
    "name" : "Carl",
    "availableHours":["2019-09-16T09:00:00","2019-09-17T09:00:00","2019-09-18T09:00:00"] (Optional)
}
```
### Returned response for the **_status code_**:

 * 201 **CREATED**, with the response in **_JSON_**
```json
{
    "id": 1,
    "name": "Carl",
    "availableHours": [
        "2019-09-20 09:00",
        "2019-09-17 09:00",
        "2019-09-16 09:00",
        "2019-09-18 10:00",
        "2019-09-19 09:00",
        "2019-09-18 09:00",
        "2019-09-18 11:00"
    ]
}
```
* 422 **_Unprocessable Entity_**, when the provided dates either do not start at 0 minutes and 0 seconds or are before the current date and time, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#invalidparameter",
    "title": "Invalid Parameter",
    "detail": "The provided dates must start at 0 minutes and 0 seconds.",
    "status": 422
}
```
* 400 **_Bad Request_**, when the body is either wrongly formed or missing, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#requestbodywrongormissingexception",
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```

## <a name='addavailablehours'></a> Add available hours to the schedule

Replaces the current available hours stored in the database.
        
 * **HTTP Method** to be used: **_PUT_**
 * **_Hostname_**: _localhost_
 * **Port**: 8080
 * **_URI_**: **api/candidates/\<cid> || api/interviewers/\<intid>** - **cid** : candidate identifier **||** **intid** : interviewer identifier
 * **_Headers_**: Content-Type: application/json
 * **_Body_**: 

```json
{
    "availableHours":["2019-09-23T09:00:00"]
}
```

### Returned response for the **_status code_**:

 * 200 **OK**, with the response in **_JSON_**
```json
{
    "id": 1,
    "name": "Carl",
    "availableHours": [
        "2019-09-20 09:00",
        "2019-09-23 09:00",
        "2019-09-17 09:00",
        "2019-09-16 09:00",
        "2019-09-18 10:00",
        "2019-09-19 09:00",
        "2019-09-18 09:00",
        "2019-09-18 11:00"
    ]
}
```
* 422 **_Unprocessable Entity_**, when the provided dates either do not start at 0 minutes and 0 seconds or are before the current date and time, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#invalidparameter",
    "title": "Invalid Parameter",
    "detail": "The provided dates must start at 0 minutes and 0 seconds.",
    "status": 422
}
```

* 404 **_Not Found_**, when the candidate / interviewer you are looking for does not exist, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#entitynotfoundexception",
    "title": "Entity Not Found",
    "detail": "The entity you are looking for does not exist.",
    "status": 404
}
```

* 400 **_Bad Request_**, when the body is either wrongly formed or missing, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#requestbodywrongormissingexception",
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```

## <a name='getbyid'></a> Get the profile by the identifier

Get the candidate / interviewer by id.
        
 * **HTTP Method** to be used: **_GET_**
 * **_Hostname_**: _localhost_
 * **Port**: 8080
 * **_URI_**: **api/candidates/\<cid> || api/interviewers/\<intid>** - **cid** : candidate identifier **||** **intid** : interviewer identifier

 ### Returned response for the **_status code_**:

 * 200 **OK**, with the response in **_JSON_**
```json
{
    "id": 1,
    "name": "Carl",
    "availableHours": [
        "2019-09-20 09:00",
        "2019-09-23 09:00",
        "2019-09-17 09:00",
        "2019-09-16 09:00",
        "2019-09-18 10:00",
        "2019-09-19 09:00",
        "2019-09-18 09:00",
        "2019-09-18 11:00"
    ]
}
```

* 404 **_Not Found_**, when the candidate / interviewer you are looking for does not exist, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#entitynotfoundexception",
    "title": "Entity Not Found",
    "detail": "The entity you are looking for does not exist.",
    "status": 404
}
```

## <a name='allprofiles'></a> Get all profiles

Get all the candidates / interviewers, with only the necessary information to identify them.
        
 * **HTTP Method** to be used: **_GET_**
 * **_Hostname_**: _localhost_
 * **Port**: 8080
 * **_URI_**: **api/candidates || api/interviewers**

 ### Returned response for the **_status code_**:

 * 200 **OK**, with the response in **_JSON_**
```json
[
    {
        "id": 1,
        "name": "Carl"
    },
    {
        "id": 3,
        "name": "Bob"
    }
]
```

## <a name='deleteprofile'></a> Delete profile

Delete a candidate / interviewer profile.
        
 * **HTTP Method** to be used: **_DELETE_**
 * **_Hostname_**: _localhost_
 * **Port**: 8080
 * **_URI_**: **api/candidates/\<cid> || api/interviewers/\<intid>** - **cid** : candidate identifier **||** **intid** : interviewer identifier

 ### Returned response for the **_status code_**:

 * 204 **_No Content_**, when eveyrhing goes according to plan

 * 404 **_Not Found_**, when the candidate / interviewer you are looking for does not exist, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#entitynotfoundexception",
    "title": "Entity Not Found",
    "detail": "The entity you are looking for does not exist.",
    "status": 404
}
```


# Interview

## <a name='interviewpossibleschedule'></a> Obtain the interview possible schedule

    The possible hours for a candidate to have an interview with the given interviewer(s).

  * **HTTP Method** to be used: **_GET_**
  * **_Hostname_**: _localhost_
  * **Port**: 8080
  * **_URI_**: **api/interview**
  * **_Parameters_**: 
    * candidateId - Identifier (*Long*) 
    * interviewersId - Identifiers (*Array\<Long>*) 


 ### Returned response for the **_status code_**:

 * 200 **OK**, with the response in **_JSON_**
```json
{
    "candidate": {
        "id": 1,
        "name": "Carl"
    },
    "interviewers": [
        {
            "id": 1,
            "name": "Inês"
        },
        {
            "id": 2,
            "name": "Ingrid"
        }
    ],
    "availableHours": [
        "2019-09-17 09:00",
        "2019-09-19 09:00"
    ]
}
```

 * 404 **_Not Found_**, when the candidate / interviewers you are looking for does not exist, with the error response in **_Problem+JSON_**:
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#entitynotfoundexception",
    "title": "Entity Not Found",
    "detail": "The entity you are looking for does not exist.",
    "status": 404
}
```


# Exceptions

## <a name='invalidparameterexception'></a> *Invalid Parameter Exception*

When the provided dates either do not start at 0 minutes and 0 seconds or are before the current date and time.
```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#invalidparameterexception",
    "title": "Invalid Parameter",
    "detail": "The provided dates must start at 0 minutes and 0 seconds.",
    "status": 422
}
```

## <a name='bodywrongormissingexception'></a> *Body Wrong Or Missing Exception*

When the body is either wrongly formed or missing.

```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#bodywrongormissingexception",
    "title": "Request Body Wrong Or Missing",
    "detail": "Impossible to proceed with the request because body wrongly formed or missing.",
    "status": 400
}
```

## <a name='entitynotfoundexception'></a> *Entity Not Found Exception*

When the entity does not exist.

```json
{
    "type": "https://rafaelbatalha.github.io/KI-Labs-Challenge-Documentation/#entitynotfoundexception",
    "title": "Entity Not Found",
    "detail": "The entity you are looking for does not exist.",
    "status": 404
}
```
