# Odo Pipelines Service Command

## Service add

_Service_add_ command adds a new service to an existing environment.  Services are logically grouped by applications.   This command will create an application for the new service if the target application does not exist.  It outputs resources yaml files, kustomization files, and updated Manifest to filesystem.

```shell
$ odo pipelines service add 
    --env-name 
    --app-name 
    --service-name
    [--git-repo-url]
    [--webhook-secret]
    [--manifest]
```

| Option                  | Description |
| ----------------------- | ----------- |
| --env-name | Name of the environment which the service will be added.|
| --app-name | Name of the application which the service will be added.|
| --service-name | Name of the service to be added.  Service name must be unique within an environment. |
| --git-repo-url | Optional.  Source Git repository URL.  It must be unique within GitOps.|
| --webhook-secret | Optional.  Webhook secret of the source Git repository URL. It is required if git-repo-url is provided|
| --manifest | Optional.  Path to manifest file.  Default is _pipelines.yaml_. |


The following [directory layout](output) is generated.

```shell
.
└── environments
    └── new-env
        ├── apps
        │   └── new-app
        │       ├── base
        │       │   └── kustomization.yaml
        │       ├── kustomization.yaml
        │       └── overlays
        │           └── kustomization.yaml
        ├── env
        │   ├── base
        │   │   ├── kustomization.yaml
        │   │   ├── new-env-environment.yaml
        │   │   └── new-env-rolebinding.yaml
        │   └── overlays
        │       └── kustomization.yaml
        └── services
            └── my-service
                ├── base
                │   └── kustomization.yaml
                ├── kustomization.yaml
                └── overlays
                    └── kustomization.yaml
```
  
