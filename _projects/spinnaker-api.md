# Spinnaker API

Query JET Spinnaker pipelines, execution status, SEAL applications, and project entitlements — read-only via OAuth2 SSO

## Overview

The Spinnaker API skill provides read-only access to JET Spinnaker's REST API for pipeline inspection and deployment monitoring. It supports project and application discovery, pipeline configuration retrieval, execution status queries, entitlement validation, and SEAL application lookup. Authentication is OAuth2 SSO via IDAnywhere ADFS — no PAT fallback is available for Spinnaker.

## Metadata

| Field | Value |
|---|---|
| Plugin | spinnaker-api-plugin |
| Version | 1.0.1 |
| Compatibility | Windows |
| Runtime | PowerShell 7+ |
| Authentication | OAuth2 PKCE via IDAnywhere ADFS (SSO only — no PAT fallback) |
| Target | JET Spinnaker (`jet-spinnaker-api.jpmchase.net`) |

## Installation

1. Install `authentication-helper-plugin` from the CIB Marketplace.
2. Install `spinnaker-api-plugin` from the CIB Marketplace.

## What it provides

- List Spinnaker projects and applications
- Retrieve pipeline configurations and metadata
- Check pipeline execution status and history
- Validate user entitlements for a project
- Look up onboarded SEAL applications

All HTTP operations are `GET` only. Write operations are explicitly blocked.

## RBAC requirements

Access to Spinnaker projects requires membership in DevX Fabric groups:

- Format: `devx_{project}_deploy_read` (for example, `devx_gwpay_deploy_read`)
- Project IDs are DevX keys (for example, `GWPAY`), not SEAL IDs
- Contact the DevX Fabric team to request group membership

## Sample usage

List projects you have access to:

> "List all Spinnaker projects I have access to"

Inspect a pipeline:

> "Show me the configuration for the deploy-prod pipeline in project GWPAY, app gwpay-web"

Check execution status:

> "What is the status of Spinnaker execution 01HXR5G7K2ABCDEF?"

Verify entitlements:

> "Check my Spinnaker entitlements for project GWPAY"

List SEAL applications:

> "Which SEAL applications are onboarded to Spinnaker?"

## Troubleshooting

| Symptom | Likely cause | Resolution |
|---|---|---|
| `DEPENDENCY_MISSING` | authentication-helper-plugin not installed | Install `authentication-helper-plugin` from CIB Marketplace first |
| 403 on project access | Missing DevX DEPLOY_READ group | Check entitlements via the skill or request group access from DevX Fabric team |
| OAuth2 token not acquired | SSO session issue | Skill will reattempt authentication automatically; confirm consent if prompted |
| Project not found | Incorrect project key | Verify the DevX project key — it may differ from the Jira project key |
| Execution ID not found | Execution has expired or ID is incorrect | Spinnaker execution history has a retention window; verify the ID format |
