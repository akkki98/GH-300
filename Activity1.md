# Lab: Build a Simple Number Guessing Game Using GitHub Copilot Agent

## Lab Overview

In this lab, you will use **GitHub Copilot Agent Mode** to build a simple Number Guessing Game using HTML, CSS, and JavaScript.

The objective is to understand how GitHub Copilot Agent can:

* Generate application code
* Create multiple files
* Refactor code
* Fix bugs
* Add enhancements
* Explain generated code

---

## Prerequisites

* Visual Studio Code
* GitHub Copilot Subscription
* GitHub Copilot Chat Extension
* Agent Mode enabled in GitHub Copilot Chat

---

## Learning Objectives

By the end of this lab, you will be able to:

1. Use Copilot Agent to generate a complete application.
2. Modify application requirements using natural language.
3. Debug and improve code using Copilot.
4. Add new features through iterative prompting.

---

# Scenario

Create a browser-based Number Guessing Game where:

* The system generates a random number between 1 and 100.
* The player enters guesses.
* The application provides hints:

  * Too High
  * Too Low
  * Correct Guess
* The application tracks attempts.
* The player can restart the game.

---

# Task 1: Create a New Project

Create a folder:

```text
NumberGuessGame
```

Open the folder in VS Code.

---

# Task 2: Launch Copilot Agent Mode

Open Copilot Chat.

Select:

```text
Agent Mode
```

Verify the Agent indicator is visible.

---

# Task 3: Generate the Application

Provide the following prompt:

```text
Create a complete browser-based Number Guessing Game.

Requirements:
- Use HTML, CSS, and JavaScript.
- Generate a random number between 1 and 100.
- Allow the user to enter guesses.
- Display feedback if the guess is too high or too low.
- Show the number of attempts.
- Include a restart button.
- Use modern responsive styling.
- Create all required files.
```

---

## Expected Outcome

Copilot should generate:

```text
index.html
style.css
script.js
```

Review the proposed changes.

Accept all changes.

---

# Task 4: Run the Application

Open:

```text
index.html
```

using Live Server.

Verify:

* Game loads successfully.
* Input box is displayed.
* Guess button works.
* Restart button works.

---

# Task 5: Ask Copilot to Explain the Code

Prompt:

```text
Explain how the game logic works in script.js.
```

Observe how Copilot explains:

* Random number generation
* Event handling
* Attempt tracking
* Input validation

---

# Task 6: Improve the User Interface

Prompt:

```text
Enhance the user interface.

Requirements:
- Add gradient background.
- Add animations.
- Improve button styling.
- Improve mobile responsiveness.
- Use modern card-based design.
```

Accept the generated changes.

Run the application again.

---

# Task 7: Add Difficulty Levels

Prompt:

```text
Add difficulty levels:

Easy: 1-50
Medium: 1-100
Hard: 1-500

Allow the player to select a difficulty before starting the game.
Update the game logic accordingly.
```

Review and accept the changes.

---

# Task 8: Add Score Tracking

Prompt:

```text
Add a scoring system.

Requirements:
- Start with 100 points.
- Deduct 5 points for every incorrect guess.
- Display the current score.
- Show final score when the user wins.
```

Validate the functionality.

---

# Task 9: Debug Using Agent

Introduce an issue:

```javascript
let targetNumber = Math.floor(Math.random() * 100);
```

Prompt:

```text
The game sometimes does not allow the number 100 to be generated.
Find and fix the bug.
```

Observe:

* Agent investigates.
* Explains root cause.
* Updates code.

---

# Task 10: Add a Leaderboard

Prompt:

```text
Add a local leaderboard.

Requirements:
- Store top 5 scores.
- Use browser localStorage.
- Display leaderboard on the page.
- Sort scores in descending order.
```

Review generated code.

Test leaderboard functionality.

---

# Task 11: Refactor the Application

Prompt:

```text
Refactor the JavaScript code.

Requirements:
- Improve readability.
- Separate concerns into functions.
- Add comments.
- Follow JavaScript best practices.
```

Review the suggested improvements.

---

# Challenge Exercise

Use Agent Mode to implement:

1. Dark Mode
2. Sound Effects
3. Timer Countdown
4. High Score Persistence
5. Multiple Themes

Suggested Prompt:

```text
Enhance the game with advanced features including:
- Dark mode toggle
- Sound effects
- Countdown timer
- Theme selection
- Persistent high scores

Update all necessary files.
```

---

# Discussion Questions

1. How does Agent Mode differ from standard Copilot suggestions?
2. What tasks were completed automatically by the agent?
3. How did Agent Mode handle multi-file changes?
4. What debugging capabilities did you observe?
5. What limitations did you notice?

---

# Key Takeaways

* GitHub Copilot Agent can generate complete applications.
* Agent Mode can create and modify multiple files simultaneously.
* Natural language prompts can drive development workflows.
* Agent Mode assists with coding, debugging, refactoring, and enhancements.
* Human review remains important before accepting generated code.

---

# Lab Completion

Congratulations!

You have successfully built and enhanced a Number Guessing Game using GitHub Copilot Agent Mode while exploring application generation, debugging, and iterative development workflows.
