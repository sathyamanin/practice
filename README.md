# Release

## Release Cadence

Every Tuesday a new release of kaas-policy will be deployed to development and production.

This is based on the assumption that the only changes present in the release is configuration changes and the release can be considered a standard release. If argoCD itself is being upgraded there is a possibility that the deployment to production will occur later than Tuesday.

No on demand releases of kaas-policy will be done to either dev or prod, unless it is in response to pageable incident.

## Release process
### 1. Update tags
Run [Bump dependencies kaas-policy](https://core.cloudbees.ais.acquia.io/cloudatlas-jenkins/view/ORCH%20Releases/job/Bump%20dependencies%20kaas-policy/) jenkins job. It will create PR. Wait for CI to run and then merge

### 2. [Cut a tag](https://github.com/acquia/kaas-policy/releases/new) 

### 3. Set targetRevision in `platform.yaml` 
Run [Bump versions kaas-policy](https://core.cloudbees.ais.acquia.io/cloudatlas-jenkins/view/ORCH%20Releases/job/Bump%20version%20kaas-policy/) jenkins job. It will create PR setting version for variant to the tag in [.platform.yaml](.platform.yaml)

### 4. Deploy with acd and sync children

```sh
acd deploy --variant dev --check-health --timeout 15
```

```sh
acd deploy --variant qa --check-health --timeout 15
```

```sh
acd deploy --variant staging --check-health --timeout 15
```

```sh
acd deploy --variant prod --check-health --timeout 15
```

### 5. Verify release

Make sure kyverno, kyverno-cleanup-controller pods are running
```sh
kubectl get all -n kaas-policy
```

Check version of container image
```sh
kubectl describe deployment.apps/kyverno -n kaas-policy
```
### 6. Manin website link
[Website link](https://maninorg.com/manin/)
