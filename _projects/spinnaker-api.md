---
layout: page
title: Spinnaker API
description: API reference information for Spinnaker.
importance: 3
category: developer
---

# Spinnaker API

Query the Acme Development Toolchain (ADT) Spinnaker pipelines, execution status, SID applications, and project entitlements

## Overview

The Spinnaker API skill provides read-only access to ADT Spinnaker's REST API for pipeline inspection and deployment monitoring. It supports project and application discovery, pipeline configuration retrieval, execution status queries, entitlement validation, and SEAL application lookup. Authentication is OAuth2 SSO via IDAnywhere ADFS — no PAT fallback is available for Spinnaker.

## Metadata

| Field          | Value                                                        |
| -------------- | ------------------------------------------------------------ |
| Plugin         | spinnaker-api-plugin                                         |
| Version        | 1.0.1                                                        |
| Compatibility  | Windows                                                      |
| Runtime        | PowerShell 7+                                                |
| Authentication | OAuth2 PKCE via IDAnywhere ADFS (SSO only — no PAT fallback) |
| Target         | ADT Spinnaker (`adt-spinnaker-api.acme.net`)                 |

## Installation

1. Install `authentication-helper-plugin` from the Marketplace.
2. Install `spinnaker-api-plugin` from the Marketplace.

## What it provides

- List Spinnaker projects and applications
- Retrieve pipeline configurations and metadata
- Check pipeline execution status and history
- Validate user entitlements for a project
- Look up onboarded SEAL applications

All HTTP operations are `GET` only. Write operations are explicitly blocked.

## Sample usage

List projects you have access to:

> "List all Spinnaker projects I have access to"

Inspect a pipeline:

> "Show me the configuration for the deploy-prod pipeline in project GWPAY, app gwpay-web"

Check execution status:

> "What is the status of Spinnaker execution?"

Verify entitlements:

> "Check my Spinnaker entitlements for project GWPAY"

List SID applications:

> "Which SID applications are onboarded to Spinnaker?"

## Troubleshooting

| Symptom                   | Likely cause                               | Resolution                                                                     |
| ------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------ |
| `DEPENDENCY_MISSING`      | authentication-helper-plugin not installed | Install `authentication-helper-plugin` from Marketplace first                  |
| 403 on project access     | Missing DEPLOY_READ group                  | Check entitlements via the skill                                               |
| OAuth2 token not acquired | SSO session issue                          | Skill will reattempt authentication automatically; confirm consent if prompted |
| Project not found         | Incorrect project key                      | Verify the Jira project key                                                    |
| Execution ID not found    | Execution has expired or ID is incorrect   | Spinnaker execution history has a retention window; verify the ID format       |
