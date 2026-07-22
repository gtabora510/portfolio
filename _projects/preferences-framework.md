---
layout: page
title: Managing the Preferences Framework
description: Creating and managing user preferences across the Developer Portal.
importance: 2
category: configuration
---

## Overview

The **Preferences Framework** is a collection of configuration files and services that enable the creation and management of user preferences across the Developer Portal (DP). The framework provides a standardized way to define, store, view/interact and retrieve user preferences, allowing for a consistent user experience across the platform.

## Key Framework Features

| Feature                            | Description                                                                                          |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Centralized preferences management | Define and manage all user preferences in a single location.                                         |
| Flexible preferences types         | Support for various preferences types (boolean, string, number, select, etc.).                       |
| User-specific storage              | Preferences are stored on a per-user basis, allowing for personalized experiences.                   |
| Integration with DP applications   | Easily integrate preferences into any DP application or widget using the provided APIs and services. |
| Preferences UI components          | Built-in React components for rendering preferences controls in the UI based on the preference type. |
| Accessibility                      | A11y best practices built into the framework and design system components.                           |

## Core Concepts

| Concept                   | Description                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| Preferences Configuration | A JSON configuration that defines a preference — key, type, default value, and optional metadata (label, description, options for select types). |
| Preferences API           | API for interacting with the Preferences Services, including fetching and updating preferences values and configurations.                        |
| Preferences UI Component  | React components that render the appropriate input controls based on the preference type and handle user interactions to update preferences.     |

## What You Build vs. What the Platform Provides

You define your preferences using the framework's configuration json, and the platform provides the services and UI components to manage and render those preferences, which would allow you to consume those in your applications and widgets.

## Platform Applications

The DP Preferences Framework consists of multiple applications and libraries working together:

| Application                                  | Type     | Purpose                                                                                       | Docs   |
| -------------------------------------------- | -------- | --------------------------------------------------------------------------------------------- | ------ |
| dp-preferences-service/DP-preferences-bundle | Backend  | API for preferences metadata and values (definitions, user-specific values).                  | README |
| dp-preferences-ui                            | Frontend | Host shell that renders preference UIs and provides context for accessing preferences values. | README |
| dp-core                                      | Library  | Shared hooks, context providers, and utilities.                                               | README |
| dp/design-system                             | Library  | MUI-based design system components for rendering preferences controls.                        | README |
| omd-common                                   | Library  | Common utilities and types used across DP applications, eg change verification.               | README |

## Preferences API Reference

The Preferences API provides RESTful endpoints from the `dp-preferences-service` to interact with user preferences data. Use these endpoints to fetch and update preferences configurations.

### How Data Is Stored

The `dp-preferences-service` manages preferences data in a centralized database.

#### Preferences Configuration

| Column              | Type   | Description                                                                                                                        |
| ------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| `namespace`         | string | Uniquely identifies a preference configuration.                                                                                    |
| `version`           | string | Identifies changes in configuration from the user. The version is used in data retrieval. Refer to Data behavior for more details. |
| `author`            | string | SID of the user who uploads or updates the configuration.                                                                          |
| `configurationJson` | JSON   | Configuration object for the preference, including default value and metadata.                                                     |

#### Preferences

| Column      | Type   | Description                                                                                   |
| ----------- | ------ | --------------------------------------------------------------------------------------------- |
| `sid`       | string | ID of the user to whom the preference belongs.                                                |
| `namespace` | string | Uniquely identifies a preference configuration and links the preference to its configuration. |
| `name`      | string | Uniquely identifies a preference being altered. Synonymous with preference ID.                |
| `value`     | string | Value of the preference for the user, matching the required data field in stringified JSON.   |

### Data Behavior

Creating a preferences configuration does not store anything in the database until a user sets a value for that preference. Until then, retrieving the preference returns the default value in the specified format as defined by the configuration. Once a user sets a value for a preference, the system stores that value in the database and retrieves it on subsequent calls, even if the user selects the default option again.

Changing the configuration of a preference, such as adding a new option to a select-type preference, is tracked by a version number. When a preference is retrieved, the system checks if the version number stored with the user preference value differs from the configuration version. If the version numbers differ, one of the following occurs:

- If the change does not affect the values stored, the system returns the stored value and updates the version number to the new one. For example, changing the description of a preference.
- If the change affects the values stored:
  - When adding another required data field to the configuration, the system compares the existing value. If the previous entry still exists, the new field is added with the default value specified in the configuration. The merged value is saved and returned. For example, adding an entry to get the ID of the preference while keeping the value entry the same.
  - When removing or updating a required data field, the system removes the field and any associated data, replacing it with a new one or removing it entirely. The updated value is saved and returned. For example, changing the value entry to a different name or removing it. As this is the key field for storing the preference value, the system removes the old field and replaces it with a new one with the default value specified in the configuration, or removes it if there is no replacement field.
  - When removing an option or preference from the configuration, the system removes that entry from the database and does not return anything for that field. Depending on the retrieval method, an error may indicate that no data for that preference is found, or only the remaining preferences are returned.

### Workflow Diagram

A flow describing how the Preference API resolves data for a request:

1. **Preference API requested for data**
2. Check: **Preference interacted with by user?**
   - **No** → Extract default values from configuration → Preference API response returned.
3. If **Yes** → Check: **Version of data is less than configuration version?**
   - **No** → Use the stored data → Preference API response returned.
4. If **Yes** → Check: **Required data change?**
   - **No** → Use the stored data → Preference API response returned.
5. If **Yes** → Check: **No fields match?**
   - **Yes** → Existing data removed and replaced with default value new fields → Preference API response returned.
   - **No** → Merge existing data with new required fields with default values → Update database with merged data → Preference API response returned.

### API Endpoints

#### Preferences

| Method | Endpoint                                                                                                                       | Description                                                                                                                                                                                          | Response body                                                                              |
| ------ | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| GET    | `/api/v1/preference?sid={sid}&namespace={namespace}&preferenceId={preferenceId}`                                               | Retrieves the value of a specific preference for the current user. If the user has not set a value, the default value from the configuration is returned.                                            | `{ "sid": "string", "namespace": "string", "name": "string", "value": "string" }`          |
| GET    | `/api/v1/preferences?sid={sid}&namespace={namespace}`                                                                          | Retrieves all preference values for a given namespace for the current user. If a preference has not been set by the user, the default value from the configuration is returned for that preference.  | `[ { "sid": "string", "namespace": "string", "name": "string", "value": "string" }, ... ]` |
| POST   | `/api/v1/preference/bulkSids?namespace={namespace}`<br>Body: `{ "sids": ["string"], "namespace": "string", "name": "string" }` | Retrieves all preference values for a given namespace for the list of users. If a preference has not been set by the user, the default value from the configuration is returned for that preference. | `[ { "sid": "string", "namespace": "string", "name": "string", "value": "string" }, ... ]` |

#### Preferences Admin

| Method | Endpoint                                                                                                                  | Description                                                            | Response body                                                                 |
| ------ | ------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| GET    | `/api/v1/preference/configuration?namespace={namespace}`                                                                  | Retrieves a specific preference configuration for the given namespace. | `{ "namespace": "string", "version": "string", "title": "string", ... }`      |
| PUT    | `/api/v1/preference/configuration`<br>Body: `{ "namespace": "string", "version": "number", "configurationJson": "JSON" }` | Updates an existing configuration.                                     | `{ "namespace": "string", "version": "number", "configurationJson": "JSON" }` |
| POST   | `/api/v1/preference/configuration`<br>Body: `{ "namespace": "string", "version": "number", "configurationJson": "JSON" }` | Creates a new preference configuration.                                | `{ "namespace": "string", "version": "number", "configurationJson": "JSON" }` |

## Preferences Configuration

The DP Preferences Framework uses a JSON-based configuration format to define user preferences. This configuration specifies the namespace, type, default value(s), and optional metadata for each preference. The framework uses this configuration to manage preference storage, retrieval, and rendering in the UI.

### Configuration Structure

The preference configuration is a JSON object with the following structure:

```json
{
  "namespace": "string",
  "version": "string",
  "title": "string",
  "subtitle": "string",
  "requiredData": [
    {
      "key": "string",
      "type": "string"
    }
  ],
  "groups": [
    {
      "id": "string",
      "title": "string",
      "subtitle": "string",
      "preferences": [
        {
          "id": "string",
          "type": "string",
          "default": "any",
          "label": "string",
          "children": ["string"],
          "options": [
            {
              "id": "string",
              "label": "string",
              "value": "string"
            }
          ]
        }
      ]
    }
  ]
}
```

### Configuration Fields

| Field          | Type                          | Examples                                  | Description                                                                                                                                                                          | Required |
| -------------- | ----------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| `namespace`    | string                        | `"myapp"`                                 | Unique namespace for preferences, typically using reverse domain notation                                                                                                            | Yes      |
| `version`      | string                        | `"1.0"`                                   | Version of the preference configuration, useful for managing updates and changes. Intended for user usage only                                                                       | No       |
| `title`        | string                        | `"My App Preferences"`                    | Human-readable title for preferences                                                                                                                                                 | Yes      |
| `subtitle`     | string                        | `"Configure your preferences for My App"` | Optional subtitle providing additional context                                                                                                                                       | No       |
| `requiredData` | array of objects              | Refer to Required Data                    | List of key pair values used to create a preferences JSON that is stored in the database for that user and the preference being updated. Additional detail in Required Data section. | Yes      |
| `groups`       | array of objects (preference) | Refer to Groups                           | List of groups, each containing a set of related preferences                                                                                                                         | Yes      |

### Required Data

The `requiredData` field is an array of objects, where each object defines a key and type that will be used to create a preferences JSON object stored in the database. This allows specification of additional contextual information that may be necessary for preferences, such as specific naming conventions for application context.

| Field  | Type   | Examples                     | Description                                                            | Required |
| ------ | ------ | ---------------------------- | ---------------------------------------------------------------------- | -------- |
| `key`  | string | `"appName"`                  | The key for the required data field. Any string can be used.           | Yes      |
| `type` | string | Refer to Required Data Types | The type of the required data field. Used to extract the data to save. | Yes      |

Refer to the Examples section for variations.

### Required Data Types

The `type` field in the `requiredData` array can have the following values:

| Type      | Description                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------- |
| `VERSION` | Adds the current version within the configuration.                                                        |
| `VALUE`   | The value of the preference being updated. For example, a boolean preference type would be true or false. |
| `ID`      | The ID of the preference that has been updated.                                                           |

If required data is left empty, the default behavior is to store the preference id and the value being updated, for example: `{ "preferenceId": "value" }`. Refer to the Examples section for variations.

### Groups

The `groups` field is an array of objects, where each object defines a group of related preferences. Each group has its own ID, title, subtitle, and an array of preferences.

| Field         | Type             | Examples                      | Description                                                  | Required |
| ------------- | ---------------- | ----------------------------- | ------------------------------------------------------------ | -------- |
| `id`          | string           | `"displaySettings"`           | Unique identifier for the group                              | Yes      |
| `title`       | string           | `"Display Settings"`          | Human-readable title for the group                           | No       |
| `subtitle`    | string           | `"Configure display options"` | Optional subtitle providing additional context for the group | No       |
| `preferences` | array of objects | Refer to Preferences          | List of preferences within the group                         | Yes      |

Refer to the Examples section for variations.

### Preferences

Each preference within a group is defined as an object with the following fields:

| Field      | Type             | Examples                | Description                                                                                                                                                                                                                                                                    | Required |
| ---------- | ---------------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- |
| `id`       | string           | `"darkMode"`            | Unique identifier for the preference                                                                                                                                                                                                                                           | Yes      |
| `type`     | string           | `"boolean"`             | Type of the preference. Supported values: `"boolean"`, `"list"`, `"parent"`. This dictates the type of data for that preference in both the default value and what is stored when interacted with. Also determines which UI components are used when rendering the preference. | Yes      |
| `default`  | any              | `true` (for boolean)    | Default value for the preference. Varies depending on the preference type.                                                                                                                                                                                                     | Yes      |
| `label`    | string           | `"Enable Dark Mode"`    | Human-readable label for the preference                                                                                                                                                                                                                                        | Yes      |
| `children` | array of strings | `["darkModeIntensity"]` | Used to create a group of preferences with a select all option. Refer to the UI Component documentation for further examples.                                                                                                                                                  | No       |
| `options`  | array of objects | Refer to Options        | For list type preferences, an array of options that the user can choose from. Required only if one of those types is used.                                                                                                                                                     | Partial  |

Refer to the Examples section for variations.

### Options

The `options` field is an array of objects, where each object defines an option for list type preferences.

| Field   | Type   | Examples       | Description                                                     | Required |
| ------- | ------ | -------------- | --------------------------------------------------------------- | -------- |
| `id`    | string | `"light"`      | Unique identifier for the option                                | Yes      |
| `label` | string | `"Light Mode"` | Human-readable label for the option                             | Yes      |
| `value` | string | `"light"`      | The value that will be stored when the user selects this option | Yes      |

### Examples

#### Basic Single Choice List

This example sets up a preference where a user selects a single choice from a small list of options:

```json
{
  "namespace": "icecream",
  "version": "1.0",
  "title": "Ice Cream Flavors",
  "subtitle": "Select your favorite ice cream flavor",
  "requiredData": [],
  "groups": [
    {
      "id": "flavorGroup",
      "title": "Flavor Choices",
      "preferences": [
        {
          "id": "favoriteFlavor",
          "type": "list",
          "default": "vanilla",
          "label": "Favorite Ice Cream Flavor",
          "options": [
            {
              "id": "vanilla",
              "label": "Vanilla",
              "value": "vanilla"
            },
            {
              "id": "chocolate",
              "label": "Chocolate",
              "value": "chocolate"
            },
            {
              "id": "strawberry",
              "label": "Strawberry",
              "value": "strawberry"
            }
          ]
        }
      ]
    }
  ]
}
```

When rendered and the user changes their favorite flavor, as the required data is empty, the stored preference is:

```json
{
  "favoriteFlavor": "chocolate"
}
```

#### Basic Multi-Choice Preference

```json
{
  "namespace": "burgerToppings",
  "version": "1.0",
  "title": "Burger Toppings",
  "subtitle": "Select your ideal burger toppings",
  "requiredData": [],
  "groups": {
    "id": "appTheWay",
    "label": "All the way",
    "type": "Parent",
    "default": false,
    "children": ["ketchup", "mustard", "mayo", "lettuce", "tomato", "onions", "pickles", "cheese", "bacon", "mushrooms"]
  },
  {
    "id": "ketchup",
    "label": "Ketchup",
    "type": "boolean",
    "default": true
  },
  {
    "id": "mustard",
    "label": "Mustard",
    "type": "boolean",
    "default": true
  },
  {
    "id": "mayo",
    "label": "Mayo",
    "type": "boolean",
    "default": true
  }
}
```

This example provides a preference that allows users to select their ideal burger toppings. Each topping is a preference, allowing users to select or deselect any preference independently, except for the "All the way" option, which toggles all the options listed in the children field. As the required data is empty, the stored preference is as follows. The main difference from a list is that each preference that is altered is stored in the database. For example, if the user deselects Ketchup, the following is stored:

```json
{
  "ketchup": false
}
```

#### Preference with Required Data

```json
{
  "namespace": "theme",
  "version": "1.0",
  "title": "Theme Preferences",
  "subtitle": "Configure your theme settings",
  "requiredData": [
    {
      "key": "themeElement",
      "type": "ID"
    },
    {
      "key": "choice",
      "type": "VALUE"
    }
  ],
  "groups": [
    {
      "id": "mode",
      "title": "Light/Dark Mode",
      "preferences": [
        {
          "id": "modeChoice",
          "type": "list",
          "default": "light",
          "label": "Choose your Mode",
          "options": [
            { "id": "light", "label": "Light Mode", "value": "light" },
            { "id": "dark", "label": "Dark Mode", "value": "dark" },
            { "id": "system", "label": "System Default", "value": "system" }
          ]
        }
      ]
    },
    {
      "id": "colorScheme",
      "title": "Color Scheme",
      "preferences": [
        {
          "id": "colorChoice",
          "type": "list",
          "default": "blue",
          "label": "Choose your Color Scheme",
          "options": [
            { "id": "blue", "label": "Blue", "value": "blue" },
            { "id": "red", "label": "Red", "value": "red" },
            { "id": "green", "label": "Green", "value": "green" }
          ]
        }
      ]
    }
  ]
}
```

This example has two single choice groups and two methods of interaction. There are entries in the required data, so when any interaction with the preferences occurs, the stored preference would look like this if the mode is altered:

```json
{
  "themeElement": "modeChoice",
  "choice": "dark"
}
```

The key point is that for list types, options must be provided.

## Define Your Preferences

Set up your preferences configuration JSON to define the namespace, types, default value, and any additional metadata. The following example shows the preferences configuration:

```json
{
  "namespace": "icecreams",
  "version": 1,
  "title": "Ice Cream Flavors",
  "subtitle": "Use this to choose your favorite ice cream flavor",
  "requiredData": [],
  "groups": [
    {
      "id": "iceCreamFlavors",
      "preferences": [
        {
          "id": "flavor",
          "type": "list",
          "options": [
            { "id": "strawberry-flavor", "label": "Strawberry", "value": "strawberry" },
            { "id": "rum-raisin-flavor", "label": "Rum Raisin", "value": "rum-raisin" },
            { "id": "mint-chocolate-chip-flavor", "label": "Mint Chocolate Chip", "value": "mint-chocolate-chip" },
            { "id": "vanilla-flavor", "label": "Vanilla", "value": "vanilla" }
          ],
          "default": "rum-raisin-flavor"
        }
      ]
    }
  ]
}
```

### Key Configuration Fields

| Field          | Description                                                                                                               |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `namespace`    | Unique identifier for your preference, used for retrieval and storage.                                                    |
| `version`      | Version number for your preference configuration, used for managing updates.                                              |
| `title`        | Human-readable title for your preference, displayed in the UI.                                                            |
| `subtitle`     | Optional subtitle providing additional context for the preference.                                                        |
| `requiredData` | Array of key-value pairs describing data stored for that preference. If empty, defaults to `preference.id : value`.       |
| `groups`       | Array of preference groups, each containing an `id` and an array of `preference`.                                         |
| `preferences`  | Array of individual preference definitions, each with an `id`, `type`, `options` (for select types), and `default` value. |

### Upload Your Preferences Configuration

Go to the Swagger for DP Preferences Service: DP Platform

**POST** `/api/v1/preference/configuration`

1. Click **"Try it out"**.
2. Add your Change number from your Snow ticket in the `changeRequest` field (required in production, optional in non-production environments).
3. Fill in the request body with your details and preferences configuration:

```json
{
  "namespace": "icecreams",
  "version": 0,
  "configurationJson": {
    "namespace": "icecreams",
    "version": 1,
    "title": "Ice Cream Flavors",
    "subtitle": "Use this to choose your favorite ice cream flavor",
    "requiredData": [],
    "groups": [
      {
        "id": "iceCreamFlavors",
        "preferences": [
          {
            "id": "Flavor",
            "type": "list",
            "options": [
              { "id": "strawberry-flavor", "label": "Strawberry", "value": "strawberry" },
              { "id": "rum-raisin-flavor", "label": "Rum Raisin", "value": "rum-raisin" },
              { "id": "mint-chocolate-chip-flavor", "label": "Mint Chocolate Chip", "value": "mint-chocolate-chip" },
              { "id": "vanilla-flavor", "label": "Vanilla", "value": "vanilla" }
            ],
            "default": "rum-raisin-flavor"
          }
        ]
      }
    ]
  }
}
```

4. Click **"Execute"** to send the request and create your preferences configuration. A successful response returns status 201 with the details in the response body.
5. Verify retrieval:
   - **GET** `/api/v1/preference/configuration`
     - Enter your preferences namespace (e.g., `icecreams`) in the `namespace` text box and execute.
     - A 200 response returns your preferences configuration details in the response body.

### Add Preferences to Menu

1. After verifying the preferences is retrievable via the API, add it to the user preferences menu in DP by updating the menu configuration in the DP Registry Swagger.
2. Go to the DP Registry Swagger: DP Registry
3. **POST** `/api/v1/module`
   - Click **"Try it out"**
   - Add your Change number from your Snow ticket in the `changeRequest` field (required in production, optional in non-production environments)
   - Fill in the request body with the following details:

```json
{
  "id": "preferences_icecreams",
  "description": "A user can manage their preferred ice cream flavor in this menu",
  "parents": ["preferences"],
  "instances": [
    {
      "instanceId": "",
      "instanceArea": "MY",
      "navigationEnabled": "ENABLED",
      "navigationItems": [
        {
          "name": "Ice Cream Flavors",
          "icon": {
            "iconName": "",
            "type": ""
          },
          "parent": "preferences",
          "subRoute": null,
          "priority": 200,
          "level": 2,
          "navigationEntryType": null,
          "navigationTarget": null,
          "crossContextTooltip": null
        }
      ],
      "featureFlag": {
        "name": "dummyText",
        "effect": "DISABLED"
      }
    }
  ],
  "route": "/preferences/icecream",
  "sid": "111111",
  "supportEmail": "dp_sparc@acme.com",
  "supportQueue": "dp_sparc@acme.com",
  "type": "FEDERATED",
  "loading": {
    "remoteEntryUrl": "https://dp-preferences-ui.devplatform.prod.aws.acme.net/remoteEntry.js",
    "moduleFederationName": "DP_Preferences",
    "moduleFederationExposes": "./PreferencesApp",
    "storeConfig": null
  }
}
```

#### Key Fields in the Request Body

| Field            | Path                             | Description                                                                                                                                                                                                                  |
| ---------------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`             | `id`                             | Unique identifier for the menu node. Use the convention `preferences-[namespace]`, for example `preferences-icecream`                                                                                                        |
| `name`           | `instances.navigationItems.name` | The display name in the side bar menu.                                                                                                                                                                                       |
| `route`          | `route`                          | The URL used to access the preference and render your config. Use `/preferences/[namespace]` and match the namespace in your configuration, for example `preference/icecream`.                                               |
| `sid`            | `sid`                            | Your team's system ID.                                                                                                                                                                                                       |
| `supportEmail`   | `supportEmail`                   | Contact email for your team.                                                                                                                                                                                                 |
| `remoteEntryUrl` | `loading.remoteEntryUrl`         | Endpoint used by the module that processes the UI components. Adjust only the environment part, for example: `https://dp-preferences-ui.devplatform.prod.aws.acme.net/remoteEntry.js`, to `dev`, `uat`, or `prod` as needed. |

1. Click **"Execute"** to send the request and create your menu node.
2. Update the DP Registry Swagger manually with the provided JSON and adjust the fields as needed.

### Use Your Preferences

After setting up and adding your preferences to the menu, use the Preferences API to retrieve data that a user has interacted with via the rendered UI components.

Workflow: Preference API requested for data → Check if preference was interacted with by user → **Yes**: Preference API returns the preference value stored in the backend → **No**: Preference API returns the default value set for that preference.

Three endpoints are available for retrieving preferences data, depending on your needs:

| Endpoint                                                                                                                            | Description                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET `/api/v1/preference?sid={sid}&namespace={namespace}&preferenceId={preferenceId}`                                                | Retrieve the value of a specific preference for the current user. If the user has not set a value, returns the default value from the configuration.                                                     |
| GET `/api/v1/preferences?sid={sid}&namespace={namespace}`                                                                           | Retrieve all preference values for a given namespace for the current user. If a preference has not been set by the user, the default value from the configuration will be returned for that preference.  |
| POST `/api/v1/preference/bulkSids?namespace={namespace}`<br>Body: `{ "sids": ["string"], "namespace": "string", "name": "string" }` | Retrieve all preference values for a given namespace for the list of users. If a preference has not been set by the user, the default value from the configuration will be returned for that preference. |

### Test Your Preferences

Open the DP Portal and select the user preferences menu (usually in the top right corner under your profile) to test your preferences. Your new preferences (for example, "Ice Cream Flavors") should appear in the menu. Interact with the preferences UI, change the value, and save it. Use the Preferences API endpoints to confirm that the updated value stores and retrieves correctly. The API also returns the default value when you have not set a value for the preference.

## Preferences UI Components

The DP Preferences Framework provides a set of React components for rendering user preferences in the UI. These components are predefined for visual consistency and are selected based on the preference type defined in the configuration. The following sections explain which `type` relates to which component is rendered and what interaction result you can expect.

| Preference Type | UI Component | Description                                                                                      | User Interaction                                                              | Example                 |
| --------------- | ------------ | ------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- | ----------------------- |
| `boolean`       | `Toggle`     | Renders a toggle switch for boolean preferences.                                                 | Users toggle the switch on or off to update the preferences value.            | Toggle Example          |
| `list`          | `RadioGroup` | Renders a group of radio buttons for single-select preferences.                                  | Users select one option from the list to update the preferences value.        | Radio Example           |
| `parent`        | `Toggle`     | Renders a toggle switch for parent preferences that control the visibility of child preferences. | Users toggle the switch on or off to affect the associated child preferences. | Toggle (parent) Example |

### Examples of Each Preference Type

#### Boolean Type

When you define preference type `boolean` in the preferences configuration, the framework renders a `Toggle` switch in the UI. Users interact with the toggle to turn it on or off, which updates the corresponding preferences value in the backend.

**Rendered Example:** Page Title / Page Subtitle → Group Title → "Preference Label" toggle.

**Boolean Preferences Configuration Example**

```json
{
  "namespace": "exampleTypeBoolean",
  "groups": [
    {
      "preferences": [
        {
          "id": "exampleBooleanPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        }
      ]
    }
  ]
}
```

#### List Type

When you define preferences type `list` in the preferences configuration, the framework renders a `RadioGroup` in the UI. Users select one option from the list of radio buttons, which updates the corresponding preferences value in the backend.

**List Preferences Configuration Example**

```json
{
  "namespace": "exampleTypeList",
  "groups": [
    {
      "preferences": [
        {
          "id": "exampleListPreference",
          "label": "Preference Label",
          "type": "list",
          "options": [
            { "id": "option-1", "label": "Option 1", "value": "option1" },
            { "id": "option-2", "label": "Option 2", "value": "option2" }
          ],
          "default": "option1"
        }
      ]
    }
  ]
}
```

**Rendered Example:** Page Title / Page Subtitle → Group Title → radio buttons "Option 1" (selected), "Option 2".

### Parent Type

#### General Behavior

When you define preferences type `parent` in the preferences configuration, the framework renders a `Toggle` switch in the UI. This parent preferences also creates a grouping effect: preferences listed in the children field of the parent preference appears indented under the parent preferences in the UI. Toggling the parent preferences off turns off all child preferences, and toggling it on turns on all child preferences. If a user toggles on any single child preferences, the parent preferences toggles on as well.

#### Preferences Storage Behavior

The parent preference has a toggle and entry in the configuration, but the system does not create a database entry for it. When a user selects all on the parent preferences, the system triggers a single call for each child, and those preferences are stored in the database. If a user toggles on a single preferences within the child group, only that preferences has an entry in the database, not the parent preferences, even though the parent appears toggled on.

#### Parent-Child Nesting Behavior

Each child of a parent should appear directly under the parent preferences in the configuration to render the grouping effect properly in the UI. The UI visually indents child preferences under their parent. The framework does not limit the number of nesting levels, but for clarity and user experience, use no more than two levels (one parent and its children). For deeper nesting, create a new grouping with the desired parent-child structure rather than nesting further. Achieve nesting by making one of the children preferences a `parent` type itself, referencing its own children, and placing those child preferences another indent level down in the UI.

#### Parent Preferences Configuration Examples

**Single Level of Nesting Example**

```json
{
  "namespace": "exampleTypeParent",
  "groups": [
    {
      "preferences": [
        {
          "id": "exampleParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": false,
          "children": ["exampleChildPreference"]
        },
        {
          "id": "exampleChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        }
      ]
    }
  ]
}
```

**Multiple Levels of Nesting Example**

```json
{
  "namespace": "exampleTypeParent",
  "groups": [
    {
      "preferences": [
        {
          "id": "exampleParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleChildPreference", "exampleChildParentPreference"]
        },
        {
          "id": "exampleChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleGrandChildPreference"]
        },
        {
          "id": "exampleGrandChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        }
      ]
    }
  ]
}
```

**Complex Example** (two parent groups, each with nested child and grandchild parent/boolean preferences following the same previous pattern, duplicated with a second set of preferences suffixed `2`):

```json
{
  "namespace": "exampleTypeParent",
  "groups": [
    {
      "preferences": [
        {
          "id": "exampleParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleChildPreference", "exampleChildParentPreference", "exampleChildPreference2", "exampleChildParentPreference2"]
        },
        {
          "id": "exampleChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleGrandChildPreference"]
        },
        {
          "id": "exampleGrandChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildPreference2",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildParentPreference2",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleGrandChildPreference2"]
        },
        {
          "id": "exampleGrandChildPreference2",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        }
      ]
    },
    {
      "preferences": [
        {
          "id": "exampleParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleChildPreference", "exampleChildParentPreference", "exampleChildPreference2", "exampleChildParentPreference2"]
        },
        {
          "id": "exampleChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildParentPreference",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleGrandChildPreference"]
        },
        {
          "id": "exampleGrandChildPreference",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildPreference2",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        },
        {
          "id": "exampleChildParentPreference2",
          "label": "Preference Label",
          "type": "parent",
          "default": true,
          "children": ["exampleGrandChildPreference2"]
        },
        {
          "id": "exampleGrandChildPreference2",
          "label": "Preference Label",
          "type": "boolean",
          "default": true
        }
      ]
    }
  ]
}
```

### Notable UI Behaviors

#### Visual Ordering of Preferences

The order in which you define preferences in the configuration file determines the order in which the UI renders them. You control the visual layout of preferences by ordering them as desired in the configuration. This ordering is especially important for parent preferences, as their child preferences appear directly under them, creating a clear visual grouping effect.

#### Identifying Preferences (Title, etc.)

You can identify what preferences a user is viewing in three locations:

- Configuration Level — Use the `title` and `subtitle` fields to identify the preferences a user is viewing. The UI renders the title in a larger font at the top of the page and the subtitle directly under it in a smaller font. The title conventionally uses a capitalized version of the namespace, so if the namespace is `exampleNamespace`, the title is "Example Namespace". Use the subtitle to describe the preferences and their context.

- Group Level — Use the `title` field to group preferences under a common heading. This approach visually separates preferences into sections and clarifies what each section relates to. The UI renders the group title in a font size between the configuration title and the preference label, making it clear that it is a grouping element.

- Preferences Level — Use the `label` field to identify the preferences itself. This is the most granular level of identification and describes the purpose of the preferences. The UI renders the label directly next to the UI component for the preferences (such as a toggle or radio button). Write labels in clear, human-readable language. For example, use "This is my Preference Choice" rather than "this-is-my-preference-choice" or "another_preference".
