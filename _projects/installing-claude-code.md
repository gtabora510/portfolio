---
layout: page
title: Installing and Configuring Claude Code
description: Installing Claude Code for Windows 11 and macOS.
importance: 5
category: installation
---

> **Note for Early Access users:** If you were part of Early Access and used `npm` to install Claude Code, run the following command to uninstall the previous version before installing via Dev Shell:
>
> ```bash
> npm uninstall -g @anthropic/claude-code
> ```

## Overview

This guide provides instructions for installing and configuring Claude Code, an AI-assisted coding tool. It covers prerequisites, installation steps, and configuration details to ensure a smooth setup process.

Refer to the Availability Matrix for more details on which environments are currently supported.

## Prerequisites

You must have department permission to use Claude Code.

## Installation Steps by Platform

### Windows 11

1. Launch Dev Shell via Windows terminal:
   1. Open the **Terminal** application from your Windows Start menu.
   2. Open a Dev Shell instance by clicking the dropdown arrow in the tab menu and selecting **Dev Shell** from the list of profiles. Note that you can adjust the Terminal app's settings to default to Dev Shell if you prefer not to have to select it each time you open the Terminal app.
2. Install Claude Code by running:

   ```bash
   os-tool install claude-code
   ```

3. Verify the installation was successful by running the following command in your terminal:

   ```shell
   where claude
   ```

   ```
   # Output: C:\Users\<sid>\tools\claude-code\latest\claude.exe
   ```

### macOS

1. Open the **Self Service** app, search for "claude", then click **Install** next to the **Claude Code 2.1.x** offering.
2. Verify the installation by running the following command in your terminal:

   ```shell
   which claude
   ```

   ```
   # Output: /usr/local/jpmc/claude-code/latest/bin/claude
   ```

## Next Steps

### Run Claude Code

Once installed, `cd` into any directory and run `claude` to start, without specifying any models. This ensures that Claude can read the context of the folder and files you are working in, and route to the best model for the task at hand. For example:

- Sonnet handles daily coding tasks
- Opus handles complex reasoning tasks
- Haiku handles simple tasks, and is fast/efficient

Review the official Claude Code CLI Reference documentation for a list of CLI commands and their functions. For the list of operational commands using `/`, see Built-in commands.

### Complete Quickstart

Refer to the Quickstart Guide to learn how to use Claude Code, and the Claude Code Reference to learn about available features and capabilities. If you encounter any issues during installation or configuration, refer to the Support page for guidance on how to get help.
