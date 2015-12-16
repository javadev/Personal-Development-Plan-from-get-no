Agenda

Calculator application

 - Simple pom.xml
 - Add unit test
 - Add dependency
 - create UI
 - Add logic
 - Combine jar in one
 - Deploy jar to central maven repository
 
Simple pom.xml
==============

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.github.javadev</groupId>
    <artifactId>calculator</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>Calculator</name>
    
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```    
