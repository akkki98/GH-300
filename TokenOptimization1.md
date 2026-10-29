# Lab: GitHub Copilot Token Optimization with SpringCopilot

## Lab Overview

**Objective:** Learn how to optimize GitHub Copilot token usage by managing chat sessions, writing focused prompts, and providing only relevant context.

- **Repository:** SpringCopilot
- **Duration:** 20–30 minutes
- **Tools:** Git, Visual Studio Code, GitHub Copilot Chat
- **Level:** Beginner

## 1. Clone the Repository

Clone the SpringCopilot repository and open it in VS Code.

```bash
git clone https://github.com/akkki98/springcopilot.git
cd springcopilot
code .
```

Explore the project structure before starting the exercises.

## 2. Learning Objectives

By completing this lab, you will learn to:

- Start a new chat when switching tasks.
- Continue a conversation when previous context is useful.
- Attach only relevant files.
- Recognize when a long conversation should be summarized or restarted.
- Compare broad prompts with focused prompts to reduce unnecessary context.

## 3. Exercise 1: Bad Prompt vs. Good Prompt

### Scenario

You need to understand a Spring component in the SpringCopilot repository.

### Step 1: Try a bad prompt

Enter this prompt in GitHub Copilot Chat:

```text
Analyze the entire SpringCopilot repository.
Review all Java files, configuration files, dependencies,
tests, and documentation.

Explain everything and suggest all possible improvements.
Include a complete overview of the application.
```

**Observe:**
- Does Copilot explore more code than necessary?
- Is the response longer than required?
- Does the response include unrelated details?

### Step 2: Try a good prompt

Start a fresh chat and enter:

```text
Task: Understand one specific Spring component.

Inspect only the relevant controller and its directly
related service class.

Explain:
1. The controller's responsibility.
2. How it calls the service.
3. One potential improvement.

Constraints:
- Do not analyze unrelated packages.
- Do not modify any files.
- Keep the explanation under 200 words.
- Ask for clarification if the component is not specified.
```

Select a real controller and its related service from the repository when performing the exercise.

### Step 3: Compare the results

Record your observations:

| Evaluation criterion | Bad prompt | Good prompt |
|---|---|---|
| Scope of analysis | | |
| Relevance of response | | |
| Unnecessary details | | |
| Follow-up prompts required | | |
| Perceived context efficiency | | |

**Learning point:** A focused prompt can reduce unnecessary context and produce a more useful response. Actual token savings depend on the model and Copilot feature.

## 4. Exercise 2: Decide When to Start a New Chat

### Scenario A: Continue the same chat

You asked Copilot to explain a Spring controller. Now you want to understand how its service method works.

Prompt:

```text
Explain how the service method called by the controller
works. Use the code discussed earlier and keep the
explanation focused on this request.
```

**Action:** Continue the same chat because the previous context is relevant.

### Scenario B: Start a new chat

You finished investigating the controller. Now you want to work on an unrelated feature or investigate a different issue.

Prompt:

```text
Task: Investigate the new issue described below.

[Insert the new issue or error message.]

Analyze only the relevant code and directly related
dependencies. Do not review unrelated features.
```

**Action:** Start a new chat to avoid carrying forward unnecessary discussion.

### Decision table

| Situation | Recommended action |
|---|---|
| Follow-up question about previously discussed code | Continue |
| Unit tests for the implementation discussed earlier | Continue |
| Switch to an unrelated feature | New chat |
| Investigate a separate bug | Usually new chat |
| Long chat contains unrelated tasks | Summarize, then start a new chat |

## 5. Exercise 3: Attach Only Relevant Files

### Step 1: Use broad context

Ask Copilot:

```text
Review the repository and identify why the application
is failing. Inspect all available source files and
configuration files.
```

### Step 2: Use focused context

In a fresh chat, provide the specific error message and relevant controller, service, or configuration file.

Prompt:

```text
Investigate the following error.

Error:
[Paste the exact error message.]

Relevant code:
[Paste the relevant method or provide the file reference.]

Identify the most likely cause and suggest the smallest
necessary fix. Explain your reasoning briefly.

Do not analyze unrelated files or modify code.
```

### Step 3: Compare

Consider:
- Which response was more relevant?
- Did the focused prompt reduce unnecessary explanations?
- Was additional context required to solve the issue?
- Did either approach require fewer follow-up prompts?

**Learning point:** Provide enough context to solve the problem, but avoid adding unrelated files just in case.

## 6. Exercise 4: Summarize and Restart a Long Conversation

### Scenario

You have been working with Copilot for a long time. The conversation includes debugging, code explanations, design discussions, and unrelated questions.

### Step 1: Request a summary

Enter this prompt:

```text
Summarize the current task for a fresh GitHub Copilot chat
in no more than 150 words.

Include:
- The task objective.
- Important decisions.
- Relevant files or methods.
- Constraints and requirements.
- What has already been tried.
- The next recommended step.

Exclude unrelated conversation history.
```

### Step 2: Start a new chat

Paste the summary into a new chat and attach or reference only the files required for the next step.

### Step 3: Continue the task

Ask Copilot to proceed with the next step based on the summary.

**Learning point:** A concise handoff preserves essential information without carrying forward the entire conversation.

Note: The 50–60% context-window guideline is a practical reminder to review long conversations, not a guaranteed technical threshold at which a model begins prioritizing recent messages.

## 7. Best Practices Checklist

- [ ] Start a new chat when switching to an unrelated task.
- [ ] Continue a chat when the next prompt depends on earlier responses.
- [ ] Define one clear objective per prompt.
- [ ] Specify the files, methods, or components to inspect.
- [ ] Attach only relevant files and code snippets.
- [ ] Avoid repeating information that remains available and relevant.
- [ ] Set a useful output limit when appropriate.
- [ ] Summarize decisions and next steps before restarting a long task.
- [ ] Review suggested changes before applying them.
- [ ] Follow organizational policies for source code and sensitive information.

## 8. Reflection Questions

1. Why can a broad prompt consume unnecessary context?
2. When should you continue an existing Copilot conversation?
3. How does selecting relevant files improve response quality?
4. What information should a handoff summary contain?
5. Why is 50–60% a heuristic rather than a fixed technical threshold?
6. Which chat habit will you adopt in your daily development work?

## Expected Outcome

After completing this lab, you should be able to manage GitHub Copilot conversations effectively, write focused prompts, reduce irrelevant context, and maintain response quality while working with the SpringCopilot repository.
