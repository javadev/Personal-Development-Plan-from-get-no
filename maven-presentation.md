Agenda

Calculator application

 - Simple pom.xml
 - Add unit test
 - Add dependency
 - Add logic
 - Combine jar in one
 - Deploy jar to central maven repository
 - Example web application with JAX-RS and enpoint
 
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

Combine jar in one (maven-shade-plugin)
=======================================

Add maven plugin to the build section:

```xml
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>1.4</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <filters>
                                <filter>
                                    <artifact>*:*</artifact>
                                    <excludes>
                                        <exclude>META-INF/maven/**</exclude>
                                        <exclude>META-INF/COPYRIGHT.html</exclude>
                                        <exclude>META-INF/LICENSE*</exclude>
                                        <exclude>META-INF/NOTICE*</exclude>
                                        <exclude>META-INF/README.txt</exclude>
                                        <exclude>META-INF/DEPENDENCIES*</exclude>
                                        <exclude>LICENSE.txt</exclude>
                                        <exclude>rhinoDiff.txt</exclude>
                                        <exclude>license/**</exclude>
                                    </excludes>
                                </filter>
                            </filters>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>            
```

Deploy jar to central maven repository
======================================

Add `name`, `description` and `url` properties:

```xml
  <name>${project.groupId}:${project.artifactId}</name>
  <description>My jar module</description>
  <url>https://github.com/user/application</url>
```

Add `developers` section:

```xml
  <developers>
    <developer>
      <name>Firstname Surname</name>
    </developer>
  </developers>
```

Add `licenses` and `scm` sections:

```xml
  <licenses>
    <license>
      <name>The MIT License</name>
      <url>http://opensource.org/licenses/MIT</url>
      <distribution>repo</distribution>
    </license>
  </licenses>

  <scm>
    <connection>scm:git:https://github.com/username/application.git</connection>
    <developerConnection>scm:git:https://github.com/username/application.git</developerConnection>
    <url>https://github.com/username/application</url>
    <tag>HEAD</tag>
  </scm>
```

Add `properties` section:

```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>
```

Add `release` profile to the pom.xml file:

```xml
  <profiles>
    <profile>
      <id>release</id>
      <build>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-javadoc-plugin</artifactId>
            <version>2.10.1</version>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-gpg-plugin</artifactId>
            <version>1.4</version>
            <executions>
              <execution>
                <id>sign-artifacts</id>
                <phase>verify</phase>
                <goals>
                  <goal>sign</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
```

Example web application with JAX-RS and enpoint
===============================================

pom.xml file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.github</groupId>
  <artifactId>test-task-address-book</artifactId>
  <packaging>war</packaging>
  <version>1.0</version>
  <name>Test task, address book application</name>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <jetty.version>8.1.8.v20121106</jetty.version>
  </properties>
  
  <build>
    <plugins>
          <plugin>
          <groupId>org.mortbay.jetty</groupId>
          <artifactId>jetty-maven-plugin</artifactId>
          <version>${jetty.version}</version>
          <configuration>
              <webAppConfig>
                  <contextPath>/</contextPath>
              </webAppConfig>
              <connectors>
                  <connector implementation="org.eclipse.jetty.server.bio.SocketConnector">
                      <port>8080</port>
                      <maxIdleTime>3600000</maxIdleTime>
                  </connector>
              </connectors>
          </configuration>
      </plugin>
    </plugins>
  </build>
  <dependencies>
        <dependency>
            <groupId>org.jboss.resteasy</groupId>
            <artifactId>resteasy-jaxrs</artifactId>
            <version>2.2.1.GA</version>
        </dependency>
        <dependency>
            <groupId>org.jboss.resteasy</groupId>
            <artifactId>resteasy-jackson-provider</artifactId>
            <version>2.2.1.GA</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>4.3.6.Final</version>
            <exclusions>
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.1.2.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hsqldb</groupId>
            <artifactId>hsqldb</artifactId>
            <version>2.3.2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
  </project>
```

addressbook/entity/Contact.java

```
@Entity
@Table(name = "contact")
public class Contact implements Serializable {

    private static final long serialVersionUID =-2032908015L;
    private Long id;
    private String name;
    private Boolean done;

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", columnDefinition = "BIGINT", nullable = false)
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    @Size(max = 100)
    @Column(name = "name", columnDefinition = "VARCHAR(100)", nullable = false, length = 100)
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Column(name = "done", columnDefinition = "BIT(1)")
    public Boolean getDone() {
        return done;
    }

    public void setDone(Boolean done) {
        this.done = done;
    }

    @Override
    public String toString() {
        return "Contact@" + hashCode();
    }
}
```

addressbook/rest/ontactRestService.java

```java
@Path("/contacts")
public class ContactRestService {

    protected static EntityManager em;

    static {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("contactsDatabase");
        em = emf.createEntityManager();
    }

    @GET
    @Produces("application/json")
    public List<Contact> getContacts() {
        Query query = em.createQuery("select c from Contact c");
        return query.getResultList();
    }

    @POST
    @Consumes("application/json")
    public Response createContact(Contact contact) {
        em.getTransaction().begin();
        em.persist(contact);
        em.getTransaction().commit();
        String result = "Contact was created : " + contact;
        return Response.status(201).entity(result).build();
    }
    
    @PUT
    @Path("/{id}")
    @Consumes("application/json")
    public Response updateContact(@PathParam("id") Long id, Contact contact) {
        em.getTransaction().begin();
        em.merge(contact);
        em.getTransaction().commit();
        String result = "Contact was updated : " + contact;
        return Response.status(204).entity(result).build();
    }

    @DELETE
    @Path("/{id}")
    @Produces("application/json")
    public Response deleteContact(@PathParam("id") Long id) {
        em.getTransaction().begin();
        Query query = em.createQuery("delete from Contact c where c.id = :id");
        query.setParameter("id", id);
        query.executeUpdate();
        em.getTransaction().commit();
        String result = "Contact was deleted : " + id;
        return Response.status(204).entity(result).build();
    }
}
```

