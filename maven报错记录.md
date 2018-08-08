## maven报错记录

```java
C:\workspace\OTT\IPS_OTT\Branches\v1.3\ipupgrade>mvn  install
[INFO] Scanning for projects...
[WARNING]
[WARNING] Some problems were encountered while building the effective model for com.gx:ipupgrade:war:1.3.04
[WARNING] 'build.plugins.plugin.version' for org.apache.maven.plugins:maven-resources-plugin is missing. @ line 353, column 12
[WARNING]
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING]
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
[WARNING]
[INFO]
[INFO] --------------------------< com.gx:ipupgrade >--------------------------
[INFO] Building ipupgrade 1.3.04
[INFO] --------------------------------[ war ]---------------------------------
Downloading from central: https://repo.maven.apache.org/maven2/android/AXMLPrinter2/1.0.0/AXMLPrinter2-1.0.0.pom
[WARNING] The POM for android:AXMLPrinter2:jar:1.0.0 is missing, no dependency information available
[WARNING] The artifact aspectj:aspectjweaver:jar:1.5.4 has been relocated to org.aspectj:aspectjweaver:jar:1.5.4
[WARNING] The artifact jstl:jstl:jar:1.1.2 has been relocated to javax.servlet:jstl:jar:1.1.2
[INFO]
[INFO] --- native2ascii-maven-plugin:1.0-alpha-1:native2ascii (native2ascii-utf8) @ ipupgrade ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.525 s
[INFO] Finished at: 2018-08-08T13:58:36+08:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.codehaus.mojo:native2ascii-maven-plugin:1.0-alpha-1:native2ascii (native2ascii-utf8) on project ipupgrade: Execution native2ascii-utf8 of goal org.codehaus.mojo:native2ascii-maven-plugin:1.0-alpha-1:native2ascii failed: Error starting Sun's native2ascii: : sun.tools.native2ascii.Main -> [Help 1]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginExecutionException
```

问题

```java
[ERROR] Failed to execute goal org.codehaus.mojo:native2ascii-maven-plugin:1.0-alpha-1:native2ascii (native2ascii-utf8) on project ipupgrade: Execution native2ascii-utf8 of goal org.codehaus.mojo:native2ascii-maven-plugin:1.0-alpha-1:native2ascii failed: Error starting Sun's native2ascii: : sun.tools.native2ascii.Main -> [Help 1]
```

解决

> Copying %Java_Home%/lib/tools.jar to  %Java_Home%/jre/lib/ext/tools.jar 

