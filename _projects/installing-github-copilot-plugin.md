---
layout: page
title: Installing the GitHub Copilot Plugin
description: Instructions for installing the GitHub Copilot IDE plugin.
importance: 8
category: installation
---

## Overview

This page provides instructions for installing the GitHub Copilot IDE plugin. For more details on which IDEs are currently supported, see the Supported IDEs section.

Note that **all existing firmwide policies apply to your use of JPMC GitHub Copilot**, including the Code of Conduct, Information Safeguarding and Barriers Policy (and applicable supplement), Information Confidentiality Classifications Standard, and Acceptable Use of Technology and Information Assets Policy.

## Prerequisites

> **Warning — Proxy Disclaimer**
> **Important:** Understand and configure the recommended system proxy settings to ensure proper functionality. Copilot is compatible with system-configured proxy for Windows machines.
> You are responsible for finding and layering additional compliant proxy configurations your specific use case requires. The Copilot team will not support proxy configurations added at the DevShell or IDE level; the responsibility to engineer configurations appropriate for extraneous and bespoke use cases is not in scope for the JPMC Copilot team.

Complete **all the following steps** before proceeding with installation:

1. Ensure you have completed all steps in the Onboarding guide, including the **Prerequisites**.
2. Close any and all open IDE windows.
3. Ensure you have the latest version of IDE installed and activated:
   1. Determine if you are on the latest IDE version available in DevShell by running `os-tool list idea` for IntelliJ IDEA or `os-tool list vscode` for Visual Studio Code (VS Code), then ensure the latest version shows as `activated`.
   2. If you have Visual Studio (not to be confused with VS Code) installed from go/software, you must manually check what version you have installed (Help > About); ensure you are on version VS 2022 17.11 or later.
4. Run the following command to perform a clean installation of the latest IDE version, if you are not on the latest version:

   ```bash
   os-tool install -u "<latestIDEversion>"
   ```

5. Launch the freshly installed IDE by running `idea` or `rcode` in DevShell.

## Installation Steps by Platform

> **Warning — Compatible plugin versions**
> Ensure you install the latest compatible version of the Copilot plugin:
>
> - For Visual Studio Code, use Copilot that version 0.17+ or later. This also requires VS Code to be higher than version 1.91.
> - For IntelliJ IDEs, use Copilot version 1.5.6.5092 or later.

### Windows

At JPMC, the GitHub Copilot plugin is available for the following IDEs on Windows OS:

- IntelliJ IDEA
- Visual Studio Code (VS Code)
- Visual Studio

#### Installing Copilot for IntelliJ IDEA

1. Ensure you have completed and understood all the Prerequisites.
2. Understand the disclaimers detailed in Copilot Proxy Configuration and configure your settings accordingly.
3. Install the plugin to IntelliJ IDEA:
   1. Open IDEA and navigate to **File** > **Settings** > **Plugins**.
   2. Click on the **Marketplace** tab, then search for GitHub Copilot. Click **Install**.
   3. Click **Accept** for the Third Party Plugins Notice pop-up, then **Restart IDE**.
4. Authenticate your local IDE to your JPMC GitHub account:
   1. Click **Sign in to GitHub** from the pop-up, or click the Copilot icon in the bottom, right corner and select **Login to GitHub** from the menu.
   2. Click **Copy and Open** when the "Sign in to GitHub" pop-up displays with a device code. This opens a GitHub Sign-on window in your internet browser.
   3. Enter `<your SID>_jpmcai` in the **Username** field, leave the **Password** blank, then click **Sign in with your identity provider**.
   4. Click **Continue** when prompted to "Single sign on to jpmcai."
   5. If you receive a Microsoft/JPMC sign on page, enter your JPMC email in the **Email** field, then click **Next**. If prompted for additional credentials, enter your JPMC desktop login. If you do not receive these prompts, skip to the next step.
   6. Paste the device code copied from your IDE into the Device Activation field, then click **Continue**.
   7. Ensure your SID shows in the "Verify your GitHub identity [SID_jpmcai]" statement, then click **Authorize GitHub Copilot Plugin**. A success message displays.
   8. Return to your IDE, click on the Copilot icon, and confirm that it shows **Status: Ready**.
5. Review the Next Steps to begin leveraging Copilot.

#### Installing Copilot for Visual Studio Code (VS Code)

1. Ensure you have completed all the Prerequisites.
2. Understand the disclaimers detailed in Copilot Proxy Configuration and configure your settings accordingly.
3. Install the plugin to Visual Studio Code:
   1. Open VS Code by running `code` from your DevShell command line.
   2. Select the **building blocks icon** from the left toolbar to visit the **Extensions Marketplace**.
   3. Search for "GitHub Copilot", then click **Install** next to the Copilot extension.
4. Authorize VS Code to use GitHub Copilot:
   1. Select the **User** icon from the lower-left toolbar, then select **Sign in with GitHub to use GitHub Copilot**.
   2. Click **Allow** to allow the extension to sign in using GitHub at JPMC.
   3. Enter `<your SID>_jpmcai` in the **Username** field, leave the **Password** blank, then click **Sign in with your identity provider**.
   4. If you receive a Microsoft/JPMC sign on page, enter your JPMC email in the **Email** field, then click **Next**. If prompted for additional credentials, enter your JPMC desktop login. If you do not receive these prompts, skip to the next step.
   5. Ensure your SID shows in the "...wants to access your `<SID>_jpmcai` account", then click **Authorize Visual-Studio-Code**.
   6. Close the additional IDE session that opens as a result of signing in, and return to your original IDE session.
   7. Click **Cancel** from the sign-in pop up window, then click **Yes** when asked "Would you like to try a different way?"
   8. Again, ensure your SID shows in the "...wants to access your `<SID>_jpmcai` account", then click **Authorize Visual-Studio-Code**.

   Once the authorization is successful, the GitHub Copilot icon displays in the lower, right-hand corner of your IDE, indicating it is available for use.

5. Confirm that you are signed in with `SID_jpmcai` account by clicking on the profile icon in the lower, left-hand corner of your IDE.
6. Review the Next Steps to begin leveraging Copilot.

#### Installing Copilot for Visual Studio

1. Ensure you have completed all the Prerequisites.
2. Submit a software request for **Microsoft Visual Studio Enterprise v17.12+** via go/software. Search by Package Name "VS2022" (or by the software name "Microsoft Visual Studio").
3. Navigate to "Software Center" (from the Start menu) and install **VS2022-17-12+**, once you've received confirmation that your software request is complete.
4. Ensure that GitHub Copilot is selected as part of the installation options.
5. Launch Visual Studio from the Start menu.
6. Click **Sign In** from the top-right corner to sign in to Visual Studio. Note that this must be done before logging into GitHub Copilot.
   - Select your JPMC account when prompted for a Microsoft signin.
   - A success message displays once you are signed in.
7. Sign in to GitHub Copilot using your JPMC/GitHub account (`<SID>_jpmcai`).
   1. Click on the **GitHub Copilot** icon in the top-right corner, then click **Sign in**.
   2. Click **Sign in with your browser**. This opens a new browser window.
8. Enter your GitHub username as `<your SID>_jpmcai`, leave the **Password** field blank, then click **Sign in with your identity provider**.
9. If you receive a Microsoft/JPMC sign on page, enter your JPMC email in the **Email** field, then click **Next**. If prompted for additional credentials, enter your JPMC desktop login. If you do not receive these prompts, skip to the next step.

   The following success message displays once you are signed in.

10. Restart Visual Studio and verify it says "GitHub Copilot is active" or GitHub Copilot shows a green check mark. For example:

11. Review the Next Steps to begin leveraging Copilot.

### macOS

At JPMC, the GitHub Copilot plugin is available for the following IDEs on macOS:

- IntelliJ IDEA
- Visual Studio Code (VS Code)
- Xcode

#### Installing Copilot for IntelliJ IDEA on Mac

1. Ensure you have completed all the Prerequisites.
2. Configure the IntelliJ IDEA proxy settings:
   1. Open IntelliJ IDEA and navigate to **File** > **Settings** > **Appearance & Behavior** > **System Settings** > **HTTP Proxy**.
   2. Input `proxy.jpmchase.net` in the **Host** field.
   3. Input `10443` in the **Port** field.
   4. Input `*.jpmchase.net` in the **No proxy** field.
   5. Click **Apply**, then **OK**.
   6. Restart IntelliJ IDEA to apply the proxy settings.
3. Install the GitHub Copilot plugin:
   1. Navigate to **Preferences** > **Plugins**.
   2. Click on the **Marketplace** tab, search for GitHub Copilot. Click **Install**.
   3. Click **Accept** for the Third Party Plugins Notice pop-up, if applicable, then click **Restart IDE**.
