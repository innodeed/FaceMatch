# Face Match Rest API v1 - Documentation

# Overview
This documentation describes the Face Match API v1.

# Introduction
FaceMatch API compares two faces and determines if the images are of the 
same person. It matches the face regardless of expression, facial hair and age.

# Table Of Contents
* Face Match API - Documentation
* Overview
* Introduction
* Schema
* Endpoint
* Authentication
* Media Types
* Sample Code
* API Call Structure
* Response Structure

# Schema
Currently, HTTP and HTTPS are supported. All data is received as JSON, and all image uploads are to be performed as form-data (POST request). It is recommended that HTTPS is used for all API calls. For HTTPS, only TLS v1.2 is supported to ensure better security.

# Endpoint
`https://{endpoint}/v1/api/facematch`

# Authentication
* API keys, an appId, appKey combination is passed in the request header, which will be provided by Innodeed. API requests without API key or invalid credentials will fail.
* On requesting with invalid credentials the HTTP response would have a status code : 401
* Be sure to keep your API key secure. Do not share them in publicly accessible areas like client-side code.

# Supported Media Types

Use the following tips to ensure that your input images give the most accurate recognition results:
* FaceMatch API currently supports  JPEG and PNG images.
* Image file size should be no larger than 3 MB.
* Ensure that face in a selfie covers almost 70% of the image for better results.
* Some faces might not be recognized because of technical challenges, such as:
- Images with extreme lighting, for example, severe backlighting.
- Obstructions that block one or both eyes.


# Sample Code

```
curl --location 
--request POST ‘https://{endpoint}/v1/api/facematch’ \
--header ‘Authorization: Basic <Base64 encoded appid:appKey>’ \
--form ‘id=@/ path_to_kyc_document.jpg’ \
--form ‘selfie=@/path_to_Selfie_image.jpg’
```

# HTTP response Code

* FaceMatch API uses conventional Http response code to indicate success or failure of an API request.
* Codes in 2xx range indicate success.
* Codes in 4xx range indicate an error that failed given the information provided.
* Codes in 5xx range indicate an error with server (these are rare)

# Http Status Code Summary

| Code  | Summary |
| ------------- | ------------- |
| 200 | OK |
| 400,422  | Bad Request. The request was unacceptable, often due to missing or bad data  |
| 401   | Unauthorized    Invalid or missing API key |
| 500, 502 | Server error





# API Call Structure

The API call is used to determine if 2 face images belong to the same person

```
Endpoint https://{endpoint}/v1/api/facematch 
Method: POST
Header
content-type : ‘formdata’
Authorization: Basic <Base64 encoded appid:appKey>
```
## Request Body
To match user photo with the face in an ID card:
``
id- jpeg, png
selfie - jpeg, png
``


# Response Structure

## Success Response: In case of successful API call and face match operation, the response with following format will be sent:
```
{
    "status" : "success",
    "statusCode" : "200",
    "facematch" : {
      "match" : <"yes"/"no">,
      "score" : <0-100>
      "need-to-review": <“yes"/"no">,
    },
    "liveness" : {
      "spoof" : <"yes"/"no">
    }
}
 ```
    
## Error Response:
### Face detection failed:
### Face not detected in ‘Selfie’
```
{
    "status": "failure",
    "statusCode": "400",
    "message": "Face not detected in selfie"
}
```

### Face not detected in ‘Document ID’
```
{           
   "status": "failure",
   "statusCode": "400",
   "message": "Face not detected in document"
}
```

### Face not detected in both ‘Selfie’ and ‘Document ID’ 
```
{           
    "status": "failure",
    "statusCode": "400",
    "message": "Face not detected"       
}
```

### Blur image detected in ‘Selfie’
```
{           
    "status": "failure",
    "statusCode": "422",
    "message": “Blur selfie”    
}
```

### In case of ‘missing/invalid’ API key
```
{           
    "status": "failure",
    "statusCode": "401",
    "message": “Access denied due to invalid key. Make sure you provide the right key”
}
```

Server Errors We ensure high availability, but in case if they do, status code will be 5xx.
Form parse error or bad form-data. If the following happens, please make sure the form data is correctly set.
 ``` 
  {
     "status": "failure",
     "statusCode": "500",
     "message": "upload error"
  }
 ``` 
All error messages follow the same syntax with the statusCode, status and message(string) being part of the body.
