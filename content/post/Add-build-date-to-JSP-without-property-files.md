+++
date = "2013-11-28T22:21:03+01:00"
tags = ["Java", "Maven"]
title = "Adding build date to JSP with Maven without property files."

+++

It's often a useful thing to add a build time stamp or SVN revision number to your webapp. It may be placed in a footer, or treated as query string for CSS, JS or other statics files. I know it is possible to add such information to `*.property` file and then fetch it from JSP. What I wasn't aware is the fact that I can do it easier, without any external property files.

Add to web.xml file the following context-param.
```xml
<context-param>
	<param-name>buildTimeStamp</param-name>
	<param-value>${timestamp}</param-value>
</context-param>
```
The context-param element contains the declaration of a web application's servlet context initialization parameters.
Now from the JSP file you have access to this parameter by using: ```${initParam.buildTimeStamp}```. But of course at this point the result would be just a string ```${timestamp}```. So in a third step we need to "marry" the Maven property with the web.xml.
In the pom.xml add new property named the same as in web.xml. In this example it's value is maven build timestamp, but it can be whatever you like.

```xml
<properties>
	<timestamp>${maven.build.timestamp}</timestamp>
</properties>
```

Next in maven-war-plugin configuration add web resource and enable filtering for it.

```xml
<configuration>
	<webResources>
		<resource>
		<directory>${basedir}/src/main/webapp/WEB-INF</directory>
		<filtering>true</filtering>
		<targetPath>WEB-INF</targetPath>
		<includes>
			<include>**/web.xml</include>
		</includes>
		</resource>
	</webResources>
</configuration>
```

Thanks to that every occurrence of a timestamp variable in web.xml is replaced by actual value specified in the POM file. In this case: ${maven.build.timestamp}.

Source code on the Github.
