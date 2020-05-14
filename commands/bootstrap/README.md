# Odo Pipelines Bootstrap Command

Bootstrap command creates default environments for your initial application.  It outputs resources yaml files, kustomization files, and Manifest.  The following resources are written to filesystem.
   
* CI/CD environment with pipelines and resources
* ArgoCD environment
* Dev environment with an application/service
* Stage environment

```shell
$ odo pipelines bootstrap 
  --app-repo-url 
  --app-webhook-secret 
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
| --app-repo | The source repository of your initial application.   E.g. https://github.com/user/service.git |
| --app-webhook-secret | A secret used to validate incoming events from source repository webhook. |
| --gitops-repo-url | The Git repository where your configuration and manifest live. E.g. https://github.com/user/gitops.git|
| --gitops-webhook-secret | A secret used to validate incoming events from GitOps webhook. |
| --image-repo | Where should we configure your builds to push to? E.g. quay.io/user/service or user/service for internal registry|
| --dockercfgjson | Path to dockercfgjson file.  This is used to authenticate image pushes to your image-repo. |
| --internal-registry-hostname | Optional. Internal image registry hostname (default _image-registry.openshift-image-registry.svc:5000_)
| --prefix                | Optional.  This is used to help separate user namespaces. |
| --output                | Optional.  Output path.  (default is the current working directory|

The following [directory layout](output) is generated.

```shell
.
├── environments
│   ├── argocd
│   │   └── config
│   │       ├── dev-service-app.yaml
│   │       └── kustomization.yaml
│   ├── cicd
│   │   ├── base
│   │   │   ├── kustomization.yaml
│   │   │   └── pipelines
│   │   │       ├── 01-namespaces
│   │   │       │   └── cicd-environment.yaml
│   │   │       ├── 02-rolebindings
│   │   │       │   ├── pipeline-service-account.yaml
│   │   │       │   ├── pipeline-service-rolebinding.yaml
│   │   │       │   └── pipeline-service-role.yaml
│   │   │       ├── 03-secrets
│   │   │       │   ├── docker-config.yaml
│   │   │       │   ├── github-webhook-secret-service-svc.yaml
│   │   │       │   └── gitops-webhook-secret.yaml
│   │   │       ├── 04-tasks
│   │   │       │   ├── deploy-from-source-task.yaml
│   │   │       │   └── deploy-using-kubectl-task.yaml
│   │   │       ├── 05-pipelines
│   │   │       │   ├── app-ci-pipeline.yaml
│   │   │       │   ├── cd-deploy-from-push-pipeline.yaml
│   │   │       │   └── ci-dryrun-from-pr-pipeline.yaml
│   │   │       ├── 06-bindings
│   │   │       │   ├── github-pr-binding.yaml
│   │   │       │   └── github-push-binding.yaml
│   │   │       ├── 07-templates
│   │   │       │   ├── app-ci-build-pr-template.yaml
│   │   │       │   ├── cd-deploy-from-push-template.yaml
│   │   │       │   └── ci-dryrun-from-pr-template.yaml
│   │   │       ├── 08-eventlisteners
│   │   │       │   └── cicd-event-listener.yaml
│   │   │       ├── 09-routes
│   │   │       │   └── gitops-webhook-event-listener.yaml
│   │   │       └── kustomization.yaml
│   │   └── overlays
│   │       └── kustomization.yaml
│   ├── dev
│   │   ├── apps
│   │   │   └── service
│   │   │       ├── base
│   │   │       │   └── kustomization.yaml
│   │   │       ├── kustomization.yaml
│   │   │       └── overlays
│   │   │           └── kustomization.yaml
│   │   ├── env
│   │   │   ├── base
│   │   │   │   ├── dev-environment.yaml
│   │   │   │   ├── dev-rolebinding.yaml
│   │   │   │   └── kustomization.yaml
│   │   │   └── overlays
│   │   │       └── kustomization.yaml
│   │   └── services
│   │       └── service-svc
│   │           ├── base
│   │           │   ├── config
│   │           │   │   ├── 100-deployment.yaml
│   │           │   │   ├── 200-service.yaml
│   │           │   │   └── kustomization.yaml
│   │           │   └── kustomization.yaml
│   │           ├── kustomization.yaml
│   │           └── overlays
│   │               └── kustomization.yaml
│   └── stage
│       └── env
│           ├── base
│           │   ├── kustomization.yaml
│           │   └── stage-environment.yaml
│           └── overlays
│               └── kustomization.yaml
└── pipelines.yaml
```

