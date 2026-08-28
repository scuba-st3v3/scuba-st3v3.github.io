---
layout: post
title:  "cmux: A new kind of terminal"
date:   2026-08-28 10:00:00 -0500
categories: ai
permalink: /:categories/:title/
---

I spend enough time running coding agents that the terminal itself has become part of my development workflow. A few terminal tabs work for one project, but they become difficult to manage when several repositories, agents, test servers, and browser windows are active at the same time.

[cmux](https://cmux.com/) is a macOS terminal built on Ghostty. It adds vertical workspaces, split panes, an embedded browser, and notifications for processes that need attention. The important distinction is that cmux does not try to replace the command line or impose a particular agent workflow. It gives me a better set of primitives for organizing one.

I have been using cmux now for about 2 months in hopes of sunsetting usage of VSCode.

## Installing cmux

cmux requires macOS 14 or later. I install it with Homebrew:

```bash
brew tap manaflow-ai/cmux
brew install --cask cmux
```

After opening cmux for the first time, I verify that the terminal renders correctly, the workspace sidebar is visible, and commands run normally.

The command-line utility is available inside cmux terminals. To use it from another terminal, I add the bundled binary to my `PATH`:

```bash
sudo ln -sf "/Applications/cmux.app/Contents/Resources/bin/cmux" /usr/local/bin/cmux
```

A quick check confirms that the CLI can see the running application:

```bash
cmux list-workspaces
```

## The base layout

I keep the layout deliberately simple. Each project gets its own cmux workspace, and each workspace starts with a few predictable surfaces:

1. **Agent surface** — the coding agent working on the repository.
2. **Shell surface** — commands, logs, and manual inspection.
3. **Browser surface** — the application or documentation I need while working.

I split the terminal when two views need to remain visible at once, but I avoid creating panes just because cmux makes it easy. A layout should reduce context switching, not turn into another thing to manage.

The sidebar is useful because it keeps the project name, working directory, branch, listening ports, and notification state visible. I rename workspaces when the repository name is not enough to describe the task.

Press `⌘ ⌥ B` to toggle cmux’s right sidebar, which contains the Files/file-tree view. The quick file preview option is handy, especially for markdown files.

## Keeping the terminal familiar

cmux uses Ghostty's rendering engine and can read an existing Ghostty configuration. That means I can keep terminal preferences such as fonts, colors, themes, cursor behavior, and keybindings in the configuration I already use instead of maintaining a second visual setup.

This is one of the reasons I prefer cmux to a separate GUI orchestrator. The terminal remains the primary interface, and cmux adds organization around it. Shell scripts, Git commands, editors, and agents continue to work as normal terminal programs.

## Notifications and session restore

The most useful improvement is knowing which workspace needs me. When an agent pauses for input or finishes a task, cmux can show a notification in the panel, mark the workspace as unread, highlight the relevant pane, and optionally display a macOS notification.

Hooks need to be configured manually:

```bash
cmux hooks setup --agent opencode
```

Session restore is not the same as checkpointing every running process. Ordinary shells and unsupported applications reopen as terminals, but their live process state is not magically preserved. I treat restore as a reliable way to recover my workspace layout and agent context, not as a replacement for Git, tmux, or application-level checkpoints.

## Browser beside the terminal

The embedded browser is useful when the work involves a web application. I can keep the development server and its logs in one pane, the agent in another, and the application under test in a browser split.

That arrangement makes verification part of the same workspace. I can ask an agent to make a change, watch the server output, and inspect the result without switching between unrelated windows. The browser also exposes a scriptable API for tasks such as taking an accessibility snapshot, filling a form, or checking a page after a change.

I still use a normal browser when I need my full personal browsing environment. The cmux browser is most valuable when it is tied directly to a project or an automated verification step.

## A normal work session

My basic sequence looks like this:

1. Open or create the workspace for the repository.
2. Start the agent in the agent surface.
3. Keep a shell surface available for Git, tests, and logs.
4. Open a browser split when the project has a local web interface.
5. Let notifications tell me which workspace needs input.
6. Review the diff and run the relevant checks before considering the task complete.

This workflow is intentionally not complicated. The value comes from keeping several tasks visible and recoverable, not from building a large automation system around the terminal.

## What I like and what I am watching

The combination of vertical tabs, visible workspace metadata, and notification rings solves a problem that ordinary terminal tabs do not: it gives each agent session an identity. The embedded browser and CLI make the setup useful beyond simply splitting a window.

There are still boundaries. cmux is macOS-only, the most useful agent features depend on integrations being supported, and session restore should not be confused with durable process management.

For my base setup, that tradeoff is reasonable. cmux keeps the terminal at the center of the workflow while making parallel AI-assisted development easier to see, navigate, and resume.

- [cmux documentation](https://cmux.com/docs/getting-started)
- [cmux on GitHub](https://github.com/manaflow-ai/cmux)
