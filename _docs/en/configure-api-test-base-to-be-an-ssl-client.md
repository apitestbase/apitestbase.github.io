---
title: Configure API Test Base to be an SSL Client
permalink: /docs/en/configure-api-test-base-to-be-an-ssl-client
key: docs-configure-api-test-base-to-be-an-ssl-client
---
Sometimes API Test Base needs to connect to a service through SSL protected transport. For example, ACE test step connects to an integration node via ACE endpoint that uses SSL. In this case, we need to prepare a truststore for API Test Base and import the certificate from the integration node web console into the truststore.

If you don't already have a truststore, or you have a truststore but don't have the certificate in it, run command like below to prepare the truststore.

`<Java_Home>/bin/keytool -importcert -file abc.cer -alias <certificate_alias> -keystore truststore.jks -storepass <truststore_password> -noprompt`

Here
* abc.cer is the SSL certificate file (like that extracted from browser after opening the ACE integration node web console).
* truststore.jks is the filename of the truststore. It will be created if not already existing.

Copy the truststore file (here truststore.jks) to `<ATB_DATA_DIR>`. An app/config.yml file under the API Test Base installation directory contains corresponding settings (sslTrustStorePath, sslTrustStorePassword) for using the truststore.

Note
* API Test Base application needs to be restarted for truststore change.
* HTTP or SOAP request in API Test Base trusts all SSL certificates, hence no need to configure SSL for them.

Reference

* https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html?jn9ed3e997=3