+++
date = "2013-12-31T20:44:54+01:00"
tags = ["Java", "Maven"]
title = "Adding javadoc to Netbeans with Maven"

+++

Often when I’m using Maven and include some dependency to my project e.g. hibernate-core Netbeans doesn’t automatically include Javadocs. As a result whenever i try to gain some  info through CTRL+Space I get:

> *Javadoc not found. Either Javadoc documentation for this item does not exist or you have not added specified Javadoc in the Java Platform Manager or the Library Manager.*

I tried to include javadoc URLs through Netbeans UI but with no luck. Eventually I found excellent, neat solution on the  Ted Wise blog:

Just use your cmd and type:

```bash
mvn dependency:sources
mvn dependency:resolve -Dclassifier=javadoc
```

*The first command will attempt to download source code for each of the dependencies in your pom file.The second command will attempt to download the Javadocs.*

I’m reblogging this info because it took me a lot of time to find above solution.