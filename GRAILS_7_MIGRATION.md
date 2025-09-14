# Grails 7 Migration Guide for Jasypt Encryption Plugin

This document outlines the changes made to upgrade the Grails Jasypt Encryption plugin to support Grails 7.0.0-SNAPSHOT and the manual steps required when upgrading existing applications.

## Changes Made

### Core Plugin Upgrades

1. **Grails Version**: Updated from 4.0.0 to 7.0.0-SNAPSHOT
2. **Gradle**: Updated from 5.6.4 to 8.5
3. **GORM Version**: Updated from 7.0.0 to 8.0.0
4. **Java Compatibility**: Updated from Java 7 to Java 17

### Dependency Updates

- **Spring Boot dependencies**: Updated to use `implementation` instead of `provided` for better Gradle 8+ compatibility
- **Asset Pipeline**: Updated from 3.0.10 to 4.4.0 
- **Dependency Management**: Updated from 0.5.2.RELEASE to 1.1.4
- **Nexus Publish Plugin**: Updated from 1.0.0 to 1.3.0
- **BouncyCastle**: Updated from bcprov-jdk16:1.46 to bcprov-jdk18on:1.77 for Java 17+ support
- **Servlet API**: Updated from 3.1.0 to 4.0.1

### Configuration Changes

- Fixed missing `javadocJar` task configuration
- Updated repository URLs to include Gradle Plugin Portal
- Modernized dependency configurations (`compile` → `implementation`, `runtime` → `runtimeOnly`, etc.)

## Manual Migration Steps for Applications

### 1. Update Gradle Wrapper

```bash
./gradlew wrapper --gradle-version=8.5
```

### 2. Update gradle.properties

```properties
grailsVersion=7.0.0-SNAPSHOT
gormVersion=8.0.0
```

### 3. Update build.gradle

#### Update Buildscript Dependencies
```groovy
buildscript {
    dependencies {
        classpath "org.grails:grails-gradle-plugin:$grailsVersion"
        classpath "org.grails.plugins:hibernate5:${gormVersion}"
        classpath "com.bertramlabs.plugins:asset-pipeline-gradle:4.4.0"
    }
}
```

#### Update Plugin Versions
```groovy
plugins {
    id "io.spring.dependency-management" version "1.1.4"
}
```

#### Update Java Compatibility
```groovy
sourceCompatibility = 17
targetCompatibility = 17
```

#### Update Dependencies
Replace deprecated dependency configurations:
- `compile` → `implementation`
- `runtime` → `runtimeOnly`
- `testCompile` → `testImplementation`
- `testRuntime` → `testRuntimeOnly`

### 4. Update Plugin Version

In your application's `build.gradle`, update the Jasypt plugin dependency:

```groovy
dependencies {
    implementation "org.grails.plugins:jasypt-encryption:4.0.4"
}
```

### 5. Java Version Requirements

Ensure your application and build environment use Java 17 or higher:
- Update `JAVA_HOME` environment variable
- Update CI/CD pipeline Java versions
- Update IDE project settings

### 6. Test Configuration Updates

If you have integration tests, update test configurations to use:
- `@Integration` and `@Rollback` annotations (already available in Grails 4+)
- Update any Spock test dependencies if needed

## Breaking Changes

### Removed Dependencies
- Removed `javax.servlet:javax.servlet-api:3.1.0` (replaced with 4.0.1)
- Older BouncyCastle library (replaced with Java 17+ compatible version)

### Configuration Changes
- Maven plugin replaced with `maven-publish` in sample projects
- Updated asset pipeline configuration

## Known Issues

### Groovy Version Conflicts
If you encounter Groovy version conflicts in your application (mixing old and new Groovy versions), ensure all your dependencies are compatible with Grails 7 and Groovy 4.x.

### Sample Project Dependencies
The included sample test project may have dependency resolution issues due to mixing different Groovy versions. This is a known issue when transitioning from Grails 4 to 7.

## Verification Steps

1. Clean and rebuild your application:
   ```bash
   ./gradlew clean build
   ```

2. Run your tests:
   ```bash
   ./gradlew test
   ```

3. Start your application:
   ```bash
   ./gradlew bootRun
   ```

4. Verify encryption/decryption functionality works as expected

## Support

If you encounter issues during migration:
1. Check that all dependencies are compatible with Grails 7
2. Ensure Java 17+ is being used consistently
3. Review the Grails 7 migration guide for additional breaking changes
4. Check plugin compatibility with your other Grails plugins

## Additional Resources

- [Grails 7 Release Notes](https://grails.github.io/grails-doc/latest/)
- [Gradle 8.5 Release Notes](https://docs.gradle.org/8.5/release-notes.html)
- [Spring Boot 3.x Migration Guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide)