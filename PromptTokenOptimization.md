# Lab: Prompt Engineering for Token Optimization with GitHub Copilot

## Lab Overview

**Objective:** Learn how to design effective prompts that minimize unnecessary context, reduce repeated instructions, and improve GitHub Copilot response efficiency without sacrificing code quality.

- **Repository:** SpringCopilot
- **Duration:** 30–45 minutes
- **Tools:** Git, Visual Studio Code, GitHub Copilot Chat
- **Level:** Beginner to Intermediate

## 1. Clone the Repository

Open a terminal and execute:

```bash
git clone https://github.com/akkki98/springcopilot.git
cd springcopilot
code .
```

Explore the project structure and select a relevant Java class or Spring component for the exercises.

## 2. Learning Objectives

By completing this lab, you will be able to:

- Identify token-wasting patterns in prompts.
- Convert vague prompts into focused, structured prompts.
- Apply scope constraints to limit unnecessary code exploration.
- Avoid repeating instructions and context.
- Use relevant files and concise outputs.
- Decide when to continue a chat and when to start a new one.
- Measure prompt efficiency using practical comparison criteria.

## 3. Understanding Token Optimization

Tokens are units of text processed by a language model. Depending on the model and Copilot feature, token usage can include instructions, conversation history, code, file context, and the generated response.

Token optimization is not simply about writing the shortest possible prompt. It is about providing the **minimum sufficient context to achieve the desired result**.

Effective prompt engineering follows this principle:

**Clear objective + relevant context + defined scope + output constraints = more efficient interaction**

## 4. Exercise 1: Transform a Bad Prompt into a Good Prompt

### Scenario

You want to understand a Spring controller in the SpringCopilot repository.

### Step 1: Submit a bad prompt

Open GitHub Copilot Chat and enter:

```text
Explain the entire SpringCopilot application.
Analyze all Java files, controllers, services, repositories,
configuration, dependencies, and tests.

Explain every class in detail and suggest all possible
improvements, optimizations, and best practices.
```

### Step 2: Identify token-wasting patterns

Review the prompt and identify these issues:

- **Broad scope:** Requests analysis of the entire application.
- **Unnecessary exploration:** Requests every class and configuration file.
- **Vague objective:** Does not identify a specific problem or outcome.
- **Excessive output:** Requests every explanation and all possible improvements.
- **No constraints:** Does not limit the analysis or response length.

### Step 3: Submit a good prompt

Start a fresh chat and enter:

```text
Explain the request flow for one Spring controller.

Scope:
- The selected controller.
- Its directly called service method.

Output:
- Controller responsibility.
- Service interaction.
- One potential improvement.

Constraints:
- Maximum 150 words.
- Do not inspect unrelated packages.
- Do not modify files.
```

Select a real controller and its related service from the repository.

### Step 4: Compare the results

| Evaluation | Bad prompt | Good prompt |
|---|---|---|
| Scope | Entire application | Selected component |
| Context relevance | Potentially broad | Targeted |
| Output length | Unbounded | Maximum 150 words |
| Unrelated analysis | Likely more | Explicitly limited |
| Token efficiency | Establish baseline | Compare with baseline |

**Expected learning:** Defining scope and output requirements can reduce unnecessary input and generated text.

## 5. Exercise 2: Apply the CLEAR Prompt Framework

Use this simple framework to structure efficient prompts.

| Element | Meaning | Example |
|---|---|---|
| C — Context | Essential background | Spring controller |
| L — Limit | Bound the scope | Controller and direct service only |
| E — Expected result | Define the deliverable | Explain request flow |
| A — Accuracy constraints | Preserve requirements | Do not change behavior |
| R — Response format | Control the output | Three bullets, maximum 150 words |

### Lab task

Rewrite the following vague prompt using CLEAR.

**Bad prompt:**

```text
Improve this code and make it better.
```

**Example improved prompt:**

```text
Context: A Spring service method processes an order.

Limit: Inspect only this method and its direct dependencies.

Expected result: Identify one significant readability
or performance improvement.

Accuracy: Preserve behavior and the public interface.

Response: Explain the issue and propose a minimal fix
in three bullets. Do not modify files.
```

Replace the example context with a real method from the repository before running the prompt.

## 6. Exercise 3: Optimize Debugging Prompts

### Scenario

A Spring component produces an error, and you need to identify its cause.

### Step 1: Use an inefficient prompt

```text
My application is not working. Check the entire project,
find every possible issue, explain all the errors, and
fix everything that might be wrong.
```

### Step 2: Use a focused debugging prompt

```text
Task: Diagnose the error below.

Error:
[Paste the exact error message and relevant stack trace.]

Relevant code:
[Provide the affected method or file reference.]

Scope:
Inspect the affected method and directly related code only.

Output:
1. Most likely cause.
2. Minimal recommended fix.
3. One verification step.

Constraints:
Do not modify unrelated files. Ask for missing information
if the evidence is insufficient.
```

### Step 3: Evaluate

