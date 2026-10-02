# users-app Architecture Baseline

**Document type:** Current-state inventory; this is not a target architecture or a set of engineering standards.

**As of:** 2026-10-02

This document records only repository evidence found during inspection. Requirements in [constitution.md](constitution.md) are called out separately and are not treated as implemented capabilities.

## Application Overview

`users-app` is currently a minimal Spring Boot application. Its application class starts Spring Boot, and its only test verifies that the application context loads. No user-management behavior or application-defined HTTP endpoint was found in the source tree.

## Technology Stack and Versions

| Area | Current evidence |
|---|---|
| Language | Java; `java.version` is `21` in [pom.xml](../../pom.xml). |
| Spring Boot | Parent POM version `4.1.0` in [pom.xml](../../pom.xml). |
| Web framework | `spring-boot-starter-webmvc` is the only declared runtime dependency in [pom.xml](../../pom.xml). |
| Build | Maven project with [pom.xml](../../pom.xml), `mvnw`, and `mvnw.cmd`. The wrapper cannot currently run because `.mvn/wrapper/maven-wrapper.properties` is absent. |
| Maven version | The project does not pin a Maven distribution in the available wrapper configuration. Maven `3.9.9` was installed in the inspection environment; this is not a repository-pinned version. |
| Java runtime observed | The test run used JDK `23.0.2`; Maven compiled the project with Java release `21`. The repository does not pin the local runtime JDK. |

## Project and Module Structure

The repository contains one root Maven project; no Maven modules are declared in the POM. Relevant source paths are:

```text
src/
├── main/
│   ├── java/com/appsdeveloperblog/users_app/UsersAppApplication.java
│   └── resources/application.properties
└── test/
    └── java/com/appsdeveloperblog/users_app/UsersAppApplicationTests.java
```

The package path is `com.appsdeveloperblog.users_app`. [HELP.md](../../HELP.md) explains that the underscore is used because the original hyphenated package name is invalid as a Java package identifier.

## Package Architecture and Application Components

The only production Java class is [UsersAppApplication.java](../../src/main/java/com/appsdeveloperblog/users_app/UsersAppApplication.java). It is annotated with `@SpringBootApplication` and delegates startup to `SpringApplication.run`.

| Component | Observed state |
|---|---|
| Controllers | No application controller classes or request-mapping annotations found. No application-defined API endpoints are present in the inspected source. |
| Services | No service classes or service package found. |
| Repositories | No repository classes or repository package found. |
| Entities | No JPA or other persistence entity classes found. |
| DTOs | No DTO classes or Java records found. |
| Dependency injection | No application component injection found. |
| Architectural pattern | A single Spring Boot application entry point and Maven module are present. A controller/service/repository layered implementation is not yet present. |

## Database, Migrations, and API

No database or persistence dependency, datasource configuration, migration directory, or migration tool declaration was found in [pom.xml](../../pom.xml) or `src/main`. No application-defined HTTP endpoint mappings were found. The presence of Spring MVC on its own does not establish that the application currently exposes a domain API.

## Security and Authentication

No Spring Security dependency, security configuration, authentication code, or authorization rules were found in the current source or [pom.xml](../../pom.xml). Authentication behavior is therefore not evidenced by the implementation.

## Configuration and Profiles

[application.properties](../../src/main/resources/application.properties) contains only `spring.application.name=users-app`. No profile-specific properties files, active-profile setting, datasource properties, or externalized environment configuration were found under `src/main/resources`.

## External Integrations and Dependencies

The project directly declares these dependencies in [pom.xml](../../pom.xml):

| Dependency | Scope / observed purpose |
|---|---|
| `spring-boot-starter-webmvc` | Runtime Spring MVC web support. |
| `spring-boot-starter-webmvc-test` | Test-scoped Spring MVC testing support. |

The Spring Boot parent manages dependency versions. The build also declares `spring-boot-maven-plugin`. No external service client, messaging, database, or observability dependency is directly declared. This is a statement about direct project declarations, not the complete transitive dependency graph.

## Testing Structure

