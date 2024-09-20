NewRelic Instrumentation Plugin
================================================================================

Plugin that automatically creates XML file for NewRelic custom instrumentation.

refs: https://docs.newrelic.com/docs/agents/java-agent/custom-instrumentation/java-instrumentation-xml


Requirements
--------------------------------------------------------------------------------

* Java 8+


Usage
--------------------------------------------------------------------------------

Configure plugins section in pom.xml file.

```xml
<plugin>
    <groupId>com.yo1000</groupId>
    <artifactId>newrelic-instrumentation-maven-plugin</artifactId>
    <version>1.0.8</version>
    <executions>
        <execution>
            <goals>
                <goal>newrelic-instrumentation</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

When Run the Maven build,
NewRelic custom instrumentation XML will be created.

```
mvn clean compile

ls -l target/newrelic-instrumentation/extensions/
total 4
-rwxrwxrwx 1 ubuntu ubuntu 1931 Dec  4 16:13 newrelic-extension.xml

cat target/newrelic-instrumentation/extensions/newrelic-extension.xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<extension enabled="true" name="newrelic-extension" version="1.0">
    <instrumentation>
        <pointcut transactionStartPoint="true">
            <className>...
```

### Configuration parameters

Configurable parameters are follows.

#### outputDirectory

Compiled class files location.

defaults: `${project.build.outputDirectory}`

#### namespaceUri

XML namespace.

defaults: `https://newrelic.com/docs/java/xsd/v1.0`

#### name

Custom instrumentation XML file name.

defaults: `newrelic-extension`

#### version

Custom instrumentation XML version.
If any same named instrumentation XML, only enable to have the latest version.

defaults: `1.0`

#### enabled

Enable Custom instrumentation XML.

defaults: `true`

#### manuallyDefinitions

Custom instrumentation additional pointcut configs by manually.
Method definition separator is space characters. (e.g., ` `, `\t`, `\n`)
Inner class name separator is not `$`, use be `-`.

defaults: _empty_

examples:

```xml
<manuallyDefinitions>
    <java.util.ArrayList>
        get add set
    </java.util.ArrayList>
    <com.yo1000.demo.ExampleList>
        get
        add
        set
    </com.yo1000.demo.ExampleList>
    <com.yo1000.demo.Foo-Bar>
        baz
    </com.yo1000.demo.Foo-Bar>
</manuallyDefinitions>
```

#### asm

ASM API version. Can choose from `5`, `6`, `7`, `8`, `9`.

defaults: `9`


How to Build
--------------------------------------------------------------------------------

Install to Maven local repository.

```
./mvnw clean install
```


How to Release
--------------------------------------------------------------------------------

```bash
export GPG_TTY=$(tty)
gpg-agent

# Requires input GPG Key passphrase
gpg --import /path/to/import-gpg-key.asc
gpg --list-keys

# Requires input GPG Key passphrase
./mvnw clean deploy -Prelease -s /path/to/settings.xml -U
```
