# Lab: Optimize GitHub Copilot Token Usage with Instruction Files

## Lab Overview

**Objective:** Reduce unnecessary instruction tokens in GitHub Copilot by keeping repository-wide instructions concise and moving specialized guidance into scoped instruction files.

- **Repository:** SpringCopilot
- **Duration:** 30–40 minutes
- **Tools:** Git, Visual Studio Code, GitHub Copilot
- **Level:** Beginner to Intermediate

## 1. Learning Objectives

By completing this lab, you will learn to:

- Optimize `.github/copilot-instructions.md`.
- Remove generic instructions that provide little additional value.
- Keep only project-specific knowledge that cannot be inferred easily from the code.
- Create scoped `.instructions.md` files using `applyTo` patterns.
- Understand how instruction files affect context and token usage.
- Compare broad instructions with concise, task-specific instructions.

## 2. Clone the Repository

Open a terminal and execute:

```bash
git clone https://github.com/akkki98/springcopilot.git
cd springcopilot
code .
```

Inspect the existing `.github` directory and determine whether the repository already contains instruction files.

If `.github/copilot-instructions.md` does not exist, create it.

## 3. Understand the Problem

GitHub Copilot instruction files provide context and guidance to the model. Instructions that apply to a request can consume context, and repeated agent steps may incur additional input processing.

**Important principle:** Keep shared instructions short and specific. Put specialized guidance in files that apply only to the relevant code.

For this lab, follow these guidelines:

- Keep `.github/copilot-instructions.md` concise.
- Include project-specific facts the model cannot reliably infer from source code.
- Remove generic instructions such as “write clean code.”
- Move language- or directory-specific conventions into scoped instruction files.
- Avoid duplicating the same rules across multiple files.

## 4. Exercise 1: Audit the Existing Instruction File

Open:

```text
.github/copilot-instructions.md
```

If the file is missing, create it with the following intentionally inefficient example.

### Before: Bloated instructions

```markdown
# Copilot Instructions

- Always write clean, readable, maintainable code.
- Follow coding best practices.
- Use meaningful variable names.
- Write efficient code.
- Add comments wherever necessary.
- Explain your changes in detail.
- Always test your code.
- Follow industry standards.
- Use good software design principles.
- Avoid unnecessary complexity.
- Build the project before making changes.
- Run all tests after every change.
- Use the existing project conventions.
- Use Maven to build the application.
- Run tests using Maven.
- Follow the repository's architecture.
- Never introduce bugs.
```

This example intentionally mixes generic advice, duplicated guidance, and potentially useful project-specific information.

### Lab task

Review each instruction and classify it as one of the following:

| Category | Action |
|---|---|
| Generic advice that Copilot can usually infer | Remove |
| Duplicate or vague guidance | Remove or consolidate |
| Verified project-specific build/test command | Keep |
| Important project-specific gotcha | Keep |
| Language-specific convention | Move to a scoped file when appropriate |

**Note:** Do not assume Maven is the correct build tool for this repository without checking its files. Verify the build commands before retaining them.

## 5. Exercise 2: Create a Short Global Instruction File

Replace the bloated example with a concise, project-specific file.

File:

```text
.github/copilot-instructions.md
```

### Example optimized template

Adapt this template after inspecting the repository.

```markdown
# Repository Guidance

- Follow the existing project structure and dependency versions.
- Use the build and test commands verified from the repository's build files.
- Preserve existing API behavior unless the task explicitly requires a change.
- Do not introduce new dependencies without explaining why they are needed.
- Follow any verified repository-specific setup or testing gotchas.
```

Replace the placeholder concept about build and test commands with actual commands once verified. For example, if the repository uses Maven, document the applicable Maven wrapper commands rather than assuming them.

### Why this is better

- Removes generic advice that adds little project-specific information.
- Avoids lengthy explanations repeated in multiple requests.
- Preserves essential constraints that affect code generation.
- Reduces the global instruction content applied across relevant tasks.

**Task:** Keep the global instruction file concise. Aim for approximately 5–10 meaningful lines as a practical target, not a mandatory limit.

## 6. Exercise 3: Create Scoped Instruction Files

Suppose the repository contains multiple languages or specialized directories. Instead of putting every convention in the global file, create scoped instruction files.

Directory structure:

```text
springcopilot/
└── .github/
    ├── copilot-instructions.md
    └── instructions/
        ├── python.instructions.md
        └── java.instructions.md
```

Create only the files relevant to the actual repository.

### A. Python-specific instructions

File:

```text
.github/instructions/python.instructions.md
```

Example:

```markdown
---
applyTo: "**/*.py"
---

# Python Guidance

- Use the Python version specified by the project.
- Follow the repository's existing formatter and lint configuration.
- Use the existing test framework and test commands.
- Do not add dependencies without justification.
```

