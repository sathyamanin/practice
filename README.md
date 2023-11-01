# Release
## Release Cadence
kaas-external-secrets is released on a weekly basis by the Orch team, every Tuesday.

The release will contain any upstream updates, which are compatible with our EKS version along with rebuilt images.

These releases go through the normal CCB process.

## Release process

### 1. Update tags
Run [Bump dependencies kaas-cert-manager](https://core.cloudbees.ais.acquia.io/cloudatlas-jenkins/view/ORCH%20Releases/job/Bump%20dependencies%20kaas-cert-manager/) jenkins job. It will create PR. Wait for CI to run and then merge

### 2. Cut a tag
 https://github.com/acquia/kaas-cert-manager/releases/new
 
### 3. Set version in `platform.yaml`
Run [Bump version kaas-cert-manager](https://core.cloudbees.ais.acquia.io/cloudatlas-jenkins/view/ORCH%20Releases/job/Bump%20version%20kaas-cert-manager/) jenkins job.
It will create PR setting `version` for the variant to the tag in [platform.yaml](../.acquia/platform.yaml)

### 4. Deploy to dev, qa and staging
```sh
acd deploy --variant dev --check-health --timeout 15
```
```sh
acd deploy --variant qa --check-health --timeout 15
```
```sh
acd deploy --variant staging --check-health --timeout 15
```

### 5. File CCB ticket

### 6. Deploy to Prod
```sh
acd deploy --variant prod --check-health --timeout 15
```

## 7. Verify release
Check [Dashboards](https://service.sumologic.com/ui/#/library/folder/28283158)

[kaas-cert-manager-dev][kaas-cert-manager-dev]

[kaas-cert-manager-qa][kaas-cert-manager-qa]

[kaas-cert-manager-staging][kaas-cert-manager-staging]

[kaas-cert-manager-prod][kaas-cert-manager-prod]

Make sure cert-manager, cert-manager-cainjector, cert-manager-webhook pods are running
```sh
k get all -n cert-manager
```

Check version of container image
```sh
k describe deployment.apps/cert-manager-webhook -n cert-manager
```

Verify conversion webhook is not failing
```sh
kubectl get Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders,Challenges --all-namespaces
```
[kaas-cert-manager-dev]: https://argocd.dev.cloudservices.acquia.io/applications/kaas-cert-manager-dev
[kaas-cert-manager-qa]: https://argocd.dev.cloudservices.acquia.io/applications/kaas-cert-manager-qa
[kaas-cert-manager-staging]: https://argocd.dev.cloudservices.acquia.io/applications/kaas-cert-manager-staging
[kaas-cert-manager-prod]: https://argocd.dev.cloudservices.acquia.io/applications/kaas-cert-manager-prod
