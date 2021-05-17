# Home Assistant Custom Components

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs) [![made-with-python](https://img.shields.io/badge/Made%20with-Python-1f425f.svg)](https://www.python.org/)
[![Discord](https://img.shields.io/discord/330944238910963714?label=discord)](https://discord.gg/b25Yyh24)

Custom Components for [Home Assistant](http://www.home-assistant.io).

## Microsoft Graph API Component

Component to interface with Microsoft's Graph API on your organization's tenant.
Currently only detects the authorized user's Microsoft Teams availability and activity but the possibilities are almost endless.

The Microsoft Graph integration allows Home Assistant to create sensors from various properties of your Office 365 tenant via Microsoft's Graph API. All queries are performed using the authenticated user's account and depend on the permissions granted when creating the Azure AD application.

Home Assistant authenticates with Microsoft through OAuth2. Set up your credentials by completing the steps in [Manual Configuration](#manual-configuration) and then add the integration in **Configuration -> Integrations -> Microsoft Graph**. Ensure you log in using a Microsoft account that belongs to your organization's tenant.

- [Home Assistant Custom Components](#home-assistant-custom-components)
  - [Microsoft Graph API Component](#microsoft-graph-api-component)
    - [Installation](#installation)
    - [Manual Configuration](#manual-configuration)
    - [Sensor](#sensor)
      - [Microsoft Teams](#microsoft-teams)

### Installation

- If it doesn't already exist, create a directory called `microsoft_graph` under `config/custom_components/`.
- Copy all files in `custom_components/microsoft_graph` to your `config/custom_components/microsoft_graph/` directory.
- Restart Home Assistant to pick up the new integration.
- Configure with config below.

### Manual Configuration

You will need to register an application in Azure AD and retrieve the client ID and client secret:

- Register a new application in [Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)
  - Name your app
  - Select "Accounts in any organizational directory (Any Azure AD directory - Multitenant)" under supported account types
  - For Redirect URI, add: `https://<EXTERNAL_HOME_ASSISTANT_URL>/auth/external/callback`
- Copy your Application (client) ID for later use
- On the App Page, navigate to "Certificates & secrets"
  - Generate a new client secret and *save for later use* (you will *not* be able to see this again)

Then set the relevant permissions on the application on the API Permissions page. All of the following are required to function correctly:

- Presence.Read
- Presence.Read.All
- User.Read
- User.ReadBasic.All

Add the client id and secret to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
microsoft_graph:
  client_id: YOUR_CLIENT_ID
  client_secret: YOUR_CLIENT_SECRET
```

Finish setup in the UI through **Configuration -> Integrations -> Microsoft Graph**.

### Sensor

The Microsoft Graph sensor platform automatically tracks various resources from your Office 365 tenant.

#### Microsoft Teams

There are 2 sensors that are added, both of which are enabled by default.

| Entity ID | Default | Description                                                                                        |
| ---------------------------------| ------ | -----------------------------------------------------------------------------|
| `sensor.ms_teams_availability` | Enabled  | Shows your availability (e.g. Available, AvailableIdle, Away).               |
| `sensor.ms_teams_activity`     | Enabled  | Shows your activity (e.g. InACall, InAConferenceCall, Inactive, InAMeeting). |

See possible availability and activity values [here](https://docs.microsoft.com/en-us/graph/api/resources/presence?view=graph-rest-beta#properties).
