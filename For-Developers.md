Pull requests are welcome.

To launch Iron Test in your IDE (such as IntelliJ IDEA) without producing dist files, under the project directory (in which there is README.md) run below Maven command

    //  no MQ or IIB testing capabilities.
    mvn pre-integration-test -pl irontest-assembly -am -P dev   

If you work with irontest-mq module or irontest-iib module, first use `mvn install:install-file` to install related jars into your local Maven repository. Refer to corresponding pom.xml and this [wiki page](https://github.com/zheng-wang/irontest/wiki/Interact-with-Other-Systems) for more information about the dependencies and jars. Then copy IIB jars to <Workspace_Dir>/lib/iib/v90 and/or <Workspace_Dir>/lib/iib/v100. Finally, run commands like below

    //  with MQ 8.0 but no IIB testing capabilities
    mvn pre-integration-test -pl irontest-assembly -am -P dev -Dmq.version=8.0.0.7 -Dmq.version.is80
    //  with IIB 10.0 but no MQ testing capabilities
    mvn pre-integration-test -pl irontest-assembly -am -P dev -Diib.version=10.0.0.9 -Diib.version.is100