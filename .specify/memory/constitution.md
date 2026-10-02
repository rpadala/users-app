# users-app Constitution

This constitution defines the permanent engineering standards for `users-app`. It applies to all features and generated work. Feature-specific requirements and business rules belong in feature specifications, not in this constitution.

## Core Principles

### I. Consistent Technology Baseline

- Use Java 21, Spring Boot 4, and Maven.
- Use Thymeleaf for server-rendered views, Spring Data JPA for persistence, and Spring Security with form-based authentication.
- Use MySQL in production and H2 in development.
- Use the established stack. Introduce or replace a technology only when a clear need justifies it.

### II. Clear Layered Architecture

- Organize application code by layer and preserve separation between controllers, services, and repositories.
- Keep business logic in the service layer, HTTP request and response handling in controllers, and data access in repositories.
- Use constructor injection exclusively; field injection is prohibited.
- Favor composition, focused responsibilities, and reuse of existing components. Avoid duplicate code and unnecessary abstractions.

### III. Safe Presentation and Input Boundaries

- Represent data transfer objects with Java records.
- Never expose JPA entities directly to the presentation layer.
- Validate user input consistently using Jakarta Bean Validation.

### IV. Reliable and Efficient Persistence

- Use Spring Data JPA for data access and Flyway for all database schema changes.
- Default to lazy loading, prevent N+1 query problems, and paginate large result sets.
- Add caching only when measurable benefit justifies its maintenance cost.

### V. Security and Safe Error Handling

- Follow secure defaults and protect sensitive information in responses, logs, and configuration.
- Handle application-wide web errors through a global `@ControllerAdvice`.
- Present user-friendly errors without revealing internal exceptions or implementation details.

## Additional Constraints

### Configuration

- Use `application.properties` and maintain separate `dev` and `prod` profiles.
- Keep environment-specific configuration outside application code whenever practical.

### Logging

- Use SLF4J for application logging.
- Do not use `System.out.println()` or `System.err.println()`.
- Never log passwords, secrets, or other sensitive information.

### Testing and Coding Standards

- Write unit tests with JUnit 5. Every new business service must have appropriate automated tests.
- Keep tests readable, isolated, and maintainable.
- Do not use wildcard imports or Lombok.
- Write self-explanatory code; add comments only when they clarify intent that is not clear from the code.

## Development Workflow

- Evaluate changes for consistency, maintainability, readability, testability, security, and long-term scalability.
- Reuse existing components and avoid unnecessary libraries; any new dependency must have a clear, justified need.
- Ensure changes and generated features comply with this constitution.

## Governance

- This constitution applies to all project work and takes precedence over conflicting feature-level guidance.
- Any deviation requires a documented rationale and approval as part of the relevant change.
- Amendments must be intentional, reviewed, and reflected in the version and amendment date. Update related project guidance when an amendment changes required practices.

**Version:** 1.0.0 | **Ratified:** 2026-10-01 | **Last Amended:** 2026-10-01
