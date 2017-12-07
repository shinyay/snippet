# JUnit(Gradle) と Selenium(Maven) のテストレポート
---
## JUnit - Gradle

```
build/test-results/test/*.xml
```

### JUnitのテストコードとIntegration Testコードが混在する場合
exclude でITコードを除外

```
test {
    exclude 'it/**'
}
```

- パッケージで分離
- ソースコード名で分離
  - **maven-failsafe-plugin** を利用する場合、標準では **xxxIT.java** という命名規則

## Selenium - Maven

```
target/failsafe-reports/*.xml
```
### selenium-java

- バージョンが **2.53.1** 以前であれば、GeckoDriverは不要

- Selenium 3 以降は、**GeckoDriver** を利用する必要あり

### pom.xml

```xml
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.seleniumhq.selenium</groupId>
      <artifactId>selenium-java</artifactId>
      <!-- <version>3.8.1</version> -->
      <version>2.53.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.maven.surefire</groupId>
      <artifactId>surefire-junit47</artifactId>
      <version>2.20.1</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>seleniumdemo</finalName>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.20.1</version>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-failsafe-plugin</artifactId>
        <version>2.20.1</version>
        <dependencies>
          <dependency>
            <groupId>org.apache.maven.surefire</groupId>
            <artifactId>surefire-junit47</artifactId>
            <version>2.20.1</version>
          </dependency>
        </dependencies>
        <executions>
          <execution>
            <id>integration-tests</id>
            <phase>integration-test</phase>
            <goals>
              <goal>integration-test</goal>
              <goal>verify</goal>
            </goals>
            <configuration>
              <skip>false</skip>
              <excludes>
                <exclude>none</exclude>
              </excludes>
              <includes>
                <include>**/it/**/*IT.java</include>
              </includes>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
```