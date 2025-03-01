<!-- loio3cc788ebc00e40a091505c6b3fa485e7 -->

# Application Routes and Destinations

The application router is the single point of entry for an application.

The application router is used to serve static content, propagates user information, and acts as a proxy to forward requests to other microservices. The routing configuration for an application is defined in one or more destinations. The application router configuration is responsible for the following tasks:

-   Act as the main user-entry point to application service
-   Serve static content
-   Serve routes and destinations
-   Rewrite URLs
-   Forward requests to other services
-   Propagate user information
-   Handle authentication
-   Manage HTML5 application/browser sessions

> ### Note:  
> The application router doesn’t manage server caching. The server cache must be set \(with e-tags\) in the server itself. The cache must contain not only static content, but container resources too. For example, relating to OData metadata.



## Destinations

A destination defines the back-end connectivity. In its simplest form, a destination is a URL to which requests are forwarded. There has to be a destination for every single app \(microservice\) that is a part of the business application.

The destinations configuration can be provided by the destinations environment variable or by the destination service.



### Destinations Environment Variable

Destinations can be defined in an environment variable for the `approuter` application. The destinations are typically specified in the application manifest file \(`manifest.yml`\). The router information is added to the `env: destinations` section of the `manifest.yml` file, as illustrated in the following example:

> ### Sample Code:  
> Destinations Defined in the Application Manifest \(`manifest.yml`\)
> 
> ```
> ---
> applications:
> - name: node-hello-world
>   port: *<approuter-port\>*  #the port used for the approuter
>   memory: 100M
>   path: web
>   env:
>     destinations: >
>       [
>         {
>           "name":"backend",
>           "url":"http://*<hostname\>*:*<node-port\>*",
>           "forwardAuthToken": true
>         }
>       ]
>   services:
>     - node-uaa
> 
> ```

> ### Note:  
> The "`name`" and "`url`" properties are mandatory.

The value of the destination `"name":"backend"` must match the value of the `destinations` property configured for a route in the corresponding application-router configuration file \(`xs-app.json`\). It’s also possible to define a logout path and method for the `destinations` property in the `xs-app.json` file.



### Destination Service

Destination configuration can be provided by `destination service`. The destination service can be consumed by the application router after binding a destination service instance to it.

The following guidelines apply to the configuration of the mandatory properties of a destination:

<a name="loio3cc788ebc00e40a091505c6b3fa485e7__table_qwy_ccb_rbb"/>Mandatory Properties of a Destination


<table>
<tr>
<th valign="top">

Property



</th>
<th valign="top">

Description



</th>
</tr>
<tr>
<td valign="top">

Type



</td>
<td valign="top">

Only HTTP is supported.



</td>
</tr>
<tr>
<td valign="top">

Authentication



</td>
<td valign="top">

All authentication types are supported.

> ### Note:  
> -   When using `basic authentication`, `User` and `Password` are mandatory.
> 
> -   When using `principal propagation`, the proxy type is `on-premise`.
> 
> -   When using `OAuth2SAMLBearerAssertion`, the `uaa.user` scope in the `xs-security.json` file is required.



</td>
</tr>
<tr>
<td valign="top">

ProxyType



</td>
<td valign="top">

Supported types:

-   `on-premise`

    If set, binding to SAP Connectivity service is required.

-   `internet`




</td>
</tr>
</table>

The following guidelines apply to the configuration of the additional properties of a destination:

<a name="loio3cc788ebc00e40a091505c6b3fa485e7__table_h4k_2db_rbb"/>Additional Properties of a Destination


<table>
<tr>
<th valign="top">

Property



</th>
<th valign="top">

Description



</th>
</tr>
<tr>
<td valign="top">

HTML5.ForwardAuthToken



</td>
<td valign="top">

If `true`, the OAuth token is sent to the destination. The default value is `false`. This token contains the user identity, scopes, and other attributes. It’s signed by the UAA so it can be used for user authentication and authorization with back-end services.

> ### Note:  
> If the `ProxyType` is set to `on-premise`, don’t set the `ForwardAuthToken` property to `true`.
> 
> If the `Authentication` is other than `NoAuthentication`, don’t set the `ForwardAuthToken` property to `true`.



</td>
</tr>
<tr>
<td valign="top">

HTML5.Timeout



</td>
<td valign="top">

Positive integer representing the maximum time to wait for a response \(in milliseconds\) from the destination. The default value is 30000 ms.

> ### Note:  
> The timeout value specified also applies to the destination's log out path \(if defined\), which belongs to the `destination` property.



</td>
</tr>
<tr>
<td valign="top">

HTML5.PreserveHostHeader



</td>
<td valign="top">

If `true`, the application router preserves the host header in the back-end request.

This is expected by some back-end systems like AS ABAP, which don’t process `x-forwarded-*` headers.



</td>
</tr>
<tr>
<td valign="top">

HTML5.DynamicDestination



</td>
<td valign="top">

If `true`, the application router allows this destination to be used dynamically on the host or path level.



</td>
</tr>
<tr>
<td valign="top">

HTML5.SetXForwardedHeaders



</td>
<td valign="top">

If true , the application router adds X-Forwarded-\(Host, Path, Proto\) headers to the back-end request. The default value is `true`.



</td>
</tr>
<tr>
<td valign="top">

sap-client



</td>
<td valign="top">

If `true`, the application router propagates the `sap-client` and its value as a header in the back-end request.

This is expected by ABAP back-end systems.



</td>
</tr>
</table>

-   If a destination with the same name is defined both in the environment destination and the destination service, the destination configuration loads the settings from the environment.

-   If the configuration of a destination is updated in runtime, the changes are reflected automatically to the AppRouter. There’s no need to restart the AppRouter.

-   The destination service is only available in the Cloud Foundry environment.

-   You can define destinations at the service instance level. This destination type has a higher priority than the same destination defined on the subaccount level. This enables exposing a specific destination to a specific application instead of to the entire subaccount.




<a name="loio3cc788ebc00e40a091505c6b3fa485e7__section_aql_dm4_njb"/>

## Connectivity

Application Router supports integration with the SAP Connectivity service. The Connectivity service handles proxy access to the Cloud Connector, which tunnels connections to private network systems. In order to use connectivity, a connectivity service instance must be created and bound to the Application Router application. In addition, the relevant destination configurations must contain `proxyType=OnPremise` and a valid XSUAA login token must be obtained from the login flow.

**Related Information**  


[Application Router](application-router-01c5f9b.md "The application router is the single point-of-entry for an application running in the Cloud Foundry environment on SAP BTP. The application router is used to serve static content, authenticate users, rewrite URLs, and forward or proxy requests to other micro services while propagating user information.")

[Resource Files](resource-files-e179c0c.md "The routing configuration for an application is defined in one or more destinations.")

[Consuming the Destination Service](https://help.sap.com/viewer/cca91383641e40ffbe03bdc78f00f681/Cloud/en-US/7e306250e08340f89d6c103e28840f30.html)

[Consuming the Connectivity Service](https://help.sap.com/viewer/cca91383641e40ffbe03bdc78f00f681/Cloud/en-US/313b215066a8400db461b311e01bd99b.html)

