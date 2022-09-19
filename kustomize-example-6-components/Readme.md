Components provides the ability to define reusable pieces of configuration logic (resources + patches) that can be included in multiple overlays.

Components are useful in situations where applications support multiple optional features that need to be enabled only in a subset of overlays.

Lets say our applications is divided into 3 variations

- dev
- premium 
- self hosted

Lets say only premium and self hosted wants caching enabled. and dev and premium wants external db to be connected.

we create a seperate components -> caching and db and import this components in the overlays.

```

k8s-manifests/
- base/
   - kustomization.yaml
   - api-depl.yaml
- components/
   - caching/
     - kustomization.yaml
     - dep-patch.yaml
     - redis-depl.yaml
   - db/
     - kustomization.yaml
     - dep-patch.yaml
     - postgres-depl.yaml
- overlays/
   - dev/
     - kustomization.yaml
   - premium/
     - kustomization.yaml
   - standalone/
     - kustomization.yaml
```

in kustomization.yaml -> add
```
apiversion:
kustomize.config.k8s.io/v1alpha1
kind: Component
resources:
  - postgres-depl.yaml
patches:
  - dep-patch.yaml

```

> cd premium 
Add components section in overlays
```
components:
  - ../../components/db

```

Clean up the resource with

" kubectl delete -k . "