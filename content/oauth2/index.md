+++
title = "OAuth 2.0"
date = "2024-03-07"
description = "OAuth 2.0 is a protocol for authorization and has become the industry standard for software projects."
[taxonomies]
tags = ["OAuth 2.0", "authorization", "authentication","security","architecture"]
[extra]
cover_image = "/oauth2/oauth2.jpg"
+++

## INTRODUCTION

OAuth 2.0 is a widely used standard protocol designed for secure authorization. It provides a standard so that it allows users to grant access to their data to other applications. However, if the implementation of OAuth 2.0 is not done properly, it can expose users to several security vulnerabilities. Therefore, It is essential to ensure that the implementation of OAuth 2.0 follows best practices and security standards to keep it safe for any system. This article provides a comprehensive guide to the overview of OAuth 2.0 roles, authorization grant types, and flows.

## BACKGROUND

Authentication and authorization represent foundational security processes essential for safeguarding systems and information. Authentication, simplistically defined, is the procedure of confirming a user's identity, whereas authorization is the process of validating the resources or privileges to which they are granted access. Table 1 presents a comparison of authentication and authorization.

| Table 1: Authentication vs Authorization |
| :---: | 

| **Authentication** | **Authorization** |
|---|---|
| Verifies the identity of users to ensure they are who they claim to be. | Decides the permissible access rights and restrictions for users. |
| Prompts the user to authenticate their credentials, employing methods such as passwords, responses to security questions, or facial recognition. | Confirms the permissibility of access based on established policies and rules. |
| Commonly performed before granting authorization. | Typically conducted subsequent to successful authentication. |

## OAUTH 2.0

OAuth 2.0 is an authorization protocol that was first introduced in 2012 and has since evolved into a framework. It has become the de facto standard for authorization. It is designed to allow any application to access a set of resources without requiring the user to share their credentials. This way, it provides a standard for users to grant access to their data to other applications, without their credentials. 

OAuth 2.0 adds an extra layer of security and helps users have better control over their data sharing. With OAuth 2.0, users can restrict the actions that various client apps can perform on their data resources, thus ensuring that their data remains private and secure. 

OAuth 2.0 is a popular authorization framework widely used and adopted by major web services such as Google, Microsoft, Linkedin, Facebook, and many others.

### OAuth 2.0 Roles

There are four specific roles involved in the OAuth 2.0 workflow, each with its own set of responsibilities. Each role plays an important part in the authentication and authorization. The four roles are:

* **Resource Owner:** The resource owner is a user or system that has the ability to grant access to a protected resource.

* **Client:** The client is an application or service that requires access to the resource owner's data that is protected. To access resources, the client must belong to the access token. 

* **Authorization Server:** It is responsible for receiving requests from the client for access tokens and verifying the user's grants. When the authentication is successful and the resource owner has given their consent, the authorization server issues access tokens to the client. The authorization server provides a verification mechanism to identify the user's grant by receiving requests from the client with access tokens. It's important to note that the authorization server in OAuth 2.0 provides two endpoints - the Authorization endpoint and the Token endpoint. 

* **Resource Server:** The resource server protects the actual user's resources that the client wants to access. This server provides services that validate an access token from the client and also grants appropriate resources to those who request it.

### Protocol Flow

OAuth 2.0 is a protocol that allows the resource owner to authorize a third-party application to access their data without sharing their credentials.

The abstract OAuth 2.0 flow is illustrated in Figure 1, which describes the interaction between the four roles and includes the following steps:

1. To access service resources, the application requests authorization from the user.

2. If the user authorizes the request, the application receives an authorization grant.

3. To obtain an access token from the authorization server, the application sends a request by authenticating its own identity and presenting the authorization grant to the authorization server.

4. After the application identity is authenticated and the authorization grant is valid, the access token is issued by the authorization server. All steps of authorization are complete at this step. 

5. If the application wants the resource from the resource server, it needs to request the resources from the resource server and present the access token for authentication.

6. Upon validation of the access token issued by the authorization server, the resource server grants authorization to the application, allowing it access to the designated resources.

<center>
    <figure>
    <img src="/oauth2/Protocol_Flow.png" alt="Figure 1: Protocol Flow" width="750px">
    <figcaption>Figure 1: Protocol Flow</figcaption>
    </figure>
</center>

### Access Token

It basically tells the API that the person or organization using the token has been authorized to access the API and execute specific actions as delineated by the granted scope. 

