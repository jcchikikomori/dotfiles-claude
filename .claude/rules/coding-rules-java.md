---
paths:
  - "**/*.java"
---

# Java Coding Rules

The project's Checkstyle, Spotless, or formatter config and its CLAUDE.md win over these rules.

## Version First

- Read the Java version from `pom.xml` (`maven.compiler.release` / `source`) or `build.gradle` (`sourceCompatibility`
  or toolchain) before writing code.
- Treat **Java SE 8** as the baseline. Use a newer feature (`var`, `record`, text blocks, `switch` expressions,
  pattern matching) only when the project's version supports it.
- When you use a feature newer than Java 8, explain it in one line in chat, because the user knows Java 8 best.

## Style

- Name classes in `UpperCamelCase`, methods and fields in `lowerCamelCase`, constants in `UPPER_SNAKE_CASE`.
- Use one top-level class per file, named after the file.
- Use 4-space indentation and K&R braces.

## Java 8 Idioms

- Return `Optional<T>` instead of `null` from methods that may have no result. Never use `Optional` for fields.
- Use streams and method references for transformations, but use a plain loop when it reads better.
- Use `java.time` (`LocalDate`, `Instant`). Never use `java.util.Date` or `Calendar` in new code.
- Use `Collections.unmodifiableList` (Java 8) or `List.of` (Java 9+) for read-only collections.

## Design

- Use constructor injection. Avoid singletons and field injection.
- Keep controllers thin. Put business logic in services.
- Use a Builder for objects with many optional parameters.

## Errors and Resources

- Catch specific exceptions. Catch `Exception` only to re-throw or at the top-level handler.
- Never leave a `catch` block empty.
- Close resources with try-with-resources.

## Concurrency

- Use `ExecutorService` and `CompletableFuture`, not raw `Thread` objects.
- Prefer immutable objects. Guard shared mutable state with `java.util.concurrent` types or `Atomic*` classes.

## RxJava

- Keep every `Disposable` and dispose it when its owner's lifecycle ends (for example `CompositeDisposable`).
- Handle `onError` in every `subscribe` call. Never subscribe with only an `onNext` consumer.
- Use `subscribeOn` for where work starts and `observeOn` for where results arrive. Never block inside operators.
- Use `Flowable` when the source can outpace the consumer (backpressure). Use `Observable`, `Single`, `Maybe`, or
  `Completable` otherwise.
- Avoid `blockingGet()` and `blockingFirst()` outside tests. In tests, use `test()` and `TestObserver`.

## Security

- Use `PreparedStatement` or JPA parameters. Never concatenate user input into SQL.
- Use `SecureRandom` for tokens. Never use `Math.random()` or `java.util.Random` for secrets.
- Hash passwords with bcrypt or Argon2. Never use MD5 or SHA-1.

## Tests

- Use JUnit 5 (or the repo's JUnit version) with Mockito, and AssertJ where available.
- Compare expectations against literal values, not other method calls.

## See Also

- `skills-md:java` for patterns, concurrency detail, and tooling.
- `skills-md:spring-boot` and `skills-md:grails` for framework code.
- `skills-md:owasp` for the full security checklist.
