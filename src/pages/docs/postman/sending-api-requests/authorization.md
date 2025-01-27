---
title: "Authorizing Requests"
order: 19
page_id: "authorization"
contextual_links:
  - type: section
    name: "Prerequisites"
  - type: link
    name: "Sending your first request"
    url: "/docs/postman/launching-postman/sending-the-first-request/"
  - type: link
    name: "Requests"
    url: "/docs/postman/sending-api-requests/requests/"
  - type: section
    name: "Additional Resources"
  - type: subtitle
    name: "Videos"
  - type: link
    name: "Intro to Postman: Authorizing a request"
    url: "https://www.youtube.com/watch?v=Q23wkkfezfM"
  - type: subtitle
    name: "Related Blog Posts"
  - type: link
    name: "Generate Spotify playlists using a Postman collection"
    url: "https://blog.getpostman.com/2016/11/09/generate-spotify-playlists-using-a-postman-collection/?_ga=2.227822043.754547870.1571851340-1454169035.1570491567"
  - type: link
    name: "Keep it DRY with collection and folder elements"
    url: "https://blog.getpostman.com/2017/12/13/keep-it-dry-with-collection-and-folder-elements/?_ga=2.195252074.754547870.1571851340-1454169035.1570491567"
  - type: link
    name: "Postman makes authorization stronger and easier"
    url: "https://blog.getpostman.com/2017/11/04/postman-makes-authorization-stronger-and-easier/?_ga=2.195252074.754547870.1571851340-1454169035.1570491567"
  - type: link
    name: "Audit your AWS infrastructure with Postman"
    url: "https://blog.getpostman.com/2017/12/19/audit-your-aws-infrastructure-with-postman/?_ga=2.195252074.754547870.1571851340-1454169035.1570491567"
  - type: subtitle
    name: "Next Steps"
  - type: link
    name: "Troubleshooting your Requests"
    url: "/docs/postman/sending-api-requests/troubleshooting-api-requests/"


warning: false

---

APIs use authorization to ensure that client requests access data securely. This can involve authenticating the sender of a request and verifying that they have permission to access or manipulate the relevant data. If you're building an API, you can choose from a variety of auth models. If you're integrating a third-party API, the required authorization will be specified by the API provider.

You can pass auth details along with any request you send in Postman. Auth data can be included in the header, body, or as parameters to a request. If you enter your auth details in the __Authorization__ tab, Postman will automatically populate the relevant parts of the request for your chosen auth type. You can use variables and collections to define authorization details more safely and efficiently, letting you reuse the same information in multiple places.

## Contents

