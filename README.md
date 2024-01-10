# uds-package-sonarqube

Sonarqube deployed via UDS zarf package

## Deployment Prerequisites

### SonarQube Capability

The SonarQube Capability expects the database listed below to exist in the cluster before being deployed.

#### General

- Create `sonarqube` namespace
- Label `sonarqube` namespace with `istio-injection: enabled`

#### Database

- A Postgres database is running on port `5432` and accessible to the cluster
- This database can be logged into via the username configured with the zarf var `SONARQUBE_DB_USERNAME`. Default is `sonarqube`
- This database instance has a psql database created and configured with zarf var `SONARQUBE_DB_NAME`. Default is `sonarqubedb`
- The user has read/write access to above mentioned database
- Create `sonarqube-postgres` service in `sonarqube` namespace that points to the psql database
- Create `sonarqube-postgres` secret in `sonarqube` namespace with the key `password` that contains the password to the user for the psql database

## Building

### Use zarf to login to the needed registries i.e. registry1.dso.mil and ghcr.io

```bash
# Login to the registry
set +o history

# registry1.dso.mil (To access registry1 images needed during build time)
export REGISTRY1_USERNAME="YOUR-USERNAME-HERE"
export REGISTRY1_TOKEN="YOUR-TOKEN-HERE"
echo $REGISTRY1_TOKEN | build/zarf tools registry login registry1.dso.mil --username $REGISTRY1_USERNAME --password-stdin


# ghcr.io (If you need to push to GHCR)
export GH_USERNAME="YOUR-USERNAME-HERE"
export GH_TOKEN="YOUR-TOKEN-HERE"
echo $GH_TOKEN | build/zarf tools registry login ghcr.io --username $GH_USERNAME --password-stdin

set -o history
```

## Documentation

[Identity and Access Management](docs/idam.md)
