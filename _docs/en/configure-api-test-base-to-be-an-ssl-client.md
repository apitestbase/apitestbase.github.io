---
title: Configure API Test Base to be an SSL Client
permalink: /docs/en/configure-api-test-base-to-be-an-ssl-client
key: docs-configure-api-test-base-to-be-an-ssl-client
---
Sometimes API Test Base needs to connect to a service through SSL/TLS protected transport. Examples:
* HTTP request connects to a REST API that requires client certificate authentication.
* ACE test step connects to an integration node that requires SSL.

In these cases, we need to prepare a truststore for API Test Base and import the certificate from the remote service into the truststore.

If you don't already have a truststore, or you have a truststore but don't have the certificate in it, run command like below to prepare the truststore.

`<Java_Home>/bin/keytool -importcert -file abc.cer -alias <certificate_alias> -keystore truststore.jks -storepass <truststore_password> -noprompt`

Here
* abc.cer is the SSL certificate file.
* truststore.jks is the filename of the truststore. It will be created if not already existing.

Copy the truststore file (here truststore.jks) to `<ATB_DATA_DIR>`. An app/config.yml file under the API Test Base installation directory contains corresponding settings (sslTrustStorePath, sslTrustStorePassword) for using the truststore. The settings can be overridden in the `<ATB_DATA_DIR>/config.properties` file. 

Note
* API Test Base application needs to be restarted for truststore change.
* HTTP or SOAP request in API Test Base trusts all remote SSL certificates, hence no need to configure SSL for them if the remote API doesn't require client certificate authentication.

Reference

* https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html?jn9ed3e997=3