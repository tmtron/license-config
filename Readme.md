# License Configuration

This project contains some configuration to make the [License Maven Plugin](http://code.mycila.com/license-maven-plugin/) compatible to the [IntelliJ Idea IDE](https://www.jetbrains.com/idea/).

It contains a custom [Header style definition](http://code.mycila.com/license-maven-plugin/#changing-header-style-definitions) which matches the format of the Idea XML headers. 
 
## Usage

You want to make sure that the Idea configuration exactly matches the _License Maven Plugin_ configuration. 

### Maven

#### Maven Configuration
Example configuration for the _License Maven Plugin_ in the (parent) pom of the project: 

```xml
<project>
    <properties>
        <owner.name>Martin Trummer</owner.name>
        <owner.email>martin.trummer@tmtron.com</owner.email>
    </properties>

    <build>
        <plugins>
            <plugin>
                <groupId>com.mycila</groupId>
                <artifactId>license-maven-plugin</artifactId>
                <dependencies>
                    <dependency>
                        <groupId>com.tmtron</groupId>
                        <artifactId>license-maven-plugin-config</artifactId>
                        <version>1.0.0</version>
                    </dependency>
                </dependencies>
    
                <configuration>
                    <header>com/mycila/maven/plugin/license/templates/APACHE-2.txt</header>
                    <properties>
                        <owner>${owner.name}</owner>
                        <email>${owner.email}</email>
                    </properties>
                    <excludes>
                        <exclude>**/README</exclude>
                        <exclude>**/LICENSE</exclude>
                        <exclude>**/NOTICE</exclude>
                        <exclude>src/test/resources/**</exclude>
                        <exclude>src/main/resources/**</exclude>
                    </excludes>
                    <mapping>
                        <java>SLASHSTAR_STYLE</java>
                        <xml>idea_xml_style</xml>
                    </mapping>
                    <headerDefinitions>
                        <headerDefinition>idea-xml-header.xml</headerDefinition>
                    </headerDefinitions>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
     </build>
</project>     
```

Notes:
 * the `license-maven-plugin` must define a dependency to `license-maven-plugin-config`, so that the [idea-xml-header.xml](src/main/resources/idea-xml-header.xml) file can be found from the classpath.
 * we must add a `headerDefinition` item, to read the `idea-xml-header.xml` file
 * finally we can use the `idea_xml_style` for the xml-mapping.
 * we also add an execution section, so that the license-check will be done during the maven [verify](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html) build phase  

#### Maven Execution
When you want to add missing file-headers to your project files, just run:

    mvn license:format
 
### Idea configuration
1. _File_ - _Settings_ - _Copyright_- _Copyright profiles_
   * Click the + button and add a new profile called `Apache2`
   * Add the copyright text - see section [Idea Copyright Text](#idea-copyright-text) below        
1. _File_ - _Settings_ - _Copyright_  
   _Default project copyright_: select `Apache2`  
    
#### Idea Copyright Text
Notes:
 * we use the copyright symbol, because the _License Maven Plugin_ also replaces (C) with the symbol automatically.
 * we use the `$today.year` variable in this template
 * this text must exactly match whath the _License Maven Plugin_ expects. 

```
Copyright © $today.year Martin Trummer (martin.trummer@tmtron.com)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

## Details

The configuration is required because the _License Maven Plugin_ and Idea use different formats for the copyright in XML headers.
* Idea uses `  ~ ` at the beginning of each line
* _License Maven Plugin_ uses blanks only

## License

Licensed under the Apache License, Version 2.0 (the "License");  
you may not use this file except in compliance with the License.  
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software  
distributed under the License is distributed on an "AS IS" BASIS,  
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  
See the License for the specific language governing permissions and  
limitations under the License.