### Refresh Token

A refresh token is a specialized type of token designed to acquire a renewed access token. It is advantageous for renewing expiring access tokens without requiring the user to go through the login process again.

<center>
    <figure>
    <img src="/oauth2/Refresh_Token.png" alt="Figure 2: Refreshing an Expired Access Token" width="750px">
    <figcaption>Figure 2: Refreshing an Expired Access Token</figcaption>
    </figure>
</center>

### Scope

An application can request one or more scopes and this information is displayed to the user on the consent screen. The resulting access token issued to the application will be confined to the specific scopes granted by the user.

### OAuth 2.0 Endpoints

OAuth 2.0 utilizes two endpoints:
* Authorization Endpoint
* Token endpoint


1. **Authorization Endpoint:** The authorization endpoint is employed to engage with the resource owner and obtain authorization to access the protected resource. The request parameters of the authorization endpoint include:

    * **response_type:** Specifies to the authorization server which grant type to execute. 

    * **client_id:** The identifier of the application requesting authorization.

    * **redirect_uri:** A successful response from this endpoint leads to a redirection to the specified URL.

    * **scope:** A space-delimited list of permissions that the application necessitates.

    * **state:** The state parameter is a parameter that enables the restoration of the prior state of your application. It preserves a state object set by the client in the authorization request and makes it accessible to the client in the response.

2. **Token Endpoint:** The token endpoint is utilized by the client to acquire an access token by presenting its authorization grant or refresh token.

### Authorization Grant Types

The authorization grant type determines the flow of the OAuth 2.0 process, and different grant types have different levels of security and authentication requirements. This section delves into the concept of authorization grant types as the authorization grant type depends on the method used by the application to request authorization.

OAuth 2.0 has five important grant types that are useful in different cases:

1. **Authorization Code Grant:** The authorization server provides a single-use authorization code to the client.

2. **Client Credentials Grant:** The application or client is authenticated by using its client ID and secret code. 

3. **Device Code Grant:** The device code grant type offers a mechanism for devices without a browser or with limited inputs to acquire an access token and gain access to a user's account. 

4. **Implicit Grant:** The implicit grant is commonly used for single-page web applications. It allows for the access token to be returned directly to the client from the authorization endpoint, without going through the authorization code exchange process.

5. **Password Grant:** The password grant is used to obtain an access token, the application forwards the user's username and password, subsequently allowing the application to acquire the access token.

{% admonition(type="warning", title="warning") %}
It is important to note that two grant types, namely the **Implicit Grant** and the **Password Grant** types, are no longer recommended for use due to security concerns. Both grant types used have already caused vulnerabilities.
{% end %}

#### 1. Authorization Code Grant

The authorization server provides a single-use authorization code to the client. This authorization code can then be exchanged for an access token, which allows the client to access the protected resource. The authorization code grant type is commonly used for web applications, especially when the exchange can be securely carried out on the server side, making it ideal for server-side applications.

**Authorization Code Flow:** The example of authorization or grant type flow is illustrated step by step.

**Step 1 - Authorization Code Link:** Initially, the user receives an authorization code link formatted as follows:

```
http://localhost:5050/oauth/authorize?
response_type=code&
client_id=CLIENT_ID&
redirect_uri=CALLBACK_URL&
scope=read&
state=aY0l3S7H5qHzSlXNGEe8p
```

Explanation of the components is presented below:

* **http://localhost:5050/oauth/authorize:** API endpoint for authorization

* **client_id:** The application's client ID

* **redirect_uri:** This is the point where the service redirects the user-agent after the authorization code has been granted.

* **response_type:** This signifies that your application is requesting an authorization code grant.

* **scope:** Specifies the level of access that the application is requesting.

* **state:** The state parameter is a parameter that enables the restoration of the prior state of your application.

**Step 2 - User Authorizes Application:** When clicking the link, users are required to log in to the service to authenticate their identity. Subsequently, the service will prompt them to either authorize or deny the application access to their account. 

**Step 3 - Application Receives Authorization Code:** If the user chooses to authorize the application, the service redirects the user-agent to the application's redirect URI, as previously specified during client registration. This redirection includes an authorization code. The resulting redirect would resemble the following:

```
http://myapplication.com/callback?code=AUTHORIZATION_CODE
```

**Note:** This article assumes that the application is myapplication.com

