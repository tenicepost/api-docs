## API Authentication and authorization

#### [Intro](#id-intro)

[Pre conditions](#id-pre)

1. [Steps](#id-steps)<br />
  1.1.  [Get your user-id and key](#id-steps1)<br />
  1.2.  [Get your authorisation token](#id-steps2)<br />
2.  [Environment and testing](#id-env)<br />
  2.1. [Environments](#id-env-tst)<br />
    2.1.1. [Examples - request code](#id-exmp)<br />
    2.1.2. [Response codes](#id-res)<br />
  2.2. [Testing](#id-test)

--------

#### Intro<a id="id-intro"></a>
The API for authentication and authorization provides you the authentication key which enables the use of all the DHL-endpoints that are secured.

The DHL APIs implements the JWT for secure authentication and authorization. This JWT token is to be provided in the Authorization header using the bearer scheme.
In the  [API docs ](https://api-gw.dhlparcel.nl/docs/#/) a lock <span>&#x1F512;</span> is showen on the right hand side of the endpoint to identify a secured endpoint.

The api-gateway checks performs the following checks on the token for secured services:

- Signature is correct(using HMAC SHA-256 symmetric key).
- Expiration timestamp(exp claim) lies in the future.
- Not before timestamp(nbf claim) isn’t in the future.
- Calling an unsecured service via the gateway.

#### Pre condition<a id="id-pre"></a>
- Business account on MY DHLParcel. Accounts are handed out by the sales department __<email@cim>?__

#### 1. Steps <a id="id-steps"></a>

##### 1.1 Get your user-id and key <a id="id-steps1"></a>

To get an authentication token, you will need the user_id and user key for your application, these unique values can be generated on the  [My DHL-Parcel Application page](https://my.dhlparcel.nl).

User-id and key are generated in the DHL-Parcel application by selecting 'Settings' in the user dropdown menu and clicking on the button 'CREATE API KEY' in the tab 'API KEYS'.

![](./images/instellingen.jpg)

When 'CREATE API KEY' is pressed the user id and key will be generated.

![](./images/generated.jpg)

##### 1.2 Get your authorisation token <a id="id-steps2"></a>

The user-id and key are used to make the initial call to the [API key](https://api-gw.dhlparcel.nl/authenticate/api-key) endpoint. As a reponse the API gives an access token and a refresh token.

The validity of the access token is about 15 minutes so with the refresh token the session can be prolonged for another 15 minutes using the [refresh API](https://api-gw.dhlparcel.nl/authenticate/refresh-token). The reponse is the same as the initial call for the API key, an access and refresh token.

Example of response body (200):
The yellow shaded text is the token to be used in the HTTP header for authentication.
{
  "accessToken": "
```diff
- eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIwOGZkMTRhMC05YWYzLTRkZDMtODU3YS1hNzUyYWY3ZjUyYTciLCJzdWIiOiI2YmZjNzU2MS00YjQxLTRjYjctYTFjMy1kYzMxZmFmODYwZGIiLCJvcmdhbml6YXRpb25JZCI6ImE0MDkzM2RlLWJkMTctNDk0NC1iN2U5LTIzZmM3ZWU5YzgzNSIsIm5iZiI6MTUxMDMyNDMxNSwiZXhwIjoxNTEwMzI1MjE2LCJyb2xlcyI6WyJsYWJlbC1zZXJ2aWNlLkIyWCIsInBpY2t1cC1zZXJ2aWNlLkIyWCJdLCJhY2NvdW50cyI6WyIwODUwMDAwMSJdfQ.D9Zf0hnDXhPXoWar42wzSiZHRKLBYriyKQyj1zERrBw
```
",<br />
  "accessTokenExpiration": 1510325216,<br />
  "refreshToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiJhMWQ5MDMyMi04YmRiLTQ1NjQtOTMxMy04OTg5NThmMzgwNDQiLCJzdWIiOiI2YmZjNzU2MS00YjQxLTRjYjctYTFjMy1kYzMxZmFmODYwZGIiLCJvcmdhbml6YXRpb25JZCI6ImE0MDkzM2RlLWJkMTctNDk0NC1iN2U5LTIzZmM3ZWU5YzgzNSIsIm5iZiI6MTUxMDMyNDMxNSwiZXhwIjoxNTEwOTI5MTE2LCJyb2xlcyI6WyJhdXRoLXNlcnZpY2UuUkVGUkVTSCJdLCJhY2NvdW50cyI6WyIwODUwMDAwMSJdfQ.fi7hn6u3mFwcJ4AG8cYEh8OJFm2NDwOt407aP7sENSo",<br />
  "refreshTokenExpiration": 1510929116
}

#### 2 Environment and testing<a id="id-env-tst"></a>

#### 2.1 Enviroments <a id="id-env"></a>

Description |  Test  |  Production
----------------- | -------------------- | -------------------------
API to get your tokens| https://api-gw-accept.dhlparcel.nl/authenticate/api-key | https://api-gw.dhlparcel.nl/authenticate/api-key
 API to be used with the refresh token|https://api-gw-accept.dhlparcel.nl/authenticate/refresh-token| https://api-gw.dhlparcel.nl/authenticate/refresh-token

##### 2.1.1 Examples <a id="id-exmp"></a>

***cURL***
##### Access token:
```
curl -X POST "https://api-gw.dhlparcel.nl/authenticate/api-key" -H "accept: application/json" -H "content-type: application/json" -d "{ \"userId\": \"6bfc7561-4b41-4cb7-a1c3-dc31faf860db\", \"key\": \"8babb742-1d75-4014-bb82-aa3f459aa4b3\"}
```

##### Refresh token:
```
curl -X POST "https://api-gw.dhlparcel.nl/authenticate/refresh-token" -H "accept: application/json" -H "content-type: application/json" -d "{ \"refreshToken\": \"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIwMWVlYjBkMy1hZmM0LTRiYjEtYTgzZS0wZDkxYzE4ZjVhZDUiLCJzdWIiOiI2YmZjNzU2MS00YjQxLTRjYjctYTFjMy1kYzMxZmFmODYwZGIiLCJvcmdhbml6YXRpb25JZCI6ImE0MDkzM2RlLWJkMTctNDk0NC1iN2U5LTIzZmM3ZWU5YzgzNSIsIm5iZiI6MTUxMDIxMDcxNSwiZXhwIjoxNTEwODE1NTE2LCJyb2xlcyI6WyJhdXRoLXNlcnZpY2UuUkVGUkVTSCJdLCJhY2NvdW50cyI6WyIwODUwMDAwMSJdfQ.y-Hm3T6_moUWXF_v2uyOXcX7tu1uddyhG6fsDQliPXw\"}
```

***PHP***
```php
<?php
$auth_string = '{"userId":"{userID}","key":"{key}"}';
$ch = curl_init('https://api-gw.dhlparcel.nl/authenticate/api-key');  <span style="color:red"> **//test environment**</span>
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "POST");
curl_setopt($ch, CURLOPT_POSTFIELDS, $auth_string);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, array('Content-Type: application/json','Accept: application/json'));
$auth_response = curl_exec($ch);
$api_key = json_decode($auth_response);
$accessToken = $api_key->{'accessToken'};
?>
```

***Javascript*** ***using AJAX***
```Javascript
       $.ajax ({
          type:"POST",
          data: JSON.stringify({userId: "user-id", key: "key"}),
          url: AUTH_URL, //api url
          async: true,
          crossDomain:true,
          contentType: "application/json",
          accept: "application/json",
          jsonpCallback: "logResults", //defined function error handling
          success: function(parsed_json) {
              console.log(parsed_json.accessToken + ", " + parsed_json.refreshToken);
          }
        });
```

##### 2.1.2 Responses codes <a id="id-res"></a>
Response 200:

```
  { "accessToken": "_Your API key_"
    "accessTokenExpiration": 1510138597,
    "refreshToken":  "_Your refresh API key _"
    "refreshTokenExpiration": 1510742497 }
```

Response 400:
>	userId or key not provided

Response 401:
>	Invalid key

#### 2.2 Testing <a id="id-test"></a>

Suggested Test Cases 	| Expected Result
--------------------------  |---------------------------
Create an authorization token (initial request) | An access token and a refresh token with expiration are returned by the My DHLParcel application.
Create an new authorization with refreh token| An access token and a refresh token with expiration are returned by the My DHLParcel application.
