---
layout: page
title: Create a deployment intent
description: Creating a deployment intent.
importance: 1
category: configuration
---

## Launch the Create Deployment Intent (DI) Wizard

> **Note:** The Context selector is not visible when you are within Analytics, Configuration, or Documentation. If you are in one of these areas and want to access the Context selector, you must click on the Developer Portal (DP) logo.

If you are in one of these areas and want to access the Context selector, you must click the DP logo.

1. From the Context selector, select **Projects**.
2. Select the Boundary Project to which you want to add a deployment scope (DS).
3. On the left navigation, select **DI/DS**.
4. Under **Actions**, click **Add deployment intent** to launch the wizard.

**Note:** You must have Project Owner permission and be in a Boundary Project to add a DI.

## Provide DI Details

Under **DI details**, provide the following information:

- **DI name:** Enter a name for the DS.
- **DI description:** Enter a description for the DS.
- **Environment:** Select the environment (DEV, TEST, PROD) in which to associate with this DI.

## Set Environment

Select one of the following environments to associate with the DI:

- **DEV**
- **TEST**
- **PROD**

## Set DI Controls

Set the following DI controls.

| Control attribute   | Description                                                                                                                                                                                                        |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Data Classification | Organizes and categorizes data based on sensitivity and importance, ensuring proper handling and protection.                                                                                                       |
| Connectivity        | Ensures applications communicate in an appropriate way that satisfies JPMC standards and risk allowance.                                                                                                           |
| Jurisdiction        | Refers to the legal authority or geographic area where certain laws, regulations, and rules apply.                                                                                                                 |
| HITrust             | HITRUST certification by the HITRUST Alliance enables vendors and covered entities to demonstrate healthcare data compliance requirements based on a standardized framework.                                       |
| PCI                 | Payment Card Industry (PCI) workloads are subject to additional scrutiny across all platforms. Refer to PCI Compliance to review obligations and initiate conversations.                                           |
| SOC                 | System and Organization Control (SOC) reports provide valuable information that clients require to assess and address the risks associated with use of an outsourced service.                                      |
| SOX                 | Sarbanes-Oxley Act of 2002 (SOX) is a wide-ranging landmark legislation that was designed to improve the overall quality of financial reporting, independent audits, and accounting services for public companies. |

| Values                                              | Mutability |
| --------------------------------------------------- | ---------- |
| confidential, highly confidential, internal, public | yes        |
| ext, int                                            | yes        |
| default, lux                                        | yes        |
| r2, e1, i1, none                                    | yes        |
| cat1, cat2, cat3                                    | yes        |
| soc1, soc2, none                                    | yes        |
| false, true                                         | yes        |

## Review Summary

- Validate the information provided in the steps to create the deployment intent.
- If information requires revising, click the **Edit** button to revise the information.
- After validating the information, click **Submit**.

## Submit Deployment Intent Request

- When the **Submit** button is clicked, the system returns you to the DI/DS screen where a snackbar confirms the request submission.
- Submitting generates an approval request that is sent to the Design Committee (DC) of the SID.

## Track Status

To track the status of the deployment intent creation workflow:

1. Access the Project through the **Projects** page or the context selector in the top-left of DP.
2. Select **Audit log** in the second-hand navigation menu.
3. Click **View details** on the deployment intent creation workflow.
4. Click on a step (represented as a node in the workflow visualization) to view its granular status.

## Approvals

- DP initiates an approval request to the DC of the corresponding SID.
- Requests created for Beta and Gold, the approvals go to **Approval PROD**.
- To view the following details of the DC request, select **Waiting for Approval**:
  - Request ID
  - Requested For
  - Description
  - DI information to be created

**Note:** If the L0B DC approval is taking longer than expected, contact the approvers listed.

- Upon approval, the workflow moves to DI creation.
- Acme resource numbers (ARNs) are generated and can be viewed by selecting the DI creation step:
  - DI
  - DS
  - DS resource registration

## Review Notifications

- Project creators receive notifications on the progress of the DI creation workflow.
- The notifications icon is located in the top right corner.
- Notifications are sent when the DI creation request is approved.
- Project owners are notified when the DC approves or declines the DI creation request.

## Retry Workflow Failures

- If a step fails during the workflow, retry re-triggers the process.
- Retry is only supported for certain failed steps in the workflow.
- The retry icon appears within the diagram view (accessed by clicking **View details** from within the Audit screen) where retrying is possible or enabled.
- Steps without the retry button cannot be retriggered.

**Retry is available in the following scenarios:**

While adding a DI to an existing project, if the smartApproval request expires or is rejected by the DC, retry re-submits the DI request to the DC.
