# JBoss AS7 Bouncy Castle Modules

## Installation

The modules can be build to 
`<module>/target/modules/org/bouncycastle/<module>/main` with
the `mvn package` goal or installed directly to an JBoss AS7
installation with the `mvn install` goal. To install directly the
`JBOSS_HOME` environment variable must be set to the base directory of
the JBoss server.

The available modules are:
- jdk12
- jdk14
- jdk15
- jdk15on
- jdk16

These modules contain all corresponding jars identified by that postfix
in maven central. You can choose which ones to install by setting the
build profile. E.g. `mvn install -Pjdk16` will only install the jdk16
module. You can use the profile `mvn install -Pall` to install all of
them. The module `jdk15on` will be used if no build profile is provided.

To manually install the modules to an AS7 server copy the directory
`<module>/target/modules/org/bouncycastle/<module>/main` into the
corresponding module directory on the server
`$JBOSS_HOME/modules/org/bouncycastle/<module>/main`.

## Usage

You can activate these modules in two ways.

1. Add a jboss-deployment-structure.xml to your project containing:

```
<jboss-deployment-structure>
    <deployment>
        <dependencies>
            <module name="org.bouncycastle.jdk16" slot="main" export="true" />
        </dependencies>
    </deployment>
</jboss-deployment-structure>
```

2. Add a dependency on that module to the manifest of your jar/war/ear.
   E.g. using maven you could have:

```
    <dependencies>
        <dependency>
            <groupId>org.bouncycastle</groupId>
            <artifactId>bcprov-jdk16</artifactId>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>my-jar</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifestEntries>
                            <Dependencies>
                                org.bouncycastle.jdk16
                            </Dependencies>
                        </manifestEntries>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

The maven-jar-plugin, maven-war-plugin, maven-ear-plugin and
maven-ejb-plugin are all configured in this manner.

That's it. You should now be able to use Bouncy Castle functionality in your projects.

Last updated: 2013-01-31 (Brent Douglas)
