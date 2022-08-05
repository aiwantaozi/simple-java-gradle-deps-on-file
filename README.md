# simple-java-gradle-deps-on-file

Follow [Building Java Libraries Sample with Gradle](https://docs.gradle.org/current/samples/sample_building_java_libraries.html) guide to create this project.

```bash

$ gradle init

Welcome to Gradle 7.4.2!

Here are the highlights of this release:
 - Aggregated test and JaCoCo reports
 - Marking additional test source directories as tests in IntelliJ
 - Support for Adoptium JDKs in Java toolchains

For more details see https://docs.gradle.org/7.4.2/release-notes.html

Starting a Gradle Daemon (subsequent builds will be faster)

Select type of project to generate:
  1: basic
  2: application
  3: library
  4: Gradle plugin
Enter selection (default: basic) [1..4] 2

Select implementation language:
  1: C++
  2: Groovy
  3: Java
  4: Kotlin
  5: Scala
  6: Swift
Enter selection (default: Java) [1..6] 3

Select build script DSL:
  1: Groovy
  2: Kotlin
Enter selection (default: Groovy) [1..2] 1

Generate build using new APIs and behavior (some features may change in the next minor release)? (default: no) [yes, no] yes
Select test framework:
  1: JUnit 4
  2: TestNG
  3: Spock
  4: JUnit Jupiter
Enter selection (default: JUnit Jupiter) [1..4] 1

Project name (default: simple-java-gradle-deps-on-file):
Source package (default: simple.java.gradle.deps.on.file):

> Task :init
Get more help with your project: https://docs.gradle.org/7.4.2/samples/sample_building_java_libraries.html

BUILD SUCCESSFUL in 42s
2 actionable tasks: 2 executed
The operation couldn’t be completed. Unable to locate a Java Runtime.
Please visit http://www.java.com for information on installing Java.

```

## File dependency

Step 1. place joda-time-2.10.14.jar under app/libs

Step 2. add joda-time to dependency

**Option 1**: configure with dependency fileTree, check file app/build.gradle

./app/build.gradle

```groovy

dependencies {

    implementation fileTree(include: ['*.jar', '*.aar'], dir: 'libs')

    implementation 'com.google.guava:guava:30.1.1-jre'
}

```

dependency tree

```
+--- com.google.guava:guava:30.1.1-jre
|    +--- com.google.guava:failureaccess:1.0.1
|    +--- com.google.guava:listenablefuture:9999.0-empty-to-avoid-conflict-with-guava
|    +--- com.google.code.findbugs:jsr305:3.0.2
|    +--- org.checkerframework:checker-qual:3.8.0
|    +--- com.google.errorprone:error_prone_annotations:2.5.1
|    \--- com.google.j2objc:j2objc-annotations:1.3
\--- junit:junit:4.13.2
     \--- org.hamcrest:hamcrest-core:1.3

```

**Option 2**: repository

./app/build.gradle

```groovy

repositories {
   flatDir {
       dirs 'libs'
   }
}


dependencies {
   implementation name: 'joda-time-2.10.14'
}

```

dependency tree

```
+--- com.google.guava:guava:30.1.1-jre
|    +--- com.google.guava:failureaccess:1.0.1
|    +--- com.google.guava:listenablefuture:9999.0-empty-to-avoid-conflict-with-guava
|    +--- com.google.code.findbugs:jsr305:3.0.2
|    +--- org.checkerframework:checker-qual:3.8.0
|    +--- com.google.errorprone:error_prone_annotations:2.5.1
|    \--- com.google.j2objc:j2objc-annotations:1.3
+--- :joda-time-2.10.14
\--- junit:junit:4.13.2
     \--- org.hamcrest:hamcrest-core:1.3
```