**Step 4 - Application Requests Access Token:** The application initiates the request for an access token from the API by submitting the authorization code, along with authentication details that encompass the client secret, to the API token endpoint.

It is an example of the POST request to a token endpoint.

```
http://localhost:5050/oauth/token?
 client_id=CLIENT_ID&
 client_secret=CLIENT_SECRET&
 grant_type=authorization_code&
 code=AUTHORIZATION_CODE&
 redirect_uri=CALLBACK_URL
```

**Step 5 - Application Receives Access Token:** If the authorization is valid, the API will send a response containing the access token to the application. The response is anticipated to resemble the following:

```
{
  "access_token":"ACCESS_TOKEN",
   "token_type":"bearer",
   "expires_in":326380,
   "refresh_token":"REFRESH_TOKEN",
   "scope":"read",
   "uid":200515,
   "info":
    {
       "name":"Bill Key",
       "email":"billkey@billkey.com"
    }
}
```
The authorization code grant type is illustrated in Figure 3.

<center>
    <figure>
    <img src="/oauth2/Authorization_Code_Flow.png" alt="Figure 3: Authorization Code Flow" width="750px">
    <figcaption>Figure 3: Authorization Code Flow</figcaption>
    </figure>
</center>

#### Authorization Code Flow with Proof Key for Code Exchange (PKCE)

During authentication, mobile and native applications can use the Authorization Code Flow for security, but they require additional measures to ensure safety. Single-page apps, on the other hand, have their own set of challenges. To overcome these issues, OAuth 2.0 has introduced a variation of the Authorization Code Flow called Proof Key for Code Exchange (PKCE).

PKCE (RFC 7636) is an extension to the Authorization Code Flow designed to mitigate CSRF and authorization code injection attacks.

#### 2. Client Credentials Grant

The client credentials grant type is commonly used in scenarios where there is no user involved, and the client authenticates itself directly with the authorization server.

The application requests an access token by sending its credentials, specifically the client ID and client secret, to the authorization server.

```
http://localhost:5050/oauth/token?grant_type=client_credentials&client_id=CLIENT_ID&client_secret=CLIENT_SECRET
```

If successful verification of the application credentials, the authorization server issues an access token to the application, authorizing it to utilize its account. This method is particularly well-suited for a microservices architecture, facilitating secure communication among services or through a gateway server. 


**Client Credentials Flow:** The example of client credentials grant type flow is illustrated step by step:

**Step 1:** The application sends its credentials to the authorization server.

**Step 2:** The authorization server validates the application's credentials.

**Step 3:** The authorization server responds with an access token.

**Step 4:** The application can utilize the access token for making API calls on its behalf.

**Step 5:** API responds with requested data.

The client credentials flow is illustrated in Figure 4.

<center>
    <figure>
    <img src="/oauth2/Client_Credentials_Flow.png" alt="Figure 4: Client Credentials Flow" width="750px">
    <figcaption>Figure 4: Client Credentials Flow</figcaption>
    </figure>
</center>

 #### 3. Device Code Grant
 
 The device code grant type offers a mechanism for devices without a browser or with limited inputs to acquire an access token and gain access to a user's account. The primary purpose of this grant type is to enable users to authorize applications on such devices, allowing them access to their accounts and resources. For example, when users aim to log in to a video streaming application on a device lacking conventional keyboard input, such as a smart television, the device code provides a solution, facilitating user access to the application.


**Device Code Flow:** The example of device code grant type flow is illustrated step by step.

**Step 1:** The client transmits an access request to the authorization server, which includes its client identifier.

POST http://localhost:5050/device 
client_id=CLIENT_id

**Step 2:** The authorization server generates a device code, an end-user code, and an end-user verification URI.


```
"device_code":"5FMxmDmtHFD55X3DUg081VYYxcfc9Y",
"user_code":"ARRY-NWNJ",
"expires_in":326380,
"verification_uri":”http://localhost:5050/devices",
"interval": 10,
"expires_in”: 1600,
```

**Step 3:** The client directs the user to access the given URI using a secondary device (e.g., a mobile device) and supplies the user with the end-user code.

**Step 4:** The authorization server prompts the user to input the end-user code. Subsequently, the authorization server verifies the code and requests the end user to either accept or decline the authorization request.

**Step 5:** As the end user evaluates the authorization request, the client continuously queries the authorization server using the device code and client identifier to ascertain if the user has concluded the authorization step.

