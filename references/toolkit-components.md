# Toolkit Component Reuse

This file summarizes reusable capabilities extracted from `backend-components-toolkit`.

Use it whenever the task touches utility design, helper methods, JSON handling, HTTP invocation, Excel import/export, tree assembly, enum conversion, page interval calculation, stream helpers, or null-safe functional chaining.

## Positioning

`backend-components-toolkit` is a shared utility layer.

It is not the place for business DTOs, result envelopes, or paging request models. It provides reusable low-level helpers that business modules should call instead of re-implementing the same mechanics locally.

## Reuse Rules

Before writing a new utility class or helper method, check whether toolkit already covers the capability.

Treat these as reuse-first capabilities:
`
- null-safe value handling and try-catch chaining
- stream construction and convenience collection traversal
- page offset and interval arithmetic
- enum lookup and enum description conversion
- tree building
- JSON serialization and deserialization
- HTTP client invocation
- Excel import and template export
- dictionary assembly
- exception wrapping and stack extraction`

Do not create a project-local helper if toolkit already provides a stable version of the same behavior.

## Key Toolkit Capabilities

### 1. `Op`

Source capability: `com.ygzy.components.toolkit.util.lang.Op`

Use `Op` when you need:

- null-safe chaining
- empty checks beyond `null`, including blank strings, empty collections, empty maps, and empty arrays
- `map`, `flatMap`, `filter`, `peek`, `orElse`, `orElseGet`
- `ofTry(...)` style capture of exception-producing suppliers

Guidance:

- Prefer `Op` over ad hoc multi-layer null checks when it makes the code shorter and still readable.
- Do not force `Op` into simple `if (x == null)` scenarios where plain code is clearer.
- In review, flag duplicated local optional-wrapper utilities when `Op` already solves the problem.

### 2. `St`

Source capability: `com.ygzy.components.toolkit.util.stream.St`

Use `St` when you need:

- null-safe stream creation from arrays, iterables, iterators, files, or paths
- stream join helpers
- finite iteration helpers
- hierarchy traversal helpers

Guidance:

- Prefer `St` when the project already relies on it for null-safe stream entry.
- Do not wrap a single simple list traversal in `St` just for style.
- In review, flag custom `streamOfNullable`, iterable-to-stream, or hierarchy traversal helpers that duplicate `St`.

### 3. `PageUtil`

Source capability: `com.ygzy.components.toolkit.util.page.PageUtil`

Use `PageUtil` for:

- offset calculation
- inclusive start-stop range calculation
- total-aware page interval truncation
- end-exclusive calculation for list slices or APIs that need it

Guidance:

- Use `PageReq` and `PageRes` for business paging contracts.
- Use `PageUtil` only for page arithmetic or lower-level interval computation.
- Do not manually recalculate `(pageNo - 1) * pageSize` and range boundaries in scattered business code.

## 4. `EnumUtil`

Source capability: `com.ygzy.components.toolkit.util.enums.EnumUtil`

Use `EnumUtil` when the enum follows the shared `BaseEnum` contract and you need:

- name-to-enum lookup
- description-to-enum lookup
- default enum fallback
- enum value lists
- enum description lists
- paired value/description export

Guidance:

- Prefer the shared enum protocol over custom enum parsing logic.
- Do not write repetitive `Arrays.stream(...).filter(...).findFirst()` blocks if `EnumUtil` already matches the need.

## 5. `TreeUtil`

Source capability: `com.ygzy.components.toolkit.util.tree.TreeUtil`

Use `TreeUtil` when building hierarchical structures that follow the shared `Tree` model.

It already provides:

- default tree config
- safe config duplication
- config validation
- tree assembly with optional sorting
- parent normalization for missing parent nodes

Guidance:

- Do not hand-roll tree assembly in service code if the node type already matches the shared tree contract.
- Keep tree-building concerns in reusable helpers, not controllers.

## 6. `JacksonUtil`

Source capability: `com.ygzy.components.toolkit.util.jackson.JacksonUtil`

Use `JacksonUtil` for:

- shared `ObjectMapper` usage
- JSON serialization
- JSON deserialization
- generic conversion helpers

Guidance:

- Prefer `JacksonUtil` over creating ad hoc `ObjectMapper` instances in business code.
- Do not instantiate and reconfigure local mappers repeatedly.
- In review, flag private static `ObjectMapper` creation in business modules unless there is a clear isolation need.

## 7. `HttpUtil`

Source capability: `com.ygzy.components.toolkit.util.http.HttpUtil`

Use `HttpUtil` for:

- GET / POST / PUT / DELETE style outbound HTTP calls
- JSON requests
- form requests
- shared timeout and client configuration behavior

Guidance:

- Prefer this utility over local Apache HttpClient wrappers when the requirement is standard outbound invocation.
- Check log quality carefully when reviewing `HttpUtil` usage, because request and payload logging can expose sensitive values.
- If a business module creates its own low-level HTTP utility without a strong reason, flag it as duplicate infrastructure.

## 8. Excel Utilities

Source capabilities:

- `com.ygzy.components.toolkit.util.excel.ExcelExportUtil`
- `com.ygzy.components.toolkit.util.excel.ExcelReadUtil`

Use them for:

- template-based Excel export
- multi-sheet export
- multipart Excel import

Guidance:

- Prefer the shared Excel utilities over project-local EasyExcel wrappers.
- Keep template path, response handling, and error conversion aligned with the shared utility behavior.

## 9. `DictUtil`

Source capability: `com.ygzy.components.toolkit.util.dict.DictUtil`

Use `DictUtil` when assembling dictionary structures from shared `Dict` and `DictItem` models.

Guidance:

- Do not duplicate sort-and-wrap dictionary assembly logic in business modules.

## 10. `ThrowableUtil`

Source capability: `com.ygzy.components.toolkit.util.exception.ThrowableUtil`

Use `ThrowableUtil` for:

- unwrap logic
- runtime wrapping
- stack extraction
- exception-chain traversal
- message formatting helpers

Guidance:

- Prefer the shared exception utility over custom reflection-unwrapping or stack-to-string helpers.
- Do not use it as an excuse to swallow exceptions. Exception boundaries still need explicit business handling.

## 11. `ToolkitException`

Source capability: `com.ygzy.components.toolkit.exception.ToolkitException`

`ToolkitException` is the shared utility-layer exception and extends the base exception hierarchy.

Guidance:

- Use `ToolkitException` for toolkit-level failures.
- Do not throw `ToolkitException` directly from ordinary business services unless the failure is genuinely utility-layer in nature.
- Business modules should still prefer business-specific exception types where the project defines them.

## Review Checklist Additions

When reviewing code, explicitly check whether the implementation:

- recreated `Optional`-like null chaining instead of using `Op`
- recreated null-safe stream entry instead of using `St`
- recalculated paging ranges manually instead of using `PageUtil`
- parsed `BaseEnum` manually instead of using `EnumUtil`
- hand-built tree structures instead of using `TreeUtil`
- created local `ObjectMapper` or JSON helpers instead of using `JacksonUtil`
- created local HTTP wrappers instead of using `HttpUtil`
- created local Excel wrappers instead of using the shared Excel utilities
- wrote custom throwable unwrap/stack helpers instead of using `ThrowableUtil`

## Generation Guidance

When generating code:

- prefer toolkit utilities only when they match the problem naturally
- avoid introducing wrapper-on-wrapper abstractions around toolkit without a business reason
- avoid overusing `Op` and `St` where plain Java is clearer
- keep utility selection aligned with readability, not novelty
