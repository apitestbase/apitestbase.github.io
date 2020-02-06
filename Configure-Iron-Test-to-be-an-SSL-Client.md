Sometimes Iron Test needs to connect to a service through SSL protected transport. For example, IIB test step connects to an IIB v10 integration node via IIB endpoint that uses SSL. In this case, we need to prepare a truststore for Iron Test and import the certificate from the integration node web console into the truststore.

If you don't already have a truststore, or you have a truststore but don't have the certificate in it, run command like below to prepare the truststore.

`<Java_Home>/bin/keytool -importcert -file abc.cer -alias <certificate_alias> -keystore truststore.jks -storepass <truststore_password> -noprompt`

Here
* abc.cer is the SSL certificate file (like that extracted from the IIB integration node web console.
* truststore.jks is the filename of the truststore. It will be created if not already existing.

Copy the truststore file (here truststore.jks) to <IronTest_Home>. The config.yml contains corresponding settings (sslTrustStorePath, sslTrustStorePassword) for using the truststore. Adjust their values accordingly.

Note
* Iron Test application needs to be restarted for truststore change.
* HTTP or SOAP test step in Iron Test trusts all SSL certificates, hence no need to configure SSL in Iron Test for using them.

Reference

https://www.sslshopper.com/article-most-common-java-keytool-keystore-commands.html?jn9ed3e997=3