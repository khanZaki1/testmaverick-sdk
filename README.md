
<p align="center"><img src="images/logo.png" width="20%" /></p>
<h1 align="center">TestMaverick SDk</h1>

## Revision History

<table>
<thead>
<tr>
<th>Date</th>
<th>Version No</th>
<th>Author</th>
</tr>
</thead>
<tbody>
<tr>
<td>22<sup>nd</sup> September, 2023</td>
<td>1.0</td>
<td>Zeus</td>
</tr>
<tr>
<td>24<sup>th</sup> September, 2023</td>
<td>1.1</td>
<td>Zeus</td>
</tr>
<tr>
<td>20<sup>th</sup> October, 2023</td>
<td>1.2</td>
<td>Zeus</td>
</tr>
</table>

## Table of Content

<ol>
<li> <a href="#introduction">Introduction</a> </li>
<li> <a href="#overview">Overview</a> </li>
<li> <a href="#authentication-and-security">Authentication and Security</a> </li>
<li> 
<a href="#revision-history">Auto Proctoring SDK</a> 
<ol type="a">
<li> <a href="#revision-history">Overview</a></li>
<li> <a href="#revision-history">Sequence Diagram</a></li>
<li> <a href="#revision-history">Getting Started</a></li>
<li> <a href="#revision-history">Public Methods</a></li>
<li> <a href="#revision-history">Events</a></li>
<li> <a href="#revision-history">Integration of System Check</a></li>
<li> <a href="#revision-history">Integration of Verification Steps</a></li>
</ol>
</li>
<li> 
<a href="#revision-history">Auto Proctoring Report SDK</a> 
<ol type="a">
<li> <a href="#revision-history">Overview</a></li>
<li> <a href="#revision-history">Sequence Diagram</a></li>
<li> <a href="#revision-history">Getting Started</a></li>
<li> <a href="#revision-history">Public Methods</a></li>
<li> <a href="#revision-history">Integration of Report SDK</a></li>
</ol>
</li>
<li> <a href="#revision-history">Error Handling</a> </li>
<li> <a href="#revision-history">FAQs</a> </li>
</ol>

## Introduction

<p>
The TestMaverick SDK offers a cutting-edge solution for effortless integration of AI-based proctoring into your LMS platforms. 
</p>

<p>
With its seamless and user-friendly approach, this software development kit empowers institutions and businesses to enhance the integrity of their assessments by leveraging advanced artificial intelligence technology. 
</p>

<p>
Whether you're an educational institution, a certification body, or a business seeking to maintain the highest standards of exam security, TestMaverick SDK provides a reliable and efficient means to ensure fair and transparent testing environments.
</p>

<p>
TestMaverick SDK provides following features:
</p>

<ul>
<li>Auto Proctoring</li>
<li>Proctoring Report</li>
<li>Manual Proctoring</li>
<li>Test player</li>
<li>Smart Test evaluation using ChatGPT</li>
<li>Plagiarism Check</li>
</ul>

<p>This document covers Auto Proctoring and Auto Proctoring Report SDK features.</p>

## Overview

<p>
Below is a top level architectural overview for integration of TestMaverick SDK with Consumer’s existing system. The Consumer system will integrate TestMavericks client SDK which will communicate with the Consumer system for data through events trigger and callbacks. Also TestMaverick SDK provides various public methods which can be accessed by consumers.
</p>

<p>
The TestMaverick SDK will store all the Proctoring related information on TestMaverick Server however it will not  store any PII or sensitive information of the user on our server. TestMaverick  SDK solely store statistical data related to auto proctoring (i.e Face, audio and browser related violation counts) along with few non-identifiable GUIDs like UserGUID, TestGUID and AttemptGUID, ensuring that no personal information is stored on our end. These identifiers, which are not directly linked to personal information, are essential for keeping the data mapping within the TestMaverick system so that when the TestMaverick Report SDK is loaded in your system, using these non-identifiable GUIDs it will accurately retrieve statistical information mapped to it from TestMaverick server.
</p>

<p align="center">
<img src="images/overview.png" alt="Overview arch" />
</p>

## Authentication and Security

<p>
TestMaverick SDK provides a signature based authentication and authorization system to integrate with various existing applications to ensure the security of our APIs. 
</p>
<p>
Following are the steps to be followed by Consumer to integrate SDK with the existing system.
</p>

<ol>
<li>

### Registration and Configuration
<p>
To use the TestMaverick SDK, consumers must register with our service. Upon registering, they will receive a 'ClientID' and a 'Secret Key'. This set of secret keys needs to be kept confidential.</p>
</li>
<li>

### Add Signature API
<ul>
<li>
<p>
Consumers must provide an API which can generate the signed object in the following output format.<br/>
</p>

```json
    {
        "security": {
            "consumerKey": "xxxxxxxx",
            "domain": "xxxxxxxx",
            "timestamp": "xxxxxx",
            "signature": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "remoteIpAddress": "xxx.xxx.xxx.xxx"
            },
        "request": {
        "mode": "xxxx",
        "user": {
            "id": "xxxxx",
            "firstName": "xxxxx",
            "lastName": "xxxxx",
            "email": "xxxxx",
        },
        "meta": "xxxxx"
        },
        "userRequest": {
        "ClientUserGUID": "xxxx",
        "OrganizationID": "xxxxx",
        "ClearanceLevel": "xxxxx"
        }
    }
```
</li>
<li>To create the above signed object, consumers can use TestMaverick.SDK.Security.dll provided by the TestMaverick vendor. </li>
<li>For .Net core applications following changes can be done to create an API for signed requests.</li>

