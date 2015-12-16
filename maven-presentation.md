Agenda

Calculator application

 - Simple pom.xml
 - Add unit test
 - Add dependency
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

Start maven:

mvn clean package

Result:
```
[WARNING] JAR will be empty - no content was marked for inclusion!
[INFO] Building jar: /Users/vakol/Documents/project java/calculator/target/calculator-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.978s
[INFO] Finished at: Wed Dec 16 10:39:37 EET 2015
[INFO] Final Memory: 17M/491M
[INFO] ------------------------------------------------------------------------
```

Add unit test
=============

mkdir src/test/java/com/github/javadev/calculator
cd src/test/java/com/github/javadev/calculator
touch CalculatorTest.java

```java
package com.github.javadev.calculator;

import org.junit.Test;
import static org.junit.Assert.assertEquals;

public class CalculatorTest {
    @Test
    public void should_be_right_add_calculation() {
        Calculator calc = new Calculator();
        calc.init(2);
        calc.add(3);
        assertEquals(5, calc.result());
    }
}
```

Add dependency to the pom.xml
=============================

```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

Add logic:
==========
Source file Calculator.java in directory `src/main/java/com/github/javadev/calculator

```java
package com.github.javadev.calculator;

public class Calculator {
    private int value;
    
    public void init(int value) {
        this.value = value;
    }
    
    public int add(int localValue) {
        value += localValue;
        return value; 
    }
    
    public int result() {
        return value;
    }
}

```

Start maven:

mvn clean package

```
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.github.javadev.calculator.CalculatorTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.065 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ calculator ---
[INFO] Building jar: /Users/vakol/Documents/project java/calculator/target/calculator-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.017s
[INFO] Finished at: Wed Dec 16 12:06:17 EET 2015
[INFO] Final Memory: 15M/491M
[INFO] ------------------------------------------------------------------------
```
