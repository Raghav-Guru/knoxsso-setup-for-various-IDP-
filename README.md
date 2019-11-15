-->Create SAML client in keycloak. (review screenshot for client configuration). 

--> Create user account under the realm and set password. 

--> Get the IDP SSO metadata using keycloak url : 

#curl -ik https://keycloak-FQDN:8443/auth/realms/KeycloakRealmName/protocol/saml/descriptor -o idp.xml

--> Copy idp.xml file to the knox host. 

--> Configure knoxsso topology and set the identityProviderMetadataPath

                 <param>
                    <name>saml.identityProviderMetadataPath</name>
                    <value>/etc/knox/conf/idp.xml</value>
                  </param>
 
 --> Setup any service for SSO authentication and verify the SSO redirection and authentication 
