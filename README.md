# uds-capability-sonarqube

Platform One Sonarqube deployed via flux

## Pre-req

- Minimum compute requirements for single node deployment are at LEAST 64 GB RAM and 32 virtual CPU threads (aws `m6i.8xlarge` instance type should do)
- k3d installed on machine

## Deploy

### Use zarf to login to the needed registries i.e. registry1.dso.mil

```bash
# Download Zarf
make build/zarf

# Login to the registry
set +o history

# registry1.dso.mil (To access registry1 images needed during build time)
export REGISTRY1_USERNAME="YOUR-USERNAME-HERE"
export REGISTRY1_TOKEN="YOUR-TOKEN-HERE"
echo $REGISTRY1_TOKEN | build/zarf tools registry login registry1.dso.mil --username $REGISTRY1_USERNAME --password-stdin

set -o history
```

### Build and Deploy Everything via Makefile and local package

```bash
# This will run make build/all, make cluster/reset, and make deploy/all. Follow the breadcrumbs in the Makefile to see what and how its doing it.
make all
```

## Declare This Package In Your UDS Bundle
Below is an example of how to use this projects zarf package in your UDS Bundle

```yaml
kind: UDSBundle
metadata:
  name: example-bundle
  description: An Example UDS Bundle
  version: 0.0.1
  architecture: amd64

packages:
  # Sonarqube
  - name: sonarqube
    repository: ghcr.io/defenseunicorns/uds-capability/sonarqube
    ref: x.x.x
```

## Prerequisites

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

## Documentation

[Identity and Access Management](docs/idam.md)
