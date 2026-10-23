# Java Upgrade Notes (Java 8 -> Java 25)

## Target
- Java 25 (latest LTS). `maven.compiler.source/target/release` = 25.
- `maven-compiler-plugin` pinned to 3.13.0 in `<pluginManagement>` with `<release>${maven.compiler.release}</release>`.
- `.idea/misc.xml` language level and project JDK raised to 25.

## Build requirement
The build requires a JDK 25 toolchain. The default JDK in the current environment is
Corretto 17, so builds must point `JAVA_HOME` at JDK 25:

```
JAVA_HOME=/usr/lib/jvm/java-25 mvn clean install
```

Verified: BUILD SUCCESS; emitted class files are major version 69 (Java 25).

## Deviations
- **maven-compiler-plugin 3.13.0 on Maven 3.8.4.** The transformation guidance pins
  3.13.0 only when Maven is 3.9.0+, otherwise 3.0.5. 3.0.5 predates the `<release>`
  configuration element and cannot emit Java 25 bytecode, so it was not used.
  Plugin 3.13.0 requires only Maven 3.6.3+, so it runs correctly on the installed
  Maven 3.8.4. Deviation accepted; no functional impact.

## Skipped / inapplicable updates
- `pom.xml` declares **no third-party dependencies**, so the Jakarta EE, database/ORM,
  core utility library, and Spring/Spring Boot update groups have nothing to update.
- No `javax.*` imports exist in the single source file, so no Jakarta namespace migration
  was needed.
- No test sources exist, so there is no test suite to re-run or compare against baseline.
- No Gradle wrapper, Dockerfile, or CI configuration pinning a JDK version is present.
