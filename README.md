# springboot-security

https://www.youtube.com/watch?v=mD3vmgksvz8

C:\Liu\TechTemp\jwt-spring-security-demo-master

-->
user/passowrd
admin/admin




--> pom.xml: try findbug, jcoco and spotify ???

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.zerhusen</groupId>
    <artifactId>jwt-spring-security-demo</artifactId>
    <version>0.2.0</version>
    <packaging>jar</packaging>

    <name>jwt-spring-security-demo</name>
    <description>Demo project for JWT with Spring Boot and Spring Security</description>

    <licenses>
        <license>
            <name>MIT license</name>
            <url>https://raw.githubusercontent.com/szerhusenBC/jwt-spring-security-demo/master/LICENSE</url>
        </license>
    </licenses>

    <developers>
        <developer>
            <name>Stephan Zerhusen</name>
            <email>stephan.zerhusen@gmail.com</email>
        </developer>
    </developers>

    <scm>
        <connection>scm:git:git://github.com/szerhusenBC/jwt-spring-security-demo.git</connection>
        <url>https://github.com/szerhusenBC/jwt-spring-security-demo</url>
        <developerConnection>scm:git:git@github.com/szerhusenBC/jwt-spring-security-demo.git</developerConnection>
        <tag>HEAD</tag>
    </scm>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>

        <jjwt.version>0.7.0</jjwt.version>
        <jacoco.version>0.7.9</jacoco.version>

        <maven-compiler-plugin.version>3.6.1</maven-compiler-plugin.version>
        <findbugs-maven-plugin.version>3.0.4</findbugs-maven-plugin.version>
        <docker-maven-plugin.version>0.4.13</docker-maven-plugin.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mobile</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>${jjwt.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>${maven-compiler-plugin.version}</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>${docker-maven-plugin.version}</version>

                <configuration>
                    <imageName>hubae/jwt-spring-security-demo</imageName>
                    <baseImage>openjdk:8</baseImage>
                    <imageTags>
                        <imageTag>${project.version}</imageTag>
                        <imageTag>latest</imageTag>
                    </imageTags>

                    <entryPoint>["java", "-jar", "/${project.build.finalName}.jar"]</entryPoint>

                    <!-- copy the service's jar file from target into the root directory of the image -->
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>

                    <serverId>docker-hub</serverId>
                    <registryUrl>https://index.docker.io/v1/</registryUrl>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <profiles>
        <profile>
            <id>findbugs</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.codehaus.mojo</groupId>
                        <artifactId>findbugs-maven-plugin</artifactId>
                        <version>${findbugs-maven-plugin.version}</version>
                        <configuration>
                            <excludeFilterFile>${basedir}/etc/findbugs-exclude-filter.xml</excludeFilterFile>
                        </configuration>
                        <executions>
                            <execution>
                                <phase>compile</phase>
                                <goals>
                                    <goal>check</goal>
                                </goals>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>
        </profile>

        <profile>
            <id>jacoco</id>
            <build>
                <plugins>
                    <plugin>
                        <groupId>org.jacoco</groupId>
                        <artifactId>jacoco-maven-plugin</artifactId>
                        <version>${jacoco.version}</version>
                        <executions>
                            <execution>
                                <id>prepare-agent</id>
                                <goals>
                                    <goal>prepare-agent</goal>
                                </goals>
                            </execution>
                            <execution>
                                <id>report</id>
                                <phase>prepare-package</phase>
                                <goals>
                                    <goal>report</goal>
                                </goals>
                            </execution>
                            <execution>
                                <id>post-unit-test</id>
                                <phase>test</phase>
                                <goals>
                                    <goal>report</goal>
                                </goals>
                                <configuration>
                                    <!-- Sets the path to the file which contains the execution data. -->

                                    <dataFile>target/jacoco.exec</dataFile>
                                    <!-- Sets the output directory for the code coverage report. -->
                                    <outputDirectory>target/jacoco-ut</outputDirectory>
                                </configuration>
                            </execution>
                        </executions>
                    </plugin>
                </plugins>
            </build>
        </profile>
    </profiles>

</project>