Compare both responses for relevance, assumptions, unnecessary exploration, and follow-up prompts.

**Expected learning:** Specific error details and relevant code often provide more useful context than a request to inspect everything.

## 7. Exercise 4: Eliminate Repeated Instructions

### Scenario

You repeatedly ask Copilot to explain code using the same requirements.

**Inefficient approach:**

```text
Explain this method in simple terms.
Do not change any code.
Keep the answer short.
Only focus on the method.
```

Later:

```text
Now explain the next method in simple terms.
Do not change any code.
Keep the answer short.
Only focus on the method.
```

### Improved approach

When appropriate, establish the shared requirements once in the current conversation:

```text
For the next code explanations:
- Explain only the selected method.
- Use simple language.
- Keep each response under 100 words.
- Do not modify files.
```

Then follow up with:

```text
Explain the next method: [method name or code].
```

**Important:** Reuse these instructions only while they remain relevant. If you switch to an unrelated task, start a new chat and provide the instructions needed for that task. Also, do not assume that repeating instructions always causes the same token cost across Copilot features.

## 8. Exercise 5: Optimize Context and File Selection

### Step 1: Broad context

Ask Copilot to analyze the entire repository to explain one specific method.

### Step 2: Minimal sufficient context

Provide only:
- The relevant Java class.
- A directly called service or helper, if needed.
- The exact error or requirement, if applicable.

### Step 3: Compare

Record the outcome.

| Metric | Broad context | Focused context |
|---|---|---|
| Number of files provided or referenced | | |
| Response relevance | | |
| Unnecessary details | | |
| Follow-up prompts needed | | |
| Observed token usage, if available | | |

Use actual token counts only when your Copilot interface or available telemetry exposes them. Otherwise, mark token usage as **not measured** rather than estimating it from response length.

## 9. Exercise 6: Optimize Long Conversations

### Scenario

Your current chat contains unrelated discussions about controllers, database configuration, unit tests, and application architecture.

### Step 1: Generate a handoff summary

```text
Summarize the current task for a fresh chat in no more
than 120 words.

Include:
- Objective.
- Relevant classes and methods.
- Decisions already made.
- Constraints.
- What has been tried.
- Next action.

Exclude unrelated conversation history.
```

### Step 2: Start a new chat

Paste the summary and provide any essential code or files not included in it.

### Step 3: Continue the task

Ask Copilot to perform the next specific action.

**Expected learning:** A concise summary can preserve essential task context while excluding unrelated history.

Note: There is no universal 50–60% context threshold at which a model necessarily prioritizes recent turns. Use long-chat length as a reminder to review relevance, not as a guaranteed technical cutoff.

## 10. Exercise 7: Measure Prompt Efficiency

Choose one small, well-defined task in the SpringCopilot repository. For example, explain a method or propose a unit test.

Run the task twice using comparable conditions.

**Attempt A — Broad prompt**

- Include a broad request.
- Allow a general explanation.
- Record the response and any unnecessary details.

**Attempt B — Optimized prompt**

- State the specific objective.
- Limit the scope.
- Provide only necessary context.
- Specify a concise output format.

Record the results:

| Measurement | Attempt A | Attempt B |
|---|---|---|
| Task completed correctly? | | |
| Unnecessary content | | |
| Follow-up prompts required | | |
| Output length | | |
| Actual tokens, if exposed | | |

Keep the model, task, and other settings as similar as possible. If exact token counts are unavailable, evaluate relevance and output length as proxies—not as proof of token savings.

### Optional calculation

If comparable token counts are available:

Token reduction percentage:

`((Baseline tokens - Optimized tokens) / Baseline tokens) × 100`

Example: If a baseline uses 1,000 tokens and an optimized attempt uses 750 tokens, the reduction is 25%.

## 11. Best Practices Checklist

- [ ] Define one clear objective per prompt.
- [ ] Provide only relevant background and code.
- [ ] Limit analysis to the necessary files or methods.
- [ ] Specify the expected output and useful length constraints.
- [ ] Avoid vague requests such as “improve everything.”
- [ ] Reuse instructions when they remain relevant.
- [ ] Start a new chat when switching to an unrelated task.
- [ ] Summarize long conversations before restarting.
- [ ] Avoid attaching files just in case.
- [ ] Verify code suggestions before applying changes.
- [ ] Measure actual token usage when available.
- [ ] Never sacrifice correctness merely to shorten a prompt.

## 12. Reflection Questions

1. Which parts of the bad prompt created unnecessary scope?
2. How did the CLEAR framework improve your prompt?
3. Why is minimal sufficient context better than blindly minimizing context?
4. When should shared instructions be reused?
5. How can a long chat be summarized without losing essential information?
6. Which measurements can prove token reduction, and which are only proxies?

## Expected Outcome

After completing this lab, you should be able to design focused GitHub Copilot prompts, control unnecessary context, improve response relevance, and evaluate token efficiency using the SpringCopilot repository.

**Key takeaway:** Optimize for the smallest sufficient context and the clearest useful answer—not simply the shortest prompt.
