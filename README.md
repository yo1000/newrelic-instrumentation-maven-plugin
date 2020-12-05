# NewRelic Instrumentation Plugin
Plugin that automatically creates XML file for NewRelic custom instrumentation.

refs: https://docs.newrelic.com/docs/agents/java-agent/custom-instrumentation/java-instrumentation-xml

## Requirements
* JDK8+

## Usage
Configure plugins section in pom.xml file.

```xml
<plugin>
    <groupId>com.yo1000</groupId>
    <artifactId>newrelic-instrumentation-maven-plugin</artifactId>
    <version>1.0.1</version>
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
<extension name="newrelic-extension">
    <instrumentation>
        <pointcut transactionStartPoint="true">
            <className>...
```

### Configuration parameters
Configurable parameters are follows.

#### outputDirectory
Compiled class files location.

defaults: `${project.build.outputDirectory}`

#### name
Custom instrumentation XML file name.

defaults: `newrelic-extension`

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

## How to Build
Install to Maven local repository.

```
./mvnw clean install
```