**Step 6:** If the user approves access, the authorization server validates the verification code and issues a response containing the access token.

The device code flow is illustrated in Figure 5.

<center>
    <figure>
    <img src="/oauth2/Device_Code_Flow.png" alt="Figure 5: Device Code Flow" width="750px">
    <figcaption>Figure 5: Device Code Flow</figcaption>
    </figure>
</center>

#### 4. Implicit Grant 

The implicit grant type is notably simpler. Instead of initially acquiring an authorization code and then exchanging it for an access token, the client application instantly receives the access token once the user grants their consent. With the implicit grant type, all communication occurs through browser redirects without a secure back-channel, unlike the authorization code flow. Consequently, this exposes the sensitive access token and the user's data to potential security risks. The implicit grant type is better suited for single-page applications and native desktop applications. The implicit flow is illustrated in Figure 6.

<center>
    <figure>
    <img src="/oauth2/Implicit_Code_Flow.png" alt="Figure 6: Implicit Grant Flow" width="750px">
    <figcaption>Figure 6: Implicit Grant Flow</figcaption>
    </figure>
</center>

#### 5. Password Grant

The Password grant type is considered a legacy method for swapping a user's credentials for an access token. However, it is not recommended anymore because it involves the client application collecting the user's password and transmitting it to the authorization server, posing security risks. The latest OAuth 2.0 Security Current Practice disallows the password grant entirely, and the grant is not defined in OAuth 2.1.

## REFERENCES


[1] Mitchell Anicas, “An Introduction to OAuth 2” 2021, <a href="https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2" target="_blank">https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2</a> 

[2] Okta, “What is OAuth 2.0?”, <a href="https://auth0.com/intro-to-iam/what-is-oauth-2" target="_blank">https://auth0.com/intro-to-iam/what-is-oauth-2</a>

[3] David Demir, “OAuth 2.0: What is OAuth and How Does it Work?”, 2023, <a href="https://apidog.com/blog/what-is-oauth-2" target="_blank">https://auth0.com/intro-to-iam/what-is-oauth-2</a>

[4] Gbadebo Bello, “What is OAuth 2.0?”, 2023,  <a href="https://blog.postman.com/what-is-oauth-2-0" target="_blank">https://blog.postman.com/what-is-oauth-2-0/</a>

[5] Ramotion, “What is OAuth? A Beginner's Guide to Authentication for APIs” 2023, <a href="https://www.ramotion.com/blog/what-is-oauth-authentification" target="_blank">https://www.ramotion.com/blog/what-is-oauth-authentification</a>

[6] Aaron Parecki, “OAuth 2.0”,  <a href="https://oauth.net/2" target="_blank">https://oauth.net/2/</a>

[7] Cloudentity Portal, “Client Credentials Flow”, 2023, <a href="https://cloudentity.com/developers/basics/oauth-grant-types/client-credentials-flow" target="_blank">https://cloudentity.com/developers/basics/oauth-grant-types/client-credentials-flow</a>

[8] Daniel Yang, “How to Identify OAuth2 Vulnerabilities and Mitigate Risks”, 2022, <a href="https://www.coupa.com/blog/technology-innovation/how-to-mitigate-oauth2-vulnerabilities" target="_blank">https://www.coupa.com/blog/technology-innovation/how-to-mitigate-oauth2-vulnerabilities</a>

[9] PortSwigger, “OAuth 2.0 Authentication Vulnerabilities”, <a href="https://portswigger.net/web-security/oauth" target="_blank">https://portswigger.net/web-security/oauth</a>

[10] Auth0 by Okta, “Client Credentials Flow”, <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow" target="_blank">https://auth0.com/docs/get-started/authentication-and-authorization-flow/client-credentials-flow</a>

[11] Auth0 by Okta, “Authentication and Authorization Flows”, <a href="https://auth0.com/docs/get-started/authentication-and-authorization-flow" target="_blank"> https://auth0.com/docs/get-started/authentication-and-authorization-flow</a>

[12] Identity Server Documentation, “Device Authorization Grant (Device Flow)”, <a href="https://is.docs.wso2.com/en/latest/references/concepts/authorization" target="_blank">https://is.docs.wso2.com/en/latest/references/concepts/authorization/device-flow-grant</a>

[13] IETF, “OAuth 2.0 Security Best Current Practice”, <a href="https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics" target="_blank">https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics</a>
