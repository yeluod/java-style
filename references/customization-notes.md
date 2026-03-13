# Project Override Notes

Use this file when the user supplies project-specific engineering materials that should override or refine the baseline rules.

## Current Team Defaults

- JDK 21
- Spring Boot 3.5.x
- Backend business services
- Lombok preferred
- Slf4j logging required
- Constructor injection only
- Maven commands should prefer `~/.m2/settings.xml` for private repository access

## What To Capture From User Materials

When the user provides shared code or documentation, extract and record:

- base response wrapper conventions
- exception hierarchy and domain exception rules
- paging request and response models
- common DTO container conventions
- range models
- key-value or id-name carrier objects
- enum interfaces or serialization conventions
- shared constants and other common abstractions
- existing utility classes that the team expects to reuse
- web conventions such as global exception handling, Jackson binding, startup entrypoints, and XSS behavior
- validation annotations and centralized validation exception responses
- async context propagation, thread factory, and servlet request caching conventions
- business property conventions

## How To Apply Overrides

- Prefer explicit team conventions over generic Java style preferences.
- If the project has an established local abstraction, treat it as the default recommendation.
- If a local module deviates intentionally, preserve that deviation only when it is documented or clearly required.
- Record exceptions narrowly. Do not weaken the whole skill because one module has unusual legacy constraints.
- If `~/.m2/settings.xml` is missing and Maven execution requires private repositories, ask the user for the correct settings file path.

## Typical Project-Specific Decisions To Add Here

- whether `record` is allowed for request or response models
- whether `@Builder` is expected or restricted for entities
- standard controller response envelope
- domain exception base class and error code style
- required logging fields such as tenant, trace ID, user ID, or biz ID
- test stack such as JUnit 5, Mockito, AssertJ, Spring test slices
- Javadoc language or coverage expectations