### B. Java-specific instructions

File:

```text
.github/instructions/java.instructions.md
```

Example:

```markdown
---
applyTo: "**/*.java"
---

# Java Guidance

- Follow the Java version specified by the build configuration.
- Preserve existing package structure and dependency conventions.
- Follow the project's existing testing framework and patterns.
- Avoid introducing dependencies without justification.
```

These examples are starting points. Replace generic lines with verified conventions where possible.

### Understanding `applyTo`

The frontmatter pattern:

```yaml
---
applyTo: "**/*.py"
---
```

scopes the instruction file to Python files matching the pattern. Similarly, `**/*.java` scopes it to Java files.

This lets you keep language-specific guidance out of the global file. Confirm that the relevant instruction file is being applied in your Copilot environment.

**Important:** Scoped instruction files are not guaranteed to reduce total token usage in every interaction. They reduce unnecessary instruction context when the applicable files and task scope allow it. A broad agent task may still encounter several instruction files.

## 7. Exercise 4: Identify and Remove Redundant Instructions

Compare the following instructions.

| Instruction | Decision | Reason |
|---|---|---|
| Write clean code | Remove | Generic and difficult to verify |
| Follow best practices | Remove | Too broad to guide a specific action |
| Use `./mvnw test` | Keep if verified | Concrete and actionable |
| Java version is 21 | Keep if verified | Project-specific constraint |
| Use the existing test framework | Keep | Prevents unnecessary framework changes |
| Use the repository's current package structure | Keep | Protects architectural conventions |
| Explain everything in detail | Remove or scope | Can generate unnecessary output |
| Do not change public API behavior | Keep if relevant | Explicit compatibility requirement |

### Lab task

Edit the instruction files to remove at least five generic or redundant instructions.

For each removed instruction, explain why it did not provide useful additional project context.

## 8. Exercise 5: Compare Copilot Responses

Use the same task and model configuration for both attempts.

Example task:

> Explain how the selected Spring controller invokes its service. Do not modify files.

### Attempt A: Bloated instructions

Temporarily use the bloated instruction example from Exercise 1.

Ask Copilot to explain one controller and its directly related service.

Record the response quality and any unnecessary explanation.

### Attempt B: Optimized instructions

Use the short global instruction file and relevant scoped instruction file.

Repeat the same task in a fresh chat, with the same relevant files and equivalent context.

Record the results:

| Measurement | Attempt A | Attempt B |
|---|---|---|
| Task completed correctly? | | |
| Relevant files considered | | |
| Unnecessary response content | | |
| Follow-up prompts needed | | |
| Actual token usage, if available | | |

Do not claim token savings based only on shorter answers. Use actual token telemetry when available. Otherwise, treat relevance and response length as proxies.

## 9. Exercise 6: Validate the Instruction Design

Review the updated files and confirm:

- [ ] Global instructions contain only essential repository-wide guidance.
- [ ] Build and test commands are verified against the repository.
- [ ] Important setup issues and gotchas are documented.
- [ ] Generic lines such as “write clean code” are removed.
- [ ] Language-specific rules are moved to scoped files where appropriate.
- [ ] `applyTo` patterns match the intended file extensions.
- [ ] No instructions are duplicated without a clear reason.
- [ ] Copilot applies the expected instructions for the selected files.
- [ ] Suggested code still meets the project's correctness and quality requirements.

## 10. Best Practices

1. **Keep global instructions short.** They should contain high-value facts that matter across the repository.
2. **Prefer facts over slogans.** A verified command or compatibility constraint is more actionable than “follow best practices.”
3. **Scope specialized rules.** Use `.github/instructions/*.instructions.md` for language-, directory-, or task-specific guidance.
4. **Avoid duplication.** Repeating the same instruction across global and scoped files adds noise.
5. **Review instructions periodically.** Remove outdated commands and conventions when the repository changes.
6. **Measure rather than assume.** Actual token savings depend on instruction loading, model behavior, and the task.
7. **Keep correctness first.** Never remove an important constraint just to shorten the file.

## 11. Reflection Questions

1. Why should `copilot-instructions.md` remain concise?
2. Which instructions are useful because they cannot be inferred reliably from source code?
3. How does `applyTo` help separate global and specialized guidance?
4. Why might a scoped instruction file still be loaded for some tasks but not others?
5. What evidence would demonstrate an actual reduction in token usage?
6. How would you maintain instruction files as the repository evolves?

## Expected Outcome

By completing this lab, you should be able to organize GitHub Copilot instruction files so that repository-wide guidance stays concise, specialized instructions are scoped appropriately, and unnecessary context is minimized without compromising development quality.
