# Using VS Code 
With AI integrations, Quarto, and GitHub.

## Table of contents

### Quarto

- [R + Quarto in VS Code: Setup and Rendering Guide](#r--quarto-in-vs-code-setup-and-rendering-guide)
- [Install required software](#1-install-required-software)
- [Configure VS Code to use Quarto from RStudio](#4-configure-vs-code-to-use-quarto-from-rstudio)
- [Add a one-command preview task](#5-add-a-one-command-preview-task)
- [Add Run and Debug buttons (one-click preview)](#6-add-run-and-debug-buttons-one-click-preview)
- [Troubleshooting](#8-troubleshooting)

### AI

- [AI chatbot in VS Code (GitHub Copilot, including Education license)](#ai-chatbot-in-vs-code-github-copilot-including-education-license)
- [Sign in and enable Copilot](#sign-in-and-enable-copilot)
- [Recommended VS Code settings](#recommended-vs-code-settings)

### GitHub

- [Git and GitHub in VS Code (with Pull Requests)](#git-and-github-in-vs-code-with-pull-requests)
- [Initial one-time setup](#initial-one-time-setup)
- [Common commands and what they do](#common-commands-and-what-they-do)
- [Recommended order (daily workflow)](#recommended-order-daily-workflow)
- [Merge guidance](#merge-guidance)
- [VS Code UI equivalents (no terminal required)](#vs-code-ui-equivalents-no-terminal-required)


# R + Quarto in VS Code: Setup and Rendering Guide

This guide explains how to set up VS Code for working with R and Quarto, and how to render/preview documents with minimal friction.

## 1. Install required software

Install these first:

1. **R**
   - Download from: https://cran.r-project.org/
2. **RStudio** (optional but recommended in this workflow)
   - Download from: https://posit.co/download/rstudio-desktop/
   - Why: RStudio includes a Quarto binary you can reuse in VS Code.
3. **Quarto CLI**
   - Download from: https://quarto.org/docs/get-started/
   - Optional if you will use the Quarto binary bundled with RStudio, but recommended for a standard system-wide setup.

## 2. Install VS Code extensions

Install these extensions in VS Code:

1. **Quarto** (publisher: Quarto)
2. **R** (publisher: REditorSupport)
3. **R Extension Pack** (publisher: REditorSupport) (optional but useful)

You may also install:

- **Python** (if your `.qmd` files include Python chunks)

## 3. Verify what works where

Important distinction:

- **VS Code Command Palette commands** (like `Quarto: Preview`) must be run from Command Palette (`Ctrl+Shift+P`), not typed in an R console.
- **Shell commands** (like `quarto preview file.qmd`) must be run in a terminal such as PowerShell.
- If you type `Quarto: Restart Preview` in the R console, R treats it as code and throws an error.

## 4. Configure VS Code to use Quarto from RStudio

If `quarto` is not found in PowerShell, but rendering works in RStudio, point VS Code to RStudio's Quarto executable.

Create `.vscode/settings.json`:

```json
{
  "quarto.path": "C:\\Program Files\\RStudio\\resources\\app\\bin\\quarto\\bin\\quarto.exe"
}
```

This is the setup used in this repository.

## 5. Add a one-command preview task

Create `.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Quarto: Preview Current File",
      "type": "shell",
      "command": "C:\\Program Files\\RStudio\\resources\\app\\bin\\quarto\\bin\\quarto.exe",
      "args": [
        "preview",
        "${file}"
      ],
      "isBackground": true,
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "reveal": "always",
        "panel": "dedicated",
        "clear": false
      },
      "problemMatcher": []
    }
  ]
}
```

Because it is the default build task, users can start preview with:

- `Ctrl+Shift+B`

## 6. Add Run and Debug buttons (one-click preview)

Create `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Quarto Preview: Current File",
      "type": "node-terminal",
      "request": "launch",
      "command": "\"C:\\Program Files\\RStudio\\resources\\app\\bin\\quarto\\bin\\quarto.exe\" preview \"${file}\""
    },
    {
      "name": "Quarto Preview: Project",
      "type": "node-terminal",
      "request": "launch",
      "command": "\"C:\\Program Files\\RStudio\\resources\\app\\bin\\quarto\\bin\\quarto.exe\" preview"
    }
  ]
}
```

How to use:

1. Open a `.qmd` file.
2. Open **Run and Debug** in VS Code.
3. Select one of:
   - `Quarto Preview: Current File`
   - `Quarto Preview: Project`
4. Click the green play button.

## 7. Typical workflow for users

1. Open the project folder (the one containing `_quarto.yml`).
2. Open a `.qmd` file.
3. Start preview with either:
   - `Ctrl+Shift+B` (default build task), or
   - Run and Debug button.
4. Edit and save; preview auto-refreshes.

## 8. Troubleshooting

### `quarto` not recognized in terminal

- Either install Quarto system-wide, or
- Use `quarto.path` in `.vscode/settings.json` to point to RStudio's bundled Quarto.

### `Quarto: Restart Preview` gives R syntax errors

- You are running it in the R console.
- Run it from Command Palette (`Ctrl+Shift+P`) instead.

### Preview does not open

1. Confirm Quarto extension is installed.
2. Reload VS Code window.
3. Check terminal output from the task/debug session.
4. Verify path exists:
   - `C:\Program Files\RStudio\resources\app\bin\quarto\bin\quarto.exe`

## 9. Recommended project files to commit

Commit these files so all users get the same behavior:

- `.vscode/settings.json`
- `.vscode/tasks.json`
- `.vscode/launch.json`
- This guide: `vscode-r-quarto-setup.md`

# AI chatbot in VS Code (GitHub Copilot, including Education license)

This section explains how to add an AI assistant to VS Code for writing, editing,
explaining, and reviewing R/Quarto content.

## What to install

Install these in order:

1. **VS Code**
  - https://code.visualstudio.com/
2. **GitHub account**
  - https://github.com/
3. **GitHub Copilot extension** (publisher: GitHub)
4. **GitHub Copilot Chat extension** (publisher: GitHub)

For students/teachers:

- Check whether you qualify for GitHub Education benefits:
  https://education.github.com/
- If approved, activate Copilot through your eligible plan in GitHub settings.

## Sign in and enable Copilot

1. In VS Code, sign in to GitHub when prompted.
2. Open Command Palette and run **GitHub Copilot: Sign In** if needed.
3. Confirm Copilot status from the VS Code status bar.

## Recommended VS Code settings

Open settings (JSON) and configure preferences such as:

```json
{
  "github.copilot.enable": {
   "*": true,
   "plaintext": false,
   "markdown": true,
   "scminput": true
  },
  "github.copilot.chat.localeOverride": "en",
  "editor.inlineSuggest.enabled": true
}
```

Notes:

- Keep Copilot enabled for Markdown/Quarto files so it can help draft and edit text.
- You can disable suggestions per language if they become noisy.

## Safety and quality checks

When using AI text/code suggestions:

1. Verify function names and argument behavior in official docs.
2. Render locally with Quarto preview before committing.
3. Prefer short, clear examples and test all runnable chunks.

# Git and GitHub in VS Code (with Pull Requests)

This section explains a practical GitHub-first workflow in VS Code, including
which tools to install and the typical command order.

## What to install

Install these first:

1. **Git** (required)
  - https://git-scm.com/downloads
2. **GitHub Pull Requests and Issues** extension (publisher: GitHub)
3. **GitHub Repositories** extension (publisher: GitHub) (optional)

Without Git installed locally, VS Code Source Control features will not work.

## Initial one-time setup

In a terminal (PowerShell):

```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Then in VS Code:

1. Sign in to GitHub.
2. Open Source Control panel and confirm repository changes are visible.
3. Optionally enable auto-fetch in settings:
  - `git.autofetch: true`

## Common commands and what they do

Most common Git commands in day-to-day work:

- `git fetch` updates remote tracking info without changing your files.
- `git pull` fetches and integrates remote changes into your branch.
- `git add` stages specific file changes for commit.
- `git commit` records staged changes locally with a message.
- `git push` uploads local commits to GitHub.
- `git merge` combines another branch into your current branch.

## Recommended order (daily workflow)

Use this order to avoid conflicts and missing updates:

1. **Fetch latest remote state**
  - `git fetch`
2. **Pull latest changes into your branch**
  - `git pull`
3. **Edit files**
4. **Stage your changes**
  - `git add path/to/file`
  - or `git add .`
5. **Commit with a clear message**
  - `git commit -m "Describe what changed"`
6. **Push to GitHub**
  - `git push`

If your team uses feature branches and pull requests:

1. Create branch
  - `git checkout -b feature/my-change`
2. Make commits and push branch
  - `git push -u origin feature/my-change`
3. Open a Pull Request in VS Code using GitHub Pull Requests extension.
4. After review/approval, merge the PR on GitHub.
5. Update local main branch:
  - `git checkout main`
  - `git pull`

## Merge guidance

When to use merge:

- Use PR merge on GitHub for collaborative changes (recommended).
- Use local `git merge` when combining branches manually.

If a merge conflict appears:

1. Open conflicted files in VS Code.
2. Use conflict actions to accept current/incoming/both.
3. Re-test preview/render.
4. Stage resolved files and commit merge result.

## VS Code UI equivalents (no terminal required)

You can do most actions from Source Control:

1. **Fetch/Pull/Push** from the Source Control menu (three dots).
2. **Stage** with plus icons per file.
3. **Commit** via the message box and checkmark.
4. **Create PR** from GitHub Pull Requests view.

For most contributors, a safe habit is:

1. Fetch/Pull before starting edits.
2. Commit small, focused changes.
3. Push often.
4. Open PR early if collaborating.