The only test source found is [UsersAppApplicationTests.java](../../src/test/java/com/appsdeveloperblog/users_app/UsersAppApplicationTests.java). It uses JUnit Jupiter `@Test` and `@SpringBootTest` and contains one `contextLoads` test. There are no separate unit, integration, or API test directories in the current source tree.

System `mvn test` completed successfully during validation with one test passing. The documented wrapper invocation `./mvnw test` failed because `.mvn/wrapper/maven-wrapper.properties` is missing.

## Build, CI/CD, and Deployment

The Maven build is defined by [pom.xml](../../pom.xml) and includes the Spring Boot Maven plugin. The repository has Maven wrapper scripts, but lacks their wrapper configuration, so they are not currently usable as checked in.

No GitHub Actions workflow was found under `.github/workflows`. No Dockerfile or Terraform files were found. No deployment manifests or infrastructure definitions were identified in the repository inventory. [HELP.md](../../HELP.md) links to the Spring Boot Maven plugin's OCI image guide; that documentation link is not evidence that image building or deployment is configured or used here.

## Observability and Logging

No application-specific logging statements, logging configuration, metrics, tracing, or health-check implementation were found in the application source, resources, or direct dependency declarations. Spring Boot emitted framework startup logs during the test run; no further observability setup is evidenced.

## Architectural Patterns Actually Observed

- Single-module Maven application.
- Spring Boot auto-configuration via `@SpringBootApplication`.
- Spring MVC starter dependency.
- JUnit 5 / Spring Boot context-load test.

No evidence currently supports describing the application as a layered web application, an authenticated application, a database-backed system, or an API with domain endpoints.

## Differences from `constitution.md`

The constitution records intended standards. The following items are not currently evidenced in the implementation:

| Constitution topic | Current-state difference |
|---|---|
| Thymeleaf views | No Thymeleaf dependency or application templates are present. |
| Spring Data JPA and persistence | No JPA dependency, entities, repositories, or datasource configuration are present. |
| MySQL and H2 | No database drivers or database configuration are declared. |
| Flyway | No Flyway dependency or migration files are present. |
| Spring Security and form login | No security dependency, configuration, or authentication behavior is present. |
| Controller/service/repository layers | No such application classes or packages are present. |
| DTO records and Jakarta Bean Validation | No DTO records, validation dependency use, or validation annotations are present. |
| Global `@ControllerAdvice` | No global exception handler is present. |
| `dev` and `prod` profiles | No profile-specific resource files or profile configuration are present. |
| SLF4J application logging | No application logging statements or logger configuration are present. |
| JUnit 5 service tests | JUnit 5 is used for one context-load test; no business services or service tests exist yet. |

These differences describe implementation status only; they do not amend or reinterpret the constitution.

## Technical Debt and Risks Evidenced

- **Maven Wrapper is incomplete:** `./mvnw test` fails because `.mvn/wrapper/maven-wrapper.properties` is absent. A system Maven test passes, but the repository does not provide a functioning wrapper configuration.
- **Minimal test coverage:** The only test checks application context startup; no domain behavior or endpoint behavior is covered because none is present yet.
- **Constitution-to-code gap:** Future work that assumes persistence, authentication, server-rendered views, or profile configuration will need to introduce those capabilities; they are not available in the current implementation.
- **No repository CI workflow found:** Automated build/test enforcement is not evidenced by a workflow under `.github/workflows`.
- **Deployment setup not evidenced:** No Dockerfile, Terraform, or deployment manifest was found, so deployment topology and infrastructure cannot be established from this repository.

## Evidence Index

- [Maven project, Java/Spring Boot versions, direct dependencies, and build plugin](../../pom.xml)
- [Spring Boot application entry point](../../src/main/java/com/appsdeveloperblog/users_app/UsersAppApplication.java)
- [Application properties](../../src/main/resources/application.properties)
- [JUnit 5 context-load test](../../src/test/java/com/appsdeveloperblog/users_app/UsersAppApplicationTests.java)
- [Package-name explanation and generated project documentation](../../HELP.md)
- [Intended engineering standards (not implementation evidence)](constitution.md)
