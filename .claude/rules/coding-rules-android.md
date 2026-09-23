---
paths:
  - "**/AndroidManifest.xml"
  - "**/res/**/*.xml"
  - "**/*.gradle"
  - "**/*.{kt,kts}"
  - "**/*.java"
---

# Android Coding Rules

Apply this file only when the project has an `AndroidManifest.xml`. Otherwise ignore it. The project's CLAUDE.md,
lint config, and existing architecture win over these rules. General Java rules live in `coding-rules-java.md`.

## Project Detection

- Identify the era first:
  - modern: Kotlin, Compose, Coroutines, Hilt
  - legacy: Java, XML layouts, RxJava, Dagger 2
  - deep legacy: Eclipse/Ant, no Gradle
- On Eclipse/Ant projects, never suggest `./gradlew` commands. Use `ant debug` or `ant release`.
- Read `minSdk` before using any platform API. Guard newer APIs with `Build.VERSION.SDK_INT` checks.
- Keep the existing architecture (MVVM, MVP, or MVC). Do not migrate it unless asked.

## Architecture and UI

- Keep logic out of `Activity` and `Fragment`. Put it in a `ViewModel` or presenter.
- Never do network or disk I/O on the main thread.
- Never store an `Activity`, `View`, or `Context` in a static field or long-lived object. Use the application context.
- Use ViewBinding instead of `findViewById`. In fragments, null the binding in `onDestroyView`.
- Hide data sources behind a repository. Use Room and Retrofit when the project already uses them.

## Security

- Keep secrets out of version control. Anything shipped in the APK can be extracted, so real secrets stay server-side.
- Keep cleartext traffic off (`android:usesCleartextTraffic="false"` or a network security config).
- Store sensitive local data in the Android Keystore or `EncryptedSharedPreferences`.
- Set `android:exported` explicitly on every component that has an intent filter.
- Request only the permissions the feature needs.
- Keep WebView JavaScript off unless required. Never use `addJavascriptInterface` with untrusted content.

## Java on Android

- Never convert working Java files to Kotlin unless asked.
- Check desugaring before using the Java 8 APIs: `java.util.stream` needs API 24 and `java.time` needs API 26.
  Below those levels, both need `coreLibraryDesugaringEnabled`.
- Mark public API nullability with `androidx.annotation` `@Nullable` and `@NonNull`.
- With RxJava, use `subscribeOn(Schedulers.io())` and `observeOn(AndroidSchedulers.mainThread())`.
- Dispose subscriptions in `onDestroy`, `onDestroyView`, or `ViewModel.onCleared()`.
- Never use `AsyncTask` in new code. It is deprecated since API 30.

## Kotlin

- The user rarely writes Kotlin. When a Kotlin idiom is unfamiliar, name its Java equivalent in one line in chat.
- Prefer `val` over `var`. Use data classes and sealed classes for models and UI state.
- Never use `!!`. Use `?.`, `?:`, or an explicit null check.
- Launch coroutines in `viewModelScope` or `lifecycleScope`, never `GlobalScope`. Use `Dispatchers.IO` for blocking
  work.
- Expose UI state as `StateFlow` and collect it with `repeatOnLifecycle`.
- In Compose, hoist state and keep side effects in `LaunchedEffect` or `DisposableEffect`.
- Add `@JvmStatic` or `@JvmOverloads` when Java code calls the Kotlin API.

## See Also

- `skills-md:android` for era detection, build systems, and architecture detail.
- `skills-md:java` for Java patterns, and `skills-md:owasp` for the security checklist.
