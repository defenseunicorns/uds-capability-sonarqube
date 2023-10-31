# uds-capability-sonarqube

Platform One Sonarqube deployed via flux

## Pre-req

- Minimum compute requirements for single node deployment are at LEAST 64 GB RAM and 32 virtual CPU threads (aws `m6i.8xlarge` instance type should do)
- k3d installed on machine

## Deploy

### Use zarf to login to the needed registries i.e. registry1.dso.mil and ghcr.io

```bash
# Download Zarf
make build/zarf

# Login to the registry
set +o history

# registry1.dso.mil (To access registry1 images needed during build time)
export REGISTRY1_USERNAME="YOUR-USERNAME-HERE"
export REGISTRY1_TOKEN="YOUR-TOKEN-HERE"
echo $REGISTRY1_TOKEN | build/zarf tools registry login registry1.dso.mil --username $REGISTRY1_USERNAME --password-stdin

# ghcr.io (To access oci packages needed)
export GH_USERNAME="YOUR-USERNAME-HERE"
export GH_TOKEN="YOUR-TOKEN-HERE"
echo $GH_TOKEN | build/zarf tools registry login ghcr.io --username $GH_USERNAME --password-stdin

set -o history
```

### Deploy Everything

```bash
# This will destroy and create a compatible k3d cluster then it will run make build/all and make deploy/all. Follow the breadcrumbs in the Makefile to see what and how its doing it.
make cluster/full
```

## Import Zarf Skeleton

Below is an example of how to import this projects zarf skeleton into your zarf.yaml. The [uds-package-sofware-factory](https://github.com/defenseunicorns/uds-package-software-factory.git) does this with a subset of the uds-capability projects.

```yaml
components:
  - name: values
    required: true
    files:
      - source: <path-to-the-values-you-want-to-use>
        target: values-sonarqube.yaml
  - name: sonarqube
    required: true
    import:
      name: sonarqube
      url: oci://ghcr.io/defenseunicorns/uds-capability/sonarqube:0.0.4-skeleton
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
