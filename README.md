# Kotlin + Gradle DevOps Lab

A beginner-friendly, hands-on Kotlin application project used to learn **Gradle Kotlin DSL, build automation, dependency management, testing, dependency locking, and Git/GitHub workflows**.

This repository documents the practical steps completed so far on Windows using PowerShell.

## 1. Project details

| Component | Version / Configuration |
|---|---|
| Language | Kotlin |
| Build tool | Gradle Wrapper 9.8.2 |
| Build scripts | Kotlin DSL (`.gradle.kts`) |
| Kotlin JVM plugin | 2.4.10 |
| Java toolchain | 21 |
| Unit testing | JUnit Jupiter 6.0.1 |
| Example dependency | Guava 33.5.0-jre |
| Dependency repository | Maven Central |

> **Java note:** The local machine used during setup has Eclipse Temurin JDK 25 installed. The project itself requests a Java 21 toolchain in `app/build.gradle.kts`. Gradle may use an installed compatible JDK or download the required toolchain through the configured toolchain resolver.

## 2. Prerequisites

Install the following:

1. **Git for Windows** — [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. **A JDK** — preferably JDK 21 for this project. [Eclipse Temurin](https://adoptium.net/)
3. **GitHub account** — [https://github.com/](https://github.com/)

Check Java and Git from PowerShell:

```powershell
java -version
git --version
```

You do not need to install Gradle globally to build this repository. The included Gradle Wrapper downloads and runs the configured Gradle version.

## 3. Clone the repository

Open PowerShell and run:

```powershell
cd C:\GradleProjects
git clone https://github.com/Orrm23/kotlin-gradle-devops-lab.git
cd kotlin-gradle-devops-lab
```

If you already have the project locally, open its folder instead:

```powershell
cd C:\GradleProjects\demo-app
```

The local development folder used during the initial setup was `C:\GradleProjects\demo-app`. The GitHub repository is named `kotlin-gradle-devops-lab`.

## 4. Understand the project structure

The important files and folders are:

```text
kotlin-gradle-devops-lab/
├── app/
│   ├── src/
│   │   ├── main/kotlin/org/example/App.kt
│   │   └── test/kotlin/org/example/AppTest.kt
│   ├── build.gradle.kts
│   └── gradle.lockfile
├── gradle/
│   ├── libs.versions.toml
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle.kts
├── settings-gradle.lockfile
├── gradle.properties
├── .gitignore
└── README.md
```

| File | Purpose |
|---|---|
| `settings.gradle.kts` | Defines the root project and includes the `app` module |
| `app/build.gradle.kts` | Configures Kotlin, dependencies, application entry point, tests and custom tasks |
| `gradle/libs.versions.toml` | Central place for dependency and plugin versions |
| `gradle/wrapper/gradle-wrapper.properties` | Selects the Gradle Wrapper distribution version |
| `gradle.lockfile` files | Record resolved dependency versions for reproducible dependency resolution |
| `.gitignore` | Prevents generated build files and IDE files from being committed |

## 5. Run the application

Use the Gradle Wrapper from the project root.

On Windows PowerShell:

```powershell
.\gradlew.bat :app:run
```

Expected application output:

```text
Hello World!
```

You can also run:

```powershell
.\gradlew.bat run
```

## 6. Run unit tests

Execute the test task:

```powershell
.\gradlew.bat :app:test
```

To run tests and then build the project:

```powershell
.\gradlew.bat test
.\gradlew.bat build
```

The project uses JUnit Platform with JUnit Jupiter. Test source code is under `app/src/test/kotlin/`.

## 7. Build and clean the project

Build the application:

```powershell
.\gradlew.bat build
```

Remove generated build output:

```powershell
.\gradlew.bat clean
```

Clean and build again:

```powershell
.\gradlew.bat clean build
```

A successful build displays:

```text
BUILD SUCCESSFUL
```

The `build` task is configured to depend on the custom `validateBuild` task, so the validation messages run as part of the build.

## 8. Run custom Gradle tasks

Two custom tasks have been added in `app/build.gradle.kts`.

### Run helloDevOps

```powershell
.\gradlew.bat :app:helloDevOps
```

This task prints a few messages to demonstrate a custom Gradle task.

### Run validateBuild

```powershell
.\gradlew.bat :app:validateBuild
```

This is a simple learning/demo task. It prints validation messages; it is not a full code-quality or security validation.

To list available tasks:

```powershell
.\gradlew.bat tasks
```

## 9. Understand dependencies

Dependencies are declared in `gradle/libs.versions.toml` and referenced from `app/build.gradle.kts`.

Current examples:

- Kotlin JVM plugin: `org.jetbrains.kotlin.jvm:2.4.10`
- Guava: `com.google.guava:guava:33.5.0-jre`
- JUnit Jupiter: `org.junit.jupiter:junit-jupiter:6.0.1`

The application dependency is declared using the version catalog:

```kotlin
dependencies {
    implementation(libs.guava)
    testImplementation(libs.junit.jupiter)
    testRuntimeOnly("org.junit.platform:junit-platform-launcher")
}
```

### View runtime dependencies

```powershell
.\gradlew.bat :app:dependencies --configuration runtimeClasspath
```

### Find why a dependency is included

```powershell
.\gradlew.bat :app:dependencyInsight --dependency guava --configuration runtimeClasspath
```

A **transitive dependency** is a dependency brought in by another dependency. For example, Guava brings additional libraries into the dependency graph.

## 10. Dependency locking

Dependency locking helps keep resolved dependency versions consistent between builds.

The following configuration is enabled in `app/build.gradle.kts`:

```kotlin
dependencyLocking {
    lockAllConfigurations()
}
```

Generate or update lock state deliberately:

```powershell
.\gradlew.bat :app:build --write-locks
```

This creates or updates lockfiles, including:

- `app/gradle.lockfile`
- `settings-gradle.lockfile`

**Important:** Commit the lockfiles to Git. Do not run `--write-locks` on every normal build; use it when you intentionally want to update dependency versions.

## 11. Git workflow

The project is tracked with Git. These are the basic commands used during setup.

Check the repository and working tree:

```powershell
git rev-parse --show-toplevel
git status
```

Review recent commits:

```powershell
git log --oneline -5
```

Stage and commit changes:

```powershell
git add .
git commit -m "Describe your changes"
```

Push the main branch to GitHub:

```powershell
git push -u origin main
```

For future pushes, after committing:

```powershell
git push
```

Check the configured remote:

```powershell
git remote -v
```

Expected remote:

```text
https://github.com/Orrm23/kotlin-gradle-devops-lab.git
```

## 12. Complete quick-start sequence

After cloning the repository, these commands run the main checks in order:

```powershell
cd C:\GradleProjects\kotlin-gradle-devops-lab

# Run the application
.\gradlew.bat :app:run

# Run unit tests
.\gradlew.bat :app:test

# Inspect dependencies
.\gradlew.bat :app:dependencies --configuration runtimeClasspath

# Build the application
.\gradlew.bat clean build

# Check Git status
git status
```

## 13. Current learning progress

Completed so far:

- [x] Set up a Kotlin application using Gradle.
- [x] Use Gradle Kotlin DSL in `build.gradle.kts`.
- [x] Run the application with the Gradle Wrapper.
- [x] Execute unit tests and build tasks.
- [x] Add custom Gradle tasks: `helloDevOps` and `validateBuild`.
- [x] Configure a Java 21 toolchain.
- [x] Manage plugin and library versions through a version catalog.
- [x] Inspect dependency trees and dependency insight.
- [x] Enable dependency locking and generate lockfiles.
- [x] Initialize Git, commit the project and push it to GitHub.

## 14. Next learning steps

Planned topics (not yet implemented in this repository):

1. Add meaningful Kotlin application logic and unit tests.
2. Learn Gradle task inputs, outputs and task dependencies.
3. Configure build caching and improve build performance.
4. Add code formatting and static analysis.
5. Create a Jenkins CI pipeline for build and test automation.
6. Package the application and build a Docker image.

---

**Repository:** [Orrm23/kotlin-gradle-devops-lab](https://github.com/Orrm23/kotlin-gradle-devops-lab)

This README describes the project as it exists today. The next sections and tools will be documented as they are implemented.
