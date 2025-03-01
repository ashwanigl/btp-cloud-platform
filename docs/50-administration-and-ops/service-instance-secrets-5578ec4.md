<!-- loio5578ec4b20e84d61b34fd0fe0d6deed5 -->

# Service Instance Secrets

When an application consumes a service instance of the SAP Authorization and Trust Management service \(XSUAA\), the application identifies itself to the service instance with a client ID and a secret. The client ID and secret are the credentials with which an application authenticates itself to the service instance.

The system creates these credentials either when you bind the application to the service instance or when you create a service key for the service instance.

The service instance can use multiple secrets in the ***application*** plan.

-   The instance secret is the default secret type. The secret is the same for all bindings of the service instance. The secret remains valid as long as the instance exists.

-   Binding secrets must be enabled in the application security descriptor \(`xs-security.json`\) when you create the service instance. When you bind an application to a service instance or create a service key, you can pass a `parameters.json` to use a binding secret. The secret remains valid as long as the binding or the service key exists.

    > ### Note:  
    > The ***apiaccess*** plan only uses binding secrets. However, some old instances of the ***apiaccess*** plan might still use the instance secret.

-   X.509 secrets must be enabled in the application security descriptor \(`xs-security.json`\) when you create the service instance. When you bind an application to a service instance or create a service key, you can pass a `parameters.json` to use an X.509 secret. SAP Authorization and Trust Management service can generate an X.509 certificate for you or you can provide your own. The X.509 secret remains valid as long as the certificate itself is valid.

    > ### Note:  
    > The service doesn't check for certificate revocation. The only way to revoke an X.509 secret is to rotate the secret.


The following figure illustrates the XSUAA app and its information about the OAuth 2.0 client as part of an instanceSAP Authorization and Trust Management service. A consuming application, functioning as an OAuth 2.0 client is bound to the SAP Authorization and Trust Management service instance. The secret is part of the environment of the consuming application and the information about the OAuth 2.0 client saved with the XSUAA app. Alternatively, this information is saved as part of a service key.

   
  
<a name="loio5578ec4b20e84d61b34fd0fe0d6deed5__fig_eyd_prh_sjb"/>Binding Between an SAP Authorization and Trust Management Service Instance and a Consuming Application

 ![](images/BindingInformation_4bcb021.png "Binding Between an SAP
									Authorization and Trust Management Service
				Instance and a Consuming Application") 

The `credential-types` parameter of the OAuth client configuration in the application security descriptor \(`xs-security.json`\) determines which secrets bindings support.

In the following example, the service instance creates a binding secret for all new bindings. It allows the creation of X.509 secrets and still accepts instance secrets.

> ### Example:  
> > ### Sample Code:  
> > ```
> > "oauth2-configuration": {
> >     "credential-types": ["binding-secret","x509","instance-secret"]
> > }
> > ```

In the following example, the service instance creates a binding secret for all new bindings, but doesn’t accept the instance secret or the use of X.509 secrets.

> ### Example:  
> > ### Sample Code:  
> > ```
> > "oauth2-configuration": {
> >     "credential-types": ["binding-secret"]
> > }
> > ```

**Related Information**  


[Rotate Binding Secrets](rotate-binding-secrets-618441b.md "Service instances of the SAP Authorization and Trust Management service use different binding secrets for each binding. To rotate binding secrets, unbind and rebind any consuming applications.")

[Rotating Instance Secrets](rotating-instance-secrets-8bfbbf5.md "When configured for instance secrets, a service instance of the SAP Authorization and Trust Management service uses the same instance secret for all bindings. You can't really rotate instance secrets, but must rotate the applications and service instance together.")

[Migrate from Instance Secrets to Binding Secrets](migrate-from-instance-secrets-to-binding-secrets-dcee867.md "To simplify the management of secrets for service instances of the SAP Authorization and Trust Management service, we recommend that you configure service instances to use binding secrets.")

[Application Security Descriptor Configuration Syntax](../30-development/application-security-descriptor-configuration-syntax-517895a.md "The syntax required to set the properties and values defined in the xs-security.json application security descriptor file.")

