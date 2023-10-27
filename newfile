# Release

## Release Cadence

kaas-external-secrets is released on a weekly basis by the Orch team, every Tuesday.

The release will contain any upstream updates, which are compatible with our EKS version along with rebuilt images.

These releases go through the normal CCB process.


## Release process
### 1. Update tags
Run

```sh
make update-tags
```

What for CI to run and then merge

### 2. [Cut a tag](https://github.com/acquia/kaas-external-secrets/releases/new) and set `targetRevision` for variants to it in
[.acquia/platform.yaml](.acquia/platform.yaml)

### 3. Deploy with acd and sync children

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

## 4. Verify release
Check [Dashboards](https://service.sumologic.com/ui/#/library/folder/28283158)

[acd-dev]: https://argocd.dev.cloudservices.acquia.io/applications/kaas-external-secrets-dev
[acd-staging]: https://argocd.cloudservices.acquia.io/applications/kaas-external-secrets-staging
[acd-prod]: https://argocd.cloudservices.acquia.io/applications/kaas-external-secrets-prod

Make sure external-secrets, external-secrets-cert-controller, external-secrets-webhook pods are running
```sh
k get all -n external-secret
```

Check version of container image
```sh
k describe deployment.apps/external-secret -n external-secret
```