4. Authenticate your IDE to your JPMC GitHub account:
   1. Click on the **Copilot icon** from the bottom toolbar, then select **Login to GitHub** from the menu.
   2. Click **Copy and Open**. This displays a pop-up window with a device activation code. This opens a GitHub Sign-on window in your internet browser.
   3. Enter `<your SID>_jpmcai` in the **Username** field, leave the **Password** blank, then click **Sign in with your identity provider**.

      Or, if you have already logged into the JPMCAI GitHub account directly, click the **Continue** button that is aligned with your `<SID>_jpmcai` account:

   4. Click **Continue** when prompted to "Single sign on to jpmcai."
   5. If you receive a Microsoft/JPMC sign on page, enter your JPMC email in the **Email** field, then click **Next**. If prompted for additional credentials, enter your JPMC desktop login. If you do not receive these prompts, skip to the next step.
   6. Paste the activation code copied earlier into the **Device Activation** screen, then click **Continue**.
   7. Ensure your SID shows in the "Verify your GitHub identity [SID_jpmcai]" statement, then click **Authorize GitHub Copilot Plugin**. A success message displays.
5. Return to your IDE. A pop-up stating **Successfully logged in to GitHub for GitHub Copilot** displays.
6. Review the Next Steps to begin leveraging Copilot.

#### Installing Copilot for Visual Studio Code (VS Code) on Mac

1. Ensure you have completed all the Prerequisites.
2. Configure the VS Code proxy settings:
   1. Select **File** > **Preferences** > **Settings**, then type "proxy" in the search bar.
   2. Input `http://proxy.jpmchase.net:10443` in the **http: Proxy** field.
   3. Click **Add Item** under **No Proxy**, then input `*.jpmchase.net` in the field.
3. Install the plugin to Visual Studio Code:
   1. Open VS Code by running `code` from your DevShell command line.
   2. Select the **building blocks icon** from the left toolbar to visit the **Extensions Marketplace**.
   3. Search for "GitHub Copilot", then click **Install** next to the Copilot extension.
   4. Restart VS Code.
4. Authorize VS Code to use GitHub Copilot:
   1. Select the **User** icon from the lower-left toolbar, then select **Sign in with GitHub to use GitHub Copilot**.
   2. Click the **Sign in** button that is aligned with your `<SID>_jpmcai` account.
   3. Enter `<your SID>_jpmcai` in the **Username** field, leave the **Password** blank, then click **Sign in with your identity provider**.
   4. Click **Continue** when prompted to "Single sign on to jpmcai."
   5. Enter your JPMC email in the **Email** field of the Microsoft/JPMC sign on page, then click **Next**.
   6. Enter your JPMC desktop login credentials, then click **Sign In**.
   7. Ensure your SID shows in the "...wants to access your `<SID>_jpmcai` account", then click **Authorize Visual-Studio-Code**.
5. Confirm that you are signed in with `SID_jpmcai` account by clicking on the profile icon in the lower, left-hand corner of your IDE.
6. Review the Next Steps to begin leveraging Copilot.

#### Installing Copilot for Xcode

1. Ensure you have updated Xcode version 26 or later. See the Prerequisites for more details on how to get Xcode for your corporate Mac.
2. Ensure you have completed all the Prerequisites.
3. Close any and all Xcode windows, if applicable.
4. Install the required applications via the `xServe` application:
   1. Search for "Copilot for Xcode", then click **Install**, then this can take over 10 minutes to complete; plan accordingly.
   2. Search for "Node" and click **Install** for `Node.js w/npm 20.18.0`.
5. Enable the Copilot extension:
   1. Open **System Settings** and navigate to **General** > **Login Items and Extensions**.
   2. Click on the information icon next to **Copilot for Xcode**, then click on the toggle for **Xcode Source Editor** and click **Done**. This enables the Copilot extension for Xcode.
6. Open the **Copilot for Xcode** application, if it does not automatically launch once installed.
7. Complete the required configurations in the **Service** tab:
   1. Complete the **Node Settings** section:
      1. Enter `/usr/local/bin/node` in the **Path to Node** field.
      2. Enter `/usr/bin/env` in the **Max Node** field.
         > Run the `which node` command in Terminal to verify these field inputs.
   2. Complete the **GitHub Copilot Language Server** section:
      1. Click **Install** to install the language server.
      2. Click **Sign In**. This displays a pop-up window, copies the device activation code (presented in the pop-up) to your clipboard, and opens the JPMC GitHub SSO login screen in your browser.
      3. Click **Continue** to proceed to the Device Activation screen.
      4. Paste the activation code copied earlier into your clipboard to the Device Activation screen.
      5. Click **Sign In** to authenticate to JPMC GitHub. A success message displays, indicating Copilot for Xcode has been authenticated.
      6. Click **OK** to close the device activation code pop-up.
      7. Click **Refresh** and confirm that the **Status** shows as **OK**.
   3. Complete the **Advanced** section:
      1. Select **Verbose logs**, if you would prefer lengthier log output. _This is optional._
      2. Select **Pretend IDE to be VSCode**. _Existing suggestions may not work if this is not selected._
   4. Complete the **Enterprise** section: Enter `https://github.com/settings/copilot` in the **Auth Provider URL** field.
   5. Complete the **Proxy** section as follows, if not selected:
      1. **Proxy Host:** `proxy.jpmchase.net`
      2. **Proxy port:** `9443`
      3. Select the **Proxy strict SSL checkbox**. _Note that you will not be able to complete the next step if this is not selected._
