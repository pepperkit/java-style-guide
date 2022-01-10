# Java Style Guide
![Version](https://img.shields.io/badge/Latest%20Version-v1.0.2--9.2-blue)
![Checkstyle](https://img.shields.io/badge/Checkstyle%20Compatible-v9.2.1-green)

Java style guide built upon Sun Microsystems and Google style guides, widely accepted best practices. Is offered in 
form of a textual representation, as well as `Checkstyle` and `Intellij IDEA` declarations for automation purposes.

[Checkstyle](https://checkstyle.sourceforge.io/) allows you to conduct static analysis of your Java code and
highlights its issues in the means of following the specified code style.
But it requires you to specify what is right and what is wrong. There is an official Sun Microsystems style guide, 
but it's heavily outdated. There are plenty of other style guides but some of them go too far with allowing some 
things, which can be considered a bad practice, and some are too strict.

If you struggle to choose a preferred style guide, this particular one is built upon _Sun Microsystems_ and _Google_ style 
guides and is constantly updated to conform to the latest versions of Checkstyle.
You can freely use this one, or create a new one - just fork this repository and modify the style guide as it
better suited to your needs.

- [How to use](#how-to-use)
- [Versioning](#versioning)
- [Style Guide](#style-guide)
    - [Project Structure](#project-structure)
    - [Source Code File](#source-code-file)
    - [License](#license)
    - [Import Statements](#import-statements)
    - [Java Class Structure](#java-class-structure)
    - [Formatting](#formatting)
    - [Naming](#naming)
    - [JavaDoc](#javadoc)
    - [Tests](#tests)
- [Resources](#resources)

## How to use
To force following your code style guide by all the developers in the team, it's recommended to include Checkstyle
static analysis into the Continuous Integration pipeline, which can be done with the use of the following plugins:
- [Apache Maven Checkstyle Plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/)
- [Gradle Checkstyle Plugin](https://docs.gradle.org/current/userguide/checkstyle_plugin.html)

### Apache Maven
Then the plugin could be configured in the following way (for Maven):
```xml
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>3.1.2</version>
            <dependencies>
                <dependency>
                    <groupId>com.puppycrawl.tools</groupId>
                    <artifactId>checkstyle</artifactId>
                    <version>9.2.1</version>
                </dependency>
            </dependencies>
            <configuration>
                <configLocation>https://raw.githubusercontent.com/pepperkit/java-style-guide/v1.0.2-9.2/checkstyle.xml</configLocation>
                <encoding>UTF-8</encoding>
                <consoleOutput>true</consoleOutput>
                <failsOnError>true</failsOnError>
                <linkXRef>false</linkXRef>
            </configuration>
        </plugin>
    </plugins>
```
Be aware of current versions.

### Suppressions
Checkstyle allows the definition of a list of files and their line ranges that should be suppressed from reporting any violations see [Using a Suppressions Filter](https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/suppressions-filter.html).

Update the plugin configuration like that:
```xml
<configuration>
    <configLocation>https://raw.githubusercontent.com/pepperkit/java-style-guide/v1.0.2-9.2/checkstyle.xml</configLocation>
    <encoding>UTF-8</encoding>
    <consoleOutput>true</consoleOutput>
    <failsOnError>true</failsOnError>
    <linkXRef>false</linkXRef>
    <suppressionsLocation>suppressions.xml</suppressionsLocation>
</configuration>
```

#### suppressions.xml
```xml
<?xml version="1.0"?>
 
<!DOCTYPE suppressions PUBLIC
     "-//Checkstyle//DTD SuppressionFilter Configuration 1.0//EN"
     "https://checkstyle.org/dtds/suppressions_1_0.dtd">
 
<suppressions>
  <suppress checks="ImportOrder" files="\.java"/>
  <suppress checks="javadoc" files="\.java"/>
  <suppress checks="LineLength" files="\.java"/>
</suppressions>
```

### Gradle
Include [Checstyle Plugin](https://docs.gradle.org/current/userguide/checkstyle_plugin.html) in the head of `build.gradle` file.
```
plugins {
    id 'checkstyle'
}

checkstyle {
    toolVersion '9.2.1'
    config project.resources.text.fromUri(new URI("https://raw.githubusercontent.com/pepperkit/java-style-guide/v1.0.2-9.2/checkstyle.xml"))
}
```
Be aware of current versions or any compitible versions.
It executes in the build task automatically.

## Versioning
When this style guide is changed, its version increases, it follows semver logic. However, to highlight with which
version of Checkstyle this style guide is compatible, the corresponding version of Checkstyle is stated in the version of 
the style guide.

For example, if the style guide has version `1.0.2` and it is compatible with Checkstyle version `9.2`, then the version
of Checkstyle file will be `v1.0.2-9.2`.

## Style Guide

### Project Structure

The project's structure has to conform to [Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html).
The examples are provided below.

#### Apache Maven
```text
project-name
├── pom.xml
├── .gitignore
├── LICENSE
├── CHANGELOG.md
├── README.md
└── src
    ├── docker
    │   └── Dockerfile
    ├── itest
    │   ├── java
    │   ├── kotlin
    │   └── resources
    ├── main
    │   ├── java
    │   ├── kotlin
    │   └── resources
    └── test
        ├── java
        ├── kotlin
        └── resources
```

#### Gradle
```text
project-name
├── build.gradle
├── gradle.properties
├── setting.gradle
├── .gitignore
├── LICENSE
├── CHANGELOG.md
├── README.md
└── src
    ├── docker
    │   └── Dockerfile
    ├── itest
    │   ├── java
    │   ├── kotlin
    │   └── resources
    ├── main
    │   ├── java
    │   ├── kotlin
    │   └── resources
    └── test
        ├── java
        ├── kotlin
        └── resources
```

### Source Code File

#### Encoding
Files with the source code have to be in `UTF-8` encoding.

#### Alignment
* Use spaces (not tabs), one indent - 4 spaces
* Lines should end with *LF* (Unix way), not CRLF (Windows way)
* Line's length should not exceed 120 symbols (except import statements)
* Source code file should end with an empty line

#### Source Code Files Structure
1. License
2. Package statements
3. _Empty line_
4. Import statements
5. _Empty line_
6. One high-level class

### License
The license should be put in each source code file in a header.

#### An example of full license text
```java
 /*
 * Copyright (C) 2021 PepperKit
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      https://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
 ```

#### An example of shortened license text
 ```java
/*
 * Copyright (C) 2021 PepperKit
 *
 * This software may be modified and distributed under the terms
 * of the MIT license. See the LICENSE file for details.
 */
 ```

### Import Statements
Dependencies in *import statements* should be structured in the following way:

* import `java.*`
* `<blank line>`
* import `javax.*`
* `<blank line>`
* 3rd party imports
* `<blank line>`
* import `your.own.imports`
* `<blank line>`
* other static imports

> It's not recommended to use *wildcard*s - if there are too many imports it could mean that you should split the
> class into several classes.

### Java Class Structure
The order of the `java` class elements: 

1. **static** class fields
2. **public** instance fields
3. **protected** instance fields
4. **private** instance fields
5. **constructor without parameters**
6. **other constructors**
7. **private** methods, which are called from a constructor
8. **static** fabric methods
9. **getters**, **setters**
10. interface methods implementations
11. **private** or **protected** methods, called from a method with the interface method implementation
12. other methods
13. `equals`, `hashCode`, `toString`

### Formatting
#### Curly Braces
Braces layout should follow [Kernighan and Ritcle (a.k.a. "Egyptian brackets")](https://en.wikipedia.org/wiki/Indentation_style#K.26R):
* No line breaks before the opening brace
* One space should precede opening brace
* A line break should immediately follow opening brace and closing brace, except for multi-block expressions like `if/else` and `try/catch/finally`

##### try/catch example
Correct:
```java
try {
    file = openFile("custom_file.txt");
} catch (FileNotFoundException e) {
    file = defaultFile();
}
```

Incorrect:
```java
try
{
    file = openFile("custom_file.txt");
}
catch (FileNotFoundException e)
{
    file = defaultFile();
}
```

##### if/else example
Correct:
```java
if (flag == true) {
    value = 1000;
} else {
    value = 1200);
}
```

Incorrect:
```java

if (flag == true){
    value = 1000;
}else{value = 1200);}

```

##### Class definition example
Correct:
```java
public class Cat implements Animal {

    @Override
    protected void speak() {
        this.meow();
    }
}
```

Incorrect:
```java
public class Cat implements Animal
{
    @Override
    protected void speak()
    {
        this.meow();
    }
}
```

#### Line breaks with operators and expressions
If the expression's length exceeds **120 symbols**, delimiters (commas, operators, etc.) should be moved to the next line
to improve readability. In lambda expressions `-> {` should be left on the first line.

##### if example
Correct:
```java
if (veryImportantFlagWithLongName == true && theLineIsLongerThanShouldBe == true
        && importantObject.isPresent() == false) {
        
}
```

Incorrect:
```java
if (veryImportantFlagWithLongName == true && theLineIsLongerThanShouldBe == true &&
        importantObject.isPresent() == false) {

}
```

#### Lambdas example
Correct:
```java
Optional.ofNullable(configuration.readConfigurations(configPath))
        .orElse(Lists.newArrayList())
        .stream()
        .map(config -> {
            Setting setting = new Setting(appName);
            setting.parse(config);
            return setting;
        }).collect(Collectors.toList());
```

Incorrect:
```java
Optional.ofNullable(configuration.readConfigurations(configPath))
        .orElse(Lists.newArrayList())
        .stream()
        .map(config 
        -> {
            Setting setting = new Setting(appName);
            setting.parse(config);
            return setting;
        }).collect(Collectors.toList());
```

Correct:
```java
strings.stream()
	.filter(s -> s.length() > 2)
	.sorted()
	.map(s -> s.substring(0, 2))
	.collect(Collectors.toList());
```

Incorrect:
```java
strings.stream().filter(s -> s.length() > 2).sorted()
	.map(s -> s.substring(0, 2)).collect(Collectors.toList());
```

#### Empty lines
An empty line should be added after the multi-line method declaration.

Correct:
```java
public ZooService(CatManagementService catManagementService,
                                    DogManagementService dogManagementService,
                                    ElephantManagementService elephantManagementService) {
        
    this.catManagementService = catManagementService;
    this.dogManagementService = dogManagementService;
    this.elephantManagementService = elephantManagementService;
}
```

Incorrect:
```java
public ZooService(CatManagementService catManagementService,
                                    DogManagementService dogManagementService,
                                    ElephantManagementService elephantManagementService) {
        this.catManagementService = catManagementService;
        this.dogManagementService = dogManagementService;
        this.elephantManagementService = elephantManagementService;
        }
```

#### Semantic separation
Semantically different blocks of code should be separated by an empty line.

```java
    VBox frame = new VBox(1);
    frame.getChildren().add(browser);

    Scene scene = new Scene(frame);
    stage.setScene(scene);
    stage.show();

    browser.setPrefSize(3000, 3000);
    browser.requestFocus();
```

### Naming

#### Classes
Class and object names should be a noun or its combination and have **not** to be a verb. 
For class and interface naming **UpperCamelCase** notation should be used.

#### Interfaces and Implementations
Interfaces naming with suffix `Impl` is considered an anti-pattern.

Correct:
* `Map` - interface for a data structure
* `HashMap` - implementation of that interface with **hash-tables**
* `TreeMap` - implementation with **trees**

Incorrect:
* AuthService - interface for an authentication service
* AuthService`Impl` - redundant information containing name for the implementation of that interface

Naming convention based on the example of class `Foo`:
* _Foo_ - interface
* _Abstract_**_Foo_** - abstract implementation, is supposed to be used as a basis for classes hierarchy
* _Base_**_Foo_** - concrete implementation, is supposed to be used as a basis for classes hierarchy, where this base class can be used by itself
* _Default_**_Foo_** - default implementation, is suited for most of the typical use cases
* _Simple_**_Foo_** - simple implementation, could be served as an example or a skeleton for more complex implementations
* _{Descriptive}_**_Foo_** - other kinds of implementations, the name should reflect what makes this implementation special

#### Methods
Method names should be verbs or verb collocations. 
Methods for getting and setting class fields should have prefixes `get` and `set` correspondingly. 
If a getting method returns a boolean value, it should be prefixed with `is`. 
For methods naming **lowerCamelCase** notation should be used.
Method name should fully describe what the method does, be consistent, and unambiguous.

Correct:
```java
AuthFilter createAuthenticationFilter();
```

Incorrect:
```java
AuthFilter get_filter();
```

#### Constants
Usually, constants are nouns. For naming **CONSTANT_UPPER_CASE** notation should be used - all the letters in upper case,
each word should be separated by one underscore symbol.

Correct:
```java
public static final int NUMBER_OF_ELEMENTS_ON_PAGE = 100;
```

Incorrect:
```java
public static final int numberOfElements = 100;
```

#### Instance fields, local variables
Should be in **lowerCamelCase** notation, including *final* fields.

Abbreviations in names should be avoided.
If it is impossible, for readability purposes **only the first symbol should be in uppercase**.

>For example: **_Http_**_Request_, _Product_**_Dao_**

### JavaDoc
All public methods and classes should contain JavaDoc. JavaDoc should be explanatory.

#### Formatting
It is a good practice to separate paragraphs with `<p>`. Sentences should end with the dot symbol.

One-sentenced JavaDoc is allowed if it is a class JavaDoc or there are no parameters in a method.

Correct:
```java
/** Animal implementation. */
public class Cat implements Animal { }
```

Incorrect:
```java
/**
 * Animal implementation.
 * <p/>
 */
public class Cat implements Animal { }
```

#### Tags
Any tags used in JavaDoc should not be left empty.

All tags should follow the order:
1. **@param** description of a method parameter
2. **@return** description of a return type
3. **@throws** exception class name, description of reasons, leading to this exception
4. **@see** reference to a class, method, field, constant, etc.
5. **@since** version (from which version tagged element started to be available)
6. **@deprecated** additional details, including what should be used instead

* To set links to class, field, method, or constant `{@link SomeClass}` tag can also be used.
* To show text, which should not be interpreted as HTML, the tag `{@code text}` can be used.

>Documentation on [Tag Conventions](https://www.oracle.com/technetwork/articles/java/index-137868.html).

#### Example

```java
/**
 * Creates a concrete book file reader by the specified file path. A concrete implementation is chosen based
 * on the file's extension.
 *
 * @param bookFile file referencing a book
 * @return BookFileReader instance
 * @throws IllegalArgumentException      if the specified file does not exist
 * @throws UnsupportedOperationException if an extension of the file is not supported
 * @see TxtFileReader
 */
public static BookFileReader createInstance(File bookFile) {
}
```

#### Documenting deprecated API
Deprecated API should be marked with the annotation `@Deprecated` and with the tag `@deprecated` in JavaDoc.
It is highly recommended to use the tag `@deprecated` with the commentaries, explaining how to use the new API,
so that developers had an understandable way to migrate from the old to the new API.

##### Example from JDK
```java
/**
 * Makes the {@code Dialog} visible. If the dialog and/or its owner are not yet displayable, both are made displayable.
 * @see Component#hide
 * @deprecated As of JDK version 1.5, replaced by {@link #setVisible(boolean) setVisible(boolean)}.
 */
@Deprecated
public void show() {
```

##### Javadoc for a new member
It should be stated from which version of a library the class, method, field, or constant appeared, with the tag use of tag `@since`.

### Tests
Code style conventions apply to test classes as well with some particularities described below.

#### Naming Test Classes

##### Unit Tests
Unit test class names should be ended with postfix `Test`.

Example of unit test on class `Cat.java`:
```
CatTest.java
```

#### Integration Tests
Integration test class name should end with postfix `IT` or `IntegrationTest`.

Example of integration test on class `Cat.java`:
```
CatIT.java
CatIntegrationTest.java
```

#### Recommendations on Naming Test Methods

The test method's name should reflect the essence of the test.
It is recommended to avoid prefixes or postfixes `test` in method names since the test class name already explains that.

Incorrect (it is unclear what this method tests):
```java
public void test1() {}
```

Better (it is deductible what is tested, but it is still unclear what particular behavior is tested):
```java
public void testPostNullEntityError() {}
```

Correct (test method's name is self-explanatory):
```java
public void shouldThrowExceptionWhenEntityIsNull() {}
```

## Resources
1. [Sun Code Conventions](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/sun_checks.xml)
2. [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
3. [Spring Code Style](https://github.com/spring-projects/spring-framework/wiki/Code-Style)
4. "Clean Code", Robert Martin
5. "Effective Java", Joshua Bloch
6. [How to write JavaDoc](https://www.oracle.com/technetwork/java/javase/index-137868.html)
7. [Impl Classes are evil](https://octoperf.com/blog/2016/10/27/impl-classes-are-evil/)
8. [Implementations of interfaces](https://blog.joda.org/2011/08/implementations-of-interfaces-prefixes.html)
9. [How and when to deprecate APIs](https://docs.oracle.com/javase/7/docs/technotes/guides/javadoc/deprecation/deprecation.html)
