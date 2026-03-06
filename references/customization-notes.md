# Project Override Notes

Use this file when the user supplies project-specific engineering materials that should override or refine the baseline rules.

## Current Team Defaults

- JDK 21
- Spring Boot 3.5.x
- Backend business services
- Lombok preferred
- Slf4j logging required
- Constructor injection only
- Internal base component reuse is a high-priority rule

## What To Capture From User Materials

When the user provides shared code or documentation, extract and record:

- base response wrapper conventions
- exception hierarchy and business exception rules
- paging request and response models
- common DTO containers such as `BaseDTO`
- range models
- key-value or id-name carrier objects
- enum interfaces or serialization conventions
- SPI, constants, and tuple abstractions
- toolkit utilities such as `Op`, `St`, `PageUtil`, `EnumUtil`, `TreeUtil`, `JacksonUtil`, `HttpUtil`, `ExcelExportUtil`, `ExcelReadUtil`, and `ThrowableUtil`
- web conventions such as `Result`, global exception handling, Jackson binding, startup entrypoints, and XSS behavior
- validation annotations and centralized validation exception responses
- framework wrappers for async context propagation, thread factories, and servlet request caching
- business exception and business property conventions

## How To Apply Overrides

- Prefer explicit team conventions over generic Java style preferences.
- If the project has an established internal abstraction, treat it as the default recommendation.
- If a local module deviates intentionally, preserve that deviation only when it is documented or clearly required.
- Record exceptions narrowly. Do not weaken the whole skill because one module has unusual legacy constraints.

## Typical Project-Specific Decisions To Add Here

- whether `record` is allowed for request or response models
- whether `@Builder` is expected or restricted for entities
- standard controller response envelope
- business exception base class and error code style
- required logging fields such as tenant, trace ID, user ID, or biz ID
- test stack such as JUnit 5, Mockito, AssertJ, Spring test slices
- Javadoc language or coverage expectations