* [Specifying authorization details](#specifying-authorization-details)
    * [Inheriting auth](#inheriting-auth)
    * [No auth](#no-auth)
    * [API key](#api-key)
    * [Bearer token](#bearer-token)
    * [Basic auth](#basic-auth)
    * [Digest auth](#digest-auth)
    * [OAuth 1.0](#oauth-10)
    * [OAuth 2.0](#oauth-20)
        * [Authorization code](#authorization-code)
        * [Implicit](#implicit)
        * [Password credentials](#password-credentials)
        * [Client credentials](#client-credentials)
        * [Requesting a token](#requesting-an-oauth-20-token)
    * [Hawk authentication](#hawk-authentication)
    * [AWS Signature](#aws-signature)
    * [NLTM authentication](#nltm-authentication)
    * [Akamai EdgeGrid](#akamai-edgegrid)
* [Syncing cookies](#syncing-cookies)
* [Next steps](#next-steps)

## Specifying authorization details

With a request open in Postman, use the __Authorization__ tab __Type__ dropdown to select an auth type. Postman will prompt you to complete the relevant details for your selected type. The correct data values will be determined by your API at the server side—if you're using a third party API you will need to refer to the provider for any required auth details.

![Auth Types](https://assets.postman.com/postman-docs/authorization-types.jpg)

> You can use these auth types with Newman and Postman monitors as well as in the app.

When you select a type, Postman will indicate which parts of the request your details will be included in, for example the header, body, URL, or query parameters. Click __Preview Request__ to see how your auth data will be added to the request before attempting to send it.

![Preview Request](https://assets.postman.com/postman-docs/preview-request-auth.jpg)

Your auth data will appear in the relevant parts of the request, for example appended to text in the header using the standard formats your API will be able to process.

![Bearer Token Preview](https://assets.postman.com/postman-docs/bearer-token-preview.jpg)

Your requests can use environment, collection, and global [variables](/docs/postman/environments-and-globals/variables/). Postman does not save header data or query parameters to avoid exposing sensitive data such as API keys.

> You can inspect a raw dump of the entire request including auth data in the Postman console after you send it.

## Inheriting auth

If you group your requests in [collections](/docs/postman/collections/intro-to-collections/) and [folders](/docs/postman/collections/managing-collections/#adding-folders), you can specify auth details to reuse throughout a group.

Select a collection or folder in __Collections__ on the left of the Postman app. Use the overflow button (__...__) to open the options and select __Edit__ to configure the collection or folder detail.

![Edit Collection](https://assets.postman.com/postman-docs/edit-collection.jpg)

In the edit view, select the __Authorization__ tab.

![Collection Authorization](https://assets.postman.com/postman-docs/collection-auth.jpg)

By default, requests inside the collection or folder will inherit auth from the parent, which means that they'll use the same auth that you've specified at the folder or collection level. To change this for an individual request, make a different selection in the request __Authorization__ tab.

![Inherit Authorization](https://assets.postman.com/postman-docs/inherit-auth.jpg)

You can choose an authorization type upfront using the same technique when you first create a collection or folder.

## No auth

Postman will not attempt to send authorization details with a request unless you specify an auth type. If your request does not require authorization, select __No Auth__ from the __Authorization__ tab __Type__ dropdown list.
  
## Bearer token

Bearer tokens allow requests to authenticate using an access key, such as a JSON Web Token (JWT). The token is a text string, included in the request header. In the request __Authorization__ tab, select __Bearer Token__ from the __Type__ dropdown list. In the __Token__ field, enter your API key value—or for added security, store it in a variable and reference the variable by name.

![Bearer Token Preview](https://assets.postman.com/postman-docs/bearer-token-value.jpg)

Postman will append the token value to the text "Bearer " in the required format to the request Authorization header as follows:

```shell
Bearer <Your API key>
```

![Bearer Token Preview](https://assets.postman.com/postman-docs/bearer-token-preview.jpg)

## Basic auth

Basic authentication involves sending a verified username and password with your request. In the request __Authorization__ tab, select __Basic Auth__ from the __Type__ dropdown list.

![Basic Auth](https://assets.postman.com/postman-docs/basic-auth.jpg)

Enter your API login details in the __Username__ and __Password__ fields—for additional security you can store these in variables.

Click __Preview Request__ to see how Postman will append your basic auth details to the request. In the request __Headers__, you will see that the Authorization header is being passed a Base64 encoded string representing your username and password values, appended to the text "Basic " as follows:

```shell
Basic <Base64 encoded username and password>
```

![Basic Auth Encoded](https://assets.postman.com/postman-docs/basic-auth-encoded.jpg)

## Digest auth

With Digest auth, the client sends a first request to the API, and the server responds with a few details, including a number that can be used only once (nonce), a realm value, and a `401` unauthorized response. You then send back an encrypted array of data including username and password combined with the data received from the server in the first request. The server uses the passed data to generate an encrypted string and compares it against what you sent in order to authenticate your request.

In the __Authorization__ tab for a request, select __Digest Auth__ from the __Type__ dropdown list. Postman will present fields for both stages of authentication request—however it will autocomplete the fields for the second request using data returned from the server by the first request. To allow Postman to automate the flow, enter __Username__ and __Password__ values (or variables) and these will be sent with the second request.

![Digest Auth](https://assets.postman.com/postman-docs/digest-auth.jpg)

> If you don't want Postman to automatically extract the data, check the box to disable retrying the request. If you do this, you will need to complete the advanced fields and run each request manually.

The advanced fields are optional, and Postman will attempt to populate them automatically when your request runs.

* __Realm:__ A string specified by the server in the `WWW-Authenticate` response header.
* __Nonce:__ A unique string specified by the server in the `WWW-Authenticate` response header.
* __Algorithm:__ A string that indicates a pair of algorithms used to produce the digest and a checksum.
* __qop:__ The quality of protection applied to the message. The value must be one of the alternatives specified by the server in the `WWW-Authenticate` response header.
* __Nonce Count:__ The hexadecimal count of the number of requests (including the current request) that the client has sent with the nonce value in this request.
* __Client Nonce:__ An opaque quoted string value provided by the client, used by both client and server to avoid chosen plaintext attacks, to provide mutual authentication, and to provide some message integrity protection.
* __Opaque:__ A string of data specified by the server in the `WWW-Authenticate` response header, which should be used unchanged with URIs in the same protection space.

## OAuth 1.0

OAuth 1.0 allows client applications to access data provided by a third-party API. For example, as a user of a service you can grant another application access to your data with that service without exposing your login details. Accessing user data via the OAuth 1.0 flow involves a few requests back and forth between client application, user, and service provider.

OAuth 1.0 is sometimes referred to as "two-legged" (auth only between client and server) or "three-legged" (where a client requests data for a user of a third-party service).

* To request user data with a third-party service, a consumer (client application) requests an access token using a key and secret.
* The service provider issues an initial token (that doesn't provide access to user data) and the consumer requests authorization from the user.
* When the user grants auth, the consumer makes a request to exchange the temporary token for an access token, passing verification from the user auth.
* The service provider returns the access token and the consumer can then make requests to the service provider to access the user's data.

In the __Authorization__ tab for a request, select __OAuth 1.0__ from the __Type__ dropdown list.

![OAuth 1.0](https://assets.postman.com/postman-docs/oauth1-setup.jpg)

Enter your __Consumer Key__, __Consumer Secret__, __Access Token__, and __Token Secret__ values. You can optionally set advanced details—otherwise Postman will attempt to autocomplete these.

You can include the auth details either in the request headers or in the body / URL—select one from the dropdown list. Click __Preview Request__ if you want to check how the details will be included with the request.

If you send the OAuth 1.0 data in the headers, you will see an Authorization header sending your key and secret values appended to the string " OAuth " together with additional comma-separated required details.

![OAuth 1.0 Headers](https://assets.postman.com/postman-docs/oauth1-headers.jpg)

If you send the OAuth 1.0 data in the body and URL, you will find the data added either in the request __Body__ or __Parameters__ depending on the request method.

![OAuth 1.0 Query Parameters](https://assets.postman.com/postman-docs/oauth1-query.jpg)

If the request method is `POST` or `PUT`, and if the request body type is `x-www-form-urlencoded`, Postman will add the authorization parameters to the request body. Otherwise, for example in a `GET` request, your key and secret data will be passed in the URL query parameters.

The OAuth 1.0 auth parameter values are as follows:

* __Consumer Key:__ A value used to identify a consumer with the service provider.
* __Consumer Secret:__ A value used by the consumer to establish ownership of the key.
* __Access Token:__ A value representing the consumer's permission to access the user's data.
* __Token Secret:__ A value used by the consumer to establish ownership of a given token.
* Advanced Parameters:
    * __Signature Method:__ A consumer secret that establishes ownership of a given token.
    * __Time Stamp:__ The timestamp the server uses to prevent replay attacks outside the time window.
    * __Nonce:__ A random string generated by the client.
    * __Version:__ The version of the OAuth authentication protocol (1.0).
    * __Realm:__ A string specified by the server in the `WWW-Authenticate` response header.

> Some implementations of OAuth 1.0 require empty parameters to be added to the signature. Check the __Add empty parameters to signature__ checkbox if you need this.

## OAuth 2.0

OAuth 1.0 allows client applications to access data provided by a third-party API. For example, as a user of a service you can grant another application access to your data with that service without exposing your login details. With OAuth 2.0, you first retrieve an access token for the API, then use that token to authenticate future requests. Accessing data via the OAuth 2.0 flow varies greatly between API service providers, but typically involves a few requests back and forth between client application, user, and API.

An example OAuth 2.0 flow could run as follows:

* A client application makes a request for the user to authorize access to their data.
* If the user grants access, the application then requests an access token from the service provider, passing the access grant from the user and authentication details to identify the client.
* The service provider validates these details and returns an access token.
* The client uses the access token to request the user data via the service provider.

In the __Authorization__ tab for a request, select __OAuth 2.0__ from the __Type__ dropdown list. Specify whether you want pass the auth details in the request URL or headers.

![OAuth 2.0](https://assets.postman.com/postman-docs/oauth2-setup.jpg)

To request an access token, click __Get New Access Token__.

![Get Access Token](https://assets.postman.com/postman-docs/get-access-token.jpg)

Enter the details for your client application, and any auth details from the service provider. This allows you to replicate your application auth flow inside Postman in order to test authenticated requests.

Postman will prompt you to supply specific details depending on the OAuth 2.0 __grant__ type, which can be [Authorization code](#authorization-code), [Implicit](#implicit), [Password credentials](#password-credentials), or [Client credentials](#client-credentials).

### Authorization code

Authorization code grant type requires the user to authenticate with the provider—an authorization code is then sent back to the client app, extracted, and exchanged with the provider for an access token to authenticate subsequent requests.

To use Authorization code grant type, enter a __Callback URL__ for your client application (which should be registered with the API provider), together with various details provided by the API service including __Auth URL__, __Access Token URL__, __Client ID__, and __Client Secret__.

### Implicit

Implicit grant type returns an access token to the client straight away without requiring the additional auth code step (and is therefore less secure).

To use implicit grant type with your requests in Postman, enter a __Callback URL__ you have registered with the API provider, the provider __Auth URL__, and a __Client ID__ for the app you have registered.

### Password credentials

OAuth 2.0 Password grant type involves sending username and password directly from the client and is therefore not recommended if you're dealing with third-party data.

To use password grant type, enter your API provider's __Access Token URL__, together with the __Username__ and __Password__. In some cases you will also need to provide a client ID and secret.

### Client credentials

Client credentials grant type is typically not used to access user data but instead for data associated with the client application.

Enter the provider's __Access Token URL__, together with the __Client ID__ and __Client Secret__ for your registered application.

### Requesting an OAuth 2.0 token

The full list of parameters to request a new access token is as follows, depending on your grant type:

* __Token Name:__ The name you want to use for the token.
* __Grant Type:__ A dropdown list of options—this will depend on the API service provider requirements.
* __Callback URL:__ The client application callback URL redirected to after auth, and that should be registered with the API provider. If not provided, Postman will use a default empty URL and attempt to extract the code or access token from it—if this does not work for your API, you can use the following URL: `https://www.getpostman.com/oauth2/callback`
* __Auth URL:__ The endpoint for the API provider authorization server, to retrieve the auth code.
* __Access Token URL:__ The provider's authentication server, to exchange an authorization code for an access token.
* __Client ID:__ The ID for your client application registered with the API provider.
* __Client Secret:__ The client secret given to you by the API provider.
* __Scope:__ The scope of access you are requesting, which may include multiple space-separated values.
* __State:__ An opaque value to prevent cross-site request forgery.
* __Client Authentication:__ A dropdown—send a Basic Auth request in the header, or client credentials in the request body. _After upgrading to a new version, change the value in this dropdown menu to avoid problems with client authentication._

When your config is complete, click __Request Token__. If you successfully receive a token from the API you will see its details, together with the expiry, and optionally a refresh token you can use to retrieve a new access token when your current one expires. Click __Use Token__ to select the returned value.

Any successfully retrieved tokens will be listed in the request __Available Tokens__ dropdown list. Select one to send with your request. Select __Manage Tokens__ in the dropdown list to view more details or delete your tokens.

> Deleting a token in Postman does not revoke access. Only the server that issues the token can revoke it.

## Hawk authentication

Hawk authentication enables you to authorize requests using partial cryptographic verification.

In the __Authorization__ tab for a request, select __Hawk Authentication__ from the __Type__ dropdown list.

![Hawk Auth](https://assets.postman.com/postman-docs/hawk-auth.jpg)

Enter your details in the __Hawk Auth ID__, __Hawk Auth Key__, and __Algorithm__ fields. You can optionally set advanced details, but Postman will attempt to generate values for them if necessary.

The Hawk Authentication parameters are as follows:

* __Hawk Auth ID:__ Your API authentication ID value.
* __Hawk Auth Key:__ Your API authentication key value.
* __Algorithm:__ The hash algorithm used to create the message authentication code (MAC).
* Advanced parameters:
    * __User:__ The username.
    * __Nonce:__ A random string generated by the client.
    * __ext:__ Any application-specific information to be sent with the request.
    * __app:__ The binding between credentials and the application to prevent an attacker using credentials issued to someone else.
    * __dlg:__ The ID of the application the credentials were issued to.
    * __Timestamp:__ Timestamp the server uses to prevent replay attacks outside the time window.

## AWS Signature

AWS is the authorization workflow for Amazon Web Services requests. AWS uses a custom HTTP scheme based on a keyed-HMAC (Hash Message Authentication Code) for authentication.

The official AWS Signature documentation provides more detail:

* [Signing and Authenticating REST Requests](http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html)
* [Use Postman to Call an API](http://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-use-postman-to-call-api.html)

In the __Authorization__ tab for a request, select __AWS Signature__ from the __Type__ dropdown list.

![AWS Signature Auth](https://assets.postman.com/postman-docs/aws-signature-auth.jpg)

Enter your access key and secret values either directly in the fields or via variables for additional security.

You can optionally set advanced fields, but Postman will attempt to autogenerate these if necessary.

The AWS Signature parameters are as follows:

* __AWS Region:__ The region receiving the request (defaults to `us-east-1`).
* __Service Name:__ The service receiving the request.
* __Session Token:__ Required only when using temporary security credentials.

## NTLM authentication

Windows Challenge/Response (NTLM) is the authorization flow for the Windows operating system and for standalone systems.

In the __Authorization__ tab for a request, select __NTLM Authentication__ from the __Type__ dropdown list.

![NTLM Authentication](https://assets.postman.com/postman-docs/ntlm-auth.jpg)

Enter your __Username__ and __Password__ for NTLM access (use variables to avoid entering the values directly). You can optionally specify advanced parameters, but Postman will attempt to autocomplete these if necessary. By default your request will run a second time after extracting data received from the first—you can disable this by checking the checkbox.

Advanced parameters for NTLM auth are as follows:

* __Domain:__ The domain or host to authenticate against.
* __Workstation:__ The hostname of the PC.

## Akamai EdgeGrid

 Akamai Edgegrid is an authorization helper developed and used by Akamai.

In the __Authorization__ tab for a request, select __Akamai EdgeGrid__ from the __Type__ dropdown list.

 ![Akamai EdgeGrid Auth](https://assets.postman.com/postman-docs/akamai-auth.jpg)

Enter your __Access Token__, __Client Token__, and __Client Secret__, using variables for additional security—you will receive these details when you register a client application with Akamai.

 For information on obtaining your credentials, see [Akamai Developer - Authorize your Client](https://developer.akamai.com/legacy/introduction/Prov_Creds.html).

## Syncing cookies

If you have session cookies in your browser, you can sync them to Postman using the Interceptor—see [Interceptor extension](https://learning.getpostman.com/docs/postman/sending-api-requests/interceptor-extension/) and [Cookies](/docs/postman/sending-api-requests/cookies/) for more detail.

## Next steps

If you're having issues getting a request to authenticate and run successfully, try some of the tips in [troubleshooting API requests](/docs/postman/sending-api-requests/troubleshooting-api-requests/). If you still have auth problems, check out the [authentication tag](https://community.getpostman.com/tags/authentication) on the Postman forum.
