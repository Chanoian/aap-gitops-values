# aap-gitops-values

GitOps values repository for deploying Ansible Automation Platform (AAP) on Red Hat OpenShift via ArgoCD.

## Structure

```
└── gitops-example/
    ├── aap-env.yaml          # Layer 1 — resource_requirements for all components
    └── aap-1-dev/
        ├── aap-dev.yaml      # Layer 2 — instance config (name, secrets, replicas, toggles)
        └── namespace/
            ├── values.yaml   # Layer 3 — namespace + idle_aap only
            └── application.yaml  # ArgoCD Application CR
```

## Key schema notes

- Resource key is `resource_requirements` (not `resources`) — matches the aap-gateway-helm chart
- Controller resources are split: `web_resource_requirements`, `task_resource_requirements`, `ee_resource_requirements`
- EDA and Hub resources are per sub-component (e.g. `eda.api.resource_requirements`)
- Component toggle is `disabled: true/false` (not `enabled`) for `controller`, `eda`, `hub`

See [examples/testing-full-stack.yaml](https://github.com/Chanoian/aap-gateway-helm/blob/main/examples/testing-full-stack.yaml) in the chart repo for a full field reference.

## Adding a New Instance

1. Create a new folder (e.g. `aap-2-dev/`)
2. Copy and update `aap-dev.yaml` and `namespace/` files
3. Apply the ArgoCD Application once: `oc apply -f gitops-example/aap-2-dev/namespace/application.yaml`

ArgoCD manages everything from that point on.