8. Navigate to the **Features** tab, select **Chat** from the left navigation pane, then complete the fields as follows:
   1. Select **edgeschat panel** for the **Open Chat Mode** field.
   2. Select **GitHub Copilot (pss)** for the **Chat model** field.
   3. Select **Use the default model** for the **Utility chat model** field.
   4. Select **GitHub Copilot (pss)** for the **Embedding model** field.
   5. Select **Auto-detected by LLM** for the **Reply in Language** field.
   6. Select your desired **Memory** option.
   7. Optionally, you can set a desired **Default system prompt**. JET recommends using the following prompt to start:

      ```
      You are a helpful senior programming assistant.
      You should respond in natural language.
      Your response should be correct, concise, clear, informative, and factual, etc.
      Use Markdown if you need to present code, table, list, etc.
      If you are asked to help perform a task, you MUST think step-by-step, then describe each step concisely.
      ```

   8. Select the desired **Font size of message**. The recommended size is `12`.
   9. Select `.AppleSystemUIFontMonospaced-Regular - 12 opt` for the **Font of code** field.
   10. Select the **Wrap text in code blocks** checkbox.
   11. Select the **Disable always-on-top** checkbox when the chat panel is detached from Xcode.
   12. Select the **Keep always-on-top** if the chat panel and Xcode overlaps and Xcode is active checkbox.
   13. Input the desired amount in the **Search plugin max iterations** field. The recommended value is `7`.
9. Open Xcode and enable the Copilot extension permission, if prompted.
10. To open the chat dialogue and begin using Copilot, use one of two methods; in either case, a separate chat window opens, allowing you to interact with Copilot:
    - Click the grey circle in the bottom-right corner of the Xcode editor window, then click the "+". This adds a new chat tab to the pop-up and turns the circle blue, indicating you have a Copilot chat open. If you close the window, the circle returns to grey; or,
    - Select **Editor** > **Copilot** > **Open Chat** from the Xcode menu bar.
11. Review the Next Steps below to leverage Copilot further.

## Next Steps

Review the following guides to get the most out of GitHub Copilot:

- Using Agent Mode
- Enabling Telemetry
- Determining Usage

## Troubleshooting

### Scenario 1: You Cannot Install the Plugin but Can Log in to GitHub with `SID_jpmcai` but Do Not See the "Copilot Business is active for your account" Message

If you cannot install the plugin and do not see the "Copilot Business is active for your account" message (when logging into https://github.com/settings/copilot), this likely means you do not have a license.

- Ensure you have a license to the tool by following the Onboarding steps.
- If you were previously able to install the plugin but are no longer able to access it, check to ensure you have logged out of your `SID_bemgmt` account, then log back in with your `<SID>_jpmcai` account. If you are still unable to log in, please reach out to your L2B POC for further assistance.

### Scenario 2: You Cannot Install the Plugin but Can Log in to GitHub and See the "Copilot Business is active for your account" Message

If you cannot install the plugin but see the "Copilot Business is active for your account" message when logging into https://github.com/settings/copilot, complete the following steps **in the order presented** to troubleshoot this issue:

1. Ensure you have configured the proxy properly (see steps above). Be sure to restart your IDE after any proxy setting change. **Note that inaccurate proxy configuration is an extremely common reason users have issues installing the plugin.**
   - If the Chrome browser opens with the login/device activation screen, that is indicative of an incorrect proxy configuration.
   - If you received either of these errors, these are indicative of an incorrect proxy configuration.
2. Ensure you've completed all the Prerequisites.
   - Only certain versions of the GitHub Copilot plugin are compatible with the latest version of IntelliJ IDEA. If you are using an older version of either, you may encounter issues.
   - If you are using a corporate Mac, there may be additional configurations needed, depending on use case. Review the steps above for configuring the **Login Items and Extensions** in your System Settings.
3. Check the GitHub Copilot Stack Overflow community for any known issues with the plugin.

If you are still unable to install the plugin after completing the steps above, reach out to your L2B POC for further assistance.

If you belong to the CIB Payments Technology organization, you may need to follow these workaround steps to successfully install the plugin. If you are unable to resolve the issue, visit go/MYitHelp or reach out to your L2B POC for further assistance.
