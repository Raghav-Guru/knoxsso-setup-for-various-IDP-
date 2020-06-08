**Keycloak - SAML**  : 

- Create SAML client in keycloak.

  Sample config :  [keycloak-saml-client.png](https://github.com/Raghav-Guru/knoxsso-setup-for-various-IDP-/blob/master/keycloak-saml-client.png)

- Create user account under the realm and set password. 

- Get the IDP SSO metadata using keycloak url : 

```bash
# curl -ik https://keycloak-FQDN:8443/auth/realms/KeycloakRealmName/protocol/saml/descriptor -o idp.xml
```

- Copy idp.xml file to the knox host. 

- Configure knoxsso topology and set the identityProviderMetadataPath

                 <param>
                    <name>saml.identityProviderMetadataPath</name>
                    <value>/etc/knox/conf/idp.xml</value>
                  </param>

- Setup any service for SSO authentication and verify the SSO redirection and authentication.

- Sample config [knoxsso.xml_saml_keycloak](https://github.com/Raghav-Guru/knoxsso-setup-for-various-IDP-/blob/master/knoxsso.xml_saml_keycloak)

  (Changes required for properties pac4j.callbackUrl, saml.identityProviderMetadataPath,saml.serviceProviderMetadataPath and saml.serviceProviderEntityId)

**Azure AD - SAML:** 

- In Azure AD create a custom SAML application, Only mention the entityID(user defined Name) and ACS endpoint which should be the knox SSO url like `https://<knoxHost>:8443/gateway/knoxsso/api/v1/websso`.

**Note** :  As we need IDP Initiated SSO, make sure not to specify the Sign on URL. 

Sample Config : [ms-azure-saml.png](https://github.com/Raghav-Guru/knoxsso-setup-for-various-IDP-/blob/master/ms-azure-saml.png)

- On Knox Host, create metadata file in the format 

```xml
<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" entityID="http://www.IdP.com/entity_ID">
   <md:IDPSSODescriptor WantAuthnRequestsSigned="false" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
      <md:KeyDescriptor use="signing">
         <ds:KeyInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
              <ds:X509Data><ds:X509Certificate>x509-certificate</ds:X509Certificate></ds:X509Data>
         </ds:KeyInfo>
      </md:KeyDescriptor>
      <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress</md:NameIDFormat>
      <md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
           Location="https://login.microsoftonline.com/..."/>
      <md:SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect"
           Location="https://login.microsoftonline.com/..."/>
   </md:IDPSSODescriptor>
</md:EntityDescriptor>
```

- Refer sample knoxsso config with Azure AD [knoxsso-msazure.xml](https://github.com/Raghav-Guru/knoxsso-setup-for-various-IDP-/blob/master/knoxsso-msazure.xml)

  (Changes required for saml.identityProviderMetadataPath saml.serviceProviderMetadataPath , saml.serviceProviderEntityId and knoxsso.redirect.whitelist.regex)

- Setup any service for SSO authentication and verify the SSO redirection and authentication.

  