<li>
<p>Consumer should add reference of dll to their project<br/></p>

```javascript
<ItemGroup>
  <Reference Include="TestMaverick.SDK.Security">
    <HintPath>".\Lib\TestMaverick.SDK.Security.dll</HintPath>
  </Reference>
</ItemGroup>
```
</li>

<li>
<p>Create a new web API in your application which will be used by Testmaverick SDK to generate a signed request. This web API endpoint URL will be provided in the config while initializing the SDK. The definition of the Signature API is given below for your reference.
</p>
<ul type="square">
<li>Type : POST</li>
<li>Request Param : [FromBody] object config</li>
<li>
<p>Sample Code: <br/></p>

```c#
public async Task<ActionResult> GetSdkSignedRequestObject([FromBody] object config)
        {


            // Add your logic to call a service which will create a signature for a given request object.
            // Object signedRequest = GetSignedReq();   // This is sample call
            // Then return API response in following data structure

            Object responseData = new Object();
            responseData.responseObject = signedRequest;
            responseData.statusCode = 0;

            ContentResult contentResult = new ContentResult();
            contentResult.ContentType = "application/json";
            contentResult.Content =  
            JsonSerialize.Serializer(responseData);
            return contentResult;
        }
```
</li>
</ul>
</li>

<li>
<p>To create a signed request object use the following method provided with dll. <br/></p>

```c#
public Class YourBusinesLogicClass {
    ...
    ...
    ...
    // Add this method to you code
    public Task<SignRequestModel> getSignedRequest(Object meta) {
        
        // Sample data for meta param
        // meta = '{\"attemptGUID\":\"c365a912-031b-495d-2c22-15c5d1c48d58\",\"userGUID\":\"e94ca06b-eb21-11ec-9b97-0a264b1a6b74\",\"testGUID\":\"293a30c8-59de-477c-8fc6-2d34b893cd25\",\"type\":\"verification-steps\"}' 
        

        HMACService hMACService = new HMACService();
        // Populate user information object
        SignRequestRequestParameterUserDetails userDetails = new requestRequestParameterUserDetails("yourUserUniqueId", 0, "first_name",  “abc@useremail.com”, “user_last_name”);
       
        // Populate input request object
        SignRequestRequestParameter requestParameter = new SignRequestRequestParameter("launch_mode", userDetails, meta);

        // Populate Security object for signature generation
        SignRequestSecurityParameter security = new SignRequestSecurityParameter("yourConsumerKey", "yourDomain", 1234567890, "IP address of requesting client");

        //  Populate input data model for the signature generation
        SignRequestModel signRequestModel = new SignRequestModel(security, requestParameter);


        // Pass the above input model to create signature using dll
        return await hMACService.CreateHMACSignature(signRequestModel, "secret");
     }
     ...
     ...
     ...
}
```
</li>
<li>
    Consumers should make sure that the web API added in this step is not a public API to avoid any unauthorized access.
</li>
</ul>
</li>

<li>

### SDK initialization
<p>The TestMaverick SDK provides the init method for initializing the SDK with required configuration based on consumers requirement. <b><u>This  method requires two parameters as given below:</u></b></p>

<table>
<thead>
<tr>
<th>Parameter</th>
<th>type</th>
<th>value</th>
</tr>
</thead>
<tbody>
</tbody>
<tr>
<td>initConfig</td>
<td>object</td>
<td><a href="#configParameter">

`config` : `object`</a><br/>
`authURL ` : `string`
</td>
</tr>
<tr>
<td>callbacks </td>
<td>object</td>
<td>

`readyListener()` : `function` <br/>
`errorListener(error) ` : `function`
</td>
</tr>
</table>

<h4 id="configParameter"><u>config parameter</u></h4>
<table>
<thead>
<tr>
<th>Parameter</th>
<th>type</th>
</tr>
</thead>
<tbody>
<tr>
<td>attemptGUID </td>
<td>

`string`</td>
</tr>
<tr>
<td>userGUID  </td>
<td>

`string`</td>
</tr>
<tr>
<td>testGUID  </td>
<td>

`string`</td>
</tr>
<tr>
<td>type  </td>
<td>

`string`</td>
</tr>
</tbody>
</table>

<h4><u>callbacks descriptions</u></h4>
<table>
<thead>
<tr>
<th>Function name</th>
<th>Description</th>
<th>Parameter</th>
</tr>
</thead>
<tbody>
<tr>
<td>readyListener()</td>
<td>This listener will be triggered when the SDK has been successfully initialized.</td>
<td align="center"> - </td>
</tr>
<tr>
<td>errorListener(error)</td>
<td>
This listener will be triggered whenever there is any error while initializing the SDK or at any point when the SDK is running. <br/>
 It will provide an error object that contains error code, error message, error stack trace and other useful information for debugging.

</td>
<td align="center"> 

`error` : `string`
 </td>
</tr>
</tbody>
</table>

<strong>Sample code :</strong>

```javascript
const initConfig  = {
   config: {
      attemptGUID : “7557fc68-fc1a-4755-b58e-9f6055fcebe8”
      userGUID : “dc49115b-e655-11ec-9b97-0a264b1a6b74”,
      testGUID : “xyz” ,
      type : “verification-steps”
   },
   authURL : “<consumers signed request web API URL>”
 }

const readyListener = () => {
    // DO SOMETHING
}

const errorListener = (error) => {
    // DO SOMETHING
}

// initialize the SDK using above config
window.Testmaverick.AutoProctoring.init(
    initConfig ,
    {
       readyListener,
       errorListener
    }
)
```
</li>
</ol>
