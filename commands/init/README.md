# Odo Pipelines Init Command

Init command creates CI/CD environments for you.  Unlike the [bootstrap](bootstrap.md) command, it does not create your first application and other environments.   It outputs resources yaml files, kustomization files, and Manifest to filesystem.

```shell
$ odo pipelines bootstrap 
  --gitops-repo-url
  --gitops-webhook-secret
  --image-repo
  --dockercfgjson 
  [--internal-registry-hostname]
  [--prefix]
  [--output]
```

| Option                  | Description |
| ----------------------- | ----------- |
| --gitops-repo-url | The Git repository where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret | A secret used to validate incoming events from GitOps webhook. |
| --image-repo | Where should we configure your builds to push to? E.g. quay.io/user/service or user/service for internal registry|
| --dockercfgjson | Path to dockercfgjson file..  This is used to authenticate image pushes to your image-repo. |
| --internal-registry-hostname | Optional.  Internal image registry hostname (default _image-registry.openshift-image-registry.svc:5000_)
| --prefix                | Optional.  This is used to help separate user namespaces. |
| --output                | Optional.  Output path.  (default is the current working directory|

The following [directory layout](output) is generated.

```shell
.
├── environments
│   └── [cicd]
│       ├── base
│       │   ├── kustomization.yaml
│       │   └── pipelines
│       │       ├── 01-namespaces
│       │       │   └── cicd-environment.yaml
│       │       ├── 02-rolebindings
│       │       │   ├── pipeline-service-rolebinding.yaml
│       │       │   └── pipeline-service-role.yaml
│       │       ├── 03-secrets
│       │       │   └── gitops-webhook-secret.yaml
│       │       ├── 04-tasks
│       │       │   ├── deploy-from-source-task.yaml
│       │       │   └── deploy-using-kubectl-task.yaml
│       │       ├── 05-pipelines
│       │       │   ├── app-ci-pipeline.yaml
│       │       │   ├── cd-deploy-from-push-pipeline.yaml
│       │       │   └── ci-dryrun-from-pr-pipeline.yaml
│       │       ├── 06-bindings
│       │       │   ├── github-pr-binding.yaml
│       │       │   └── github-push-binding.yaml
│       │       ├── 07-templates
│       │       │   ├── app-ci-build-pr-template.yaml
│       │       │   ├── cd-deploy-from-push-template.yaml
│       │       │   └── ci-dryrun-from-pr-template.yaml
│       │       ├── 08-eventlisteners
│       │       │   └── cicd-event-listener.yaml
│       │       ├── 09-routes
│       │       │   └── gitops-webhook-event-listener.yaml
│       │       └── kustomization.yaml
│       └── overlays
│           └── kustomization.yaml
└── pipelines.yaml

```

