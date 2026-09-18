# LLM Code Evaluation Report — Assignment 0: Setup

## Student Information
- **Name**: Prince Ani
- **Date**: September 18, 2026
- **LLM Used**: Claude (Opus 5, via Claude Code)

## Prompt Used

```text
You are a friendly but precise teaching assistant for a graduate Java course. This is a SETUP
assignment (Assignment 0) — the only code change is that the student put their name into a Greeting
class so a test passes. Help me confirm my environment and workflow are correct. Do not rewrite my
code; just verify and explain.

Check the following and answer each briefly:
1. In my Greeting class, does message() return a greeting that includes MY name (not the placeholder
   "YOUR NAME HERE")? Quote the exact string it would return.
2. Given that, will the JUnit test messageNamesTheStudent PASS (it asserts the message is not blank
   and no longer contains "YOUR NAME HERE")? Will messageHasGreetingPrefix still pass?
3. In one or two plain-English sentences, explain what STUDENT_NAME and message() do, so I can say it
   in my own words during my code walk.
4. Did my tool output below indicate a clean run (tests green, coverage + checkstyle reports
   generated)? If something looks wrong, tell me what to re-check (JDK version, ./gradlew, etc.).

Here is my Greeting.java:
[contents of src/main/java/edu/northeastern/setup/Greeting.java, pasted]

Here is the output of my commands (paste what you saw):
[output of ./gradlew run and ./gradlew test jacocoTestReport checkstyleMain, pasted]
```

## What the LLM said

**1. Does `message()` include my name?**

Yes. `STUDENT_NAME` is `"Prince Ani"`, and `message()` returns the literal `"Hello, Java! — "`
concatenated with it. The exact returned string is:

```
Hello, Java! — Prince Ani
```

This matches the observed `./gradlew run` output exactly, so the compiled code and the source agree.

**2. Will the tests pass?**

Both pass.

- `messageNamesTheStudent` — asserts `!message.isBlank()` and `!message.contains("YOUR NAME HERE")`.
  The message is non-blank and the placeholder is gone, so both assertions hold. This test was
  failing before the edit (at `GreetingTest.java:24`, the `contains` assertion), which confirms the
  test is genuinely wired to the change rather than passing for an unrelated reason.
- `messageHasGreetingPrefix` — asserts `message().startsWith("Hello, Java!")`. Only the text after
  the em dash changed, so the prefix is untouched and this test stays green.

Confirmed by the run: 2 tests, 0 failures.

**3. What `STUDENT_NAME` and `message()` do (for the code walk)**

`STUDENT_NAME` is a `private static final` constant holding the student's name — one fixed value
shared by the class, not stored per object. `message()` is a `static` method that builds and returns
the full greeting by joining a fixed prefix to that constant, so callers get the string without ever
constructing a `Greeting`.

**4. Was the tool run clean?**

Yes, clean on all three counts.

- `./gradlew test` — `BUILD SUCCESSFUL`, both tests green.
- `./gradlew jacocoTestReport` — report written to
  `build/reports/jacoco/test/html/index.html`. Coverage reads **33%** overall. That is expected and
  not a defect: `GreetingTest` exercises `Greeting.message()`, but nothing tests `Main`, and
  `Greeting`'s private constructor is never invoked. Low coverage here reflects what the assignment
  asked for, not a gap in the work.
- `./gradlew checkstyleMain` — report written to `build/reports/checkstyle/main.html` with
  **0 violations**.

Environment checks all pass: `java -version` and `javac -version` both report 21.0.12.1 (Temurin
LTS), which satisfies the JDK 21 requirement; only one JDK is installed, so there is no chance of
the wrong one being picked up. All builds were run through `./gradlew` (the wrapper, pinned to
Gradle 8.14), never a hand-installed `gradle`. Nothing to re-check.

## Your checklist
- **`./gradlew run` prints the greeting with my name?**: Yes — `Hello, Java! — Prince Ani`
- **`./gradlew test` is green?**: Yes — 2 tests, 0 failures
- **Coverage + checkstyle reports generated?**: Yes — JaCoCo 33%, Checkstyle 0 violations
- **I can explain what `Greeting.message()` returns and why the test passes?**: Yes

## Reflection

[YOUR WORDS — 1–2 sentences. Things you could honestly say, based on what actually happened:
the toolchain checks passed first try (JDK 21 Temurin, VS Code with the Extension Pack for Java and
Gradle for Java already installed); the one thing worth noting is that `how-to-submit.md` documents
the copy path as `src/main/java/edu/northeastern/sketchpad/Greeting.java`, but the actual package in
this repo is `edu.northeastern.setup`, so running that command verbatim fails with "No such file or
directory" — the fix is to use the real path. Rewrite this in your own voice before submitting.]
