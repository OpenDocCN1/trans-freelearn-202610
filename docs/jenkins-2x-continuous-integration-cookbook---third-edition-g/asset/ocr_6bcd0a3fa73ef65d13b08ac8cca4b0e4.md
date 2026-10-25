# # How to do it...

+   1. Create a template file and name it `pom_ant_simple.xml`.
+   2. Change the values of `groupId`, `artifactId`, `version`, and `name` to suit your preferences.
+   3. Add the following XML fragment just before the `</project>` tag:

```
```xml
<build>
  <plugins><plugin>
    <groupId>org.codehaus.gmaven</groupId>
    <artifactId>gmaven-plugin</artifactId>
    <version>1.3</version>
    <executions>
      <execution>
        <id>run-myGroovy-test</id>
        <goals><goal>execute</goal></goals>
        <phase>test</phase>
        <configuration>
          <source>
            def ant = new AntBuilder()
            ant.echo("\n\nTested ----> With Groovy")
          </source>
        </configuration>
      </execution>
      <execution>
        <id>run-myGroovy-verify</id>
        <goals><goal>execute</goal></goals>
        <phase>verify</phase>
        <configuration>
          <source>
            def ant = new AntBuilder()
            ant.echo("\n\nVerified at ${new Date()}")
          </source>
        </configuration>
      </execution>
    </executions>
  </plugin></plugins>
</build>
```
```

+   4. Run `mvn test -f pom_ant_simple.xml`. Review the output (note that there are no warnings about empty JAR files):
+   5. Run `mvn verify -f pom_ant_simple.xml`. Review the output; it should look similar to the following screenshot: