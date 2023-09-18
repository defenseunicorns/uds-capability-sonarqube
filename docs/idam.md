# IDAM integration with uds-capability-sonarqube

The sonarqube capability is preconfigured to work with an IDAM solution, but IDAM is disabled by default.

## IDAM Variables

These Zarf variables are mapped to sonarqube helm chart values that are documented [here.](https://docs.sonarsource.com/sonarqube/latest/instance-administration/authentication/saml/overview/#settings)

| Key                | Type   | Default | Description                                   |
|--------------------|--------|---------|-----------------------------------------------|
| IDAM_ENABLED       | bool   | `false` | Enables/disables IDAM                         |
| IDAM_CLIENT_ID     | string | `""`    | The ID of the client used to auth             |
| IDAM_PROVIDER_NAME | string | `""`    | Name of the identity provider                 |
| IDAM_REALM_URL     | string | `""`    | The URL for the realm used for auth           |
| IDAM_SAML_CERT     | string | `""`    | The SAML certificate from keycloak            |
| IDAM_ATTR_LOGIN    | string | `""`    | The IDAM attribute to map login to            |
| IDAM_ATTR_NAME     | string | `""`    | The IDAM attribute to map name to             |
| IDAM_ATTR_EMAIL    | string | `""`    | The IDAM attribute to map email to            |
| IDAM_ATTR_GROUP    | string | `""`    | The IDAM attribute to map groups to, optional |
