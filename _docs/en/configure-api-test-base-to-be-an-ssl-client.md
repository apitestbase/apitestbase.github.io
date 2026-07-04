---
title: Configure API Test Base to be an SSL Client
permalink: /docs/en/configure-api-test-base-to-be-an-ssl-client
key: docs-configure-api-test-base-to-be-an-ssl-client
---
Sometimes API Test Base needs to connect to a service through SSL/TLS protected transport. Examples:
* HTTP request connects to a REST API that requires client certificate authentication.
* ACE request connects to an integration node that requires SSL.

In these cases, prepare a truststore for API Test Base and import the certificate from the remote service into it.

## Prepare the Truststore
If you don't already have a truststore, or you have one but the certificate is not in it, run a command like the one below. The `keytool` utility comes with a Java (JDK or JRE) installation.

`<Java_Home>/bin/keytool -importcert -file abc.cer -alias <certificate_alias> -keystore truststore.jks -storepass <truststore_password> -noprompt`

Here
* `abc.cer` is the SSL certificate file from the remote service.
* `truststore.jks` is the truststore file. It will be created if not already existing.
* `truststorepass` is the truststore password API Test Base expects by default. To use a different password, see the configuration below.

For client certificate authentication (mutual TLS), also import the client certificate together with its private key (for example from a PKCS#12 file, via `keytool -importkeystore`) into the same truststore file — API Test Base uses this single file for both trusted server certificates and the client certificate.

## Configure API Test Base
Copy the truststore file to `<ATB_DATA_DIR>` (refer to [Administration](/docs/en/administration) for its location), making sure it is named `truststore.jks` — API Test Base looks for the truststore at exactly `<ATB_DATA_DIR>/truststore.jks`.

The truststore password API Test Base uses defaults to `truststorepass`. If your truststore has a different password, set the `atb.app.ssl.trust.store.password` option in the `<ATB_DATA_DIR>/config.properties` file.

Restart API Test Base for a truststore or configuration change to take effect.

## Note
HTTP or SOAP request in API Test Base trusts all remote SSL certificates, hence no need to configure SSL for them if the remote API doesn't require client certificate authentication.

## Reference
* [The Most Common Java Keytool Keystore Commands](https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html)