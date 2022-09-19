Problem of Scalability.

What if no of Yaml files increased?

- kustomization.yaml will become more complex and tedious to manage.

one kustomization.yaml will not be effective in this scenario.

we have to add kustomization.yaml in each subdirectories.
like apache, hello-world, nginx etc and 

The Structure will look like this:
`
.
└── k8s-manifests
    ├── Example2-Readme.md
    ├── apache
    │   ├── apache-depl.yaml
    │   ├── apache-svc.yaml
    │   └── kustomization.yaml
    ├── hello-world
    │   ├── hw-dep.yaml
    │   ├── hw-svc.yaml
    │   └── kustomization.yaml
    ├── kustomization.yaml
    └── nginx
        ├── kustomization.yaml
        ├── nginx-dep.yaml
        └── nginx-svc.yaml
`

Apply the yaml file by using this commands.

` kubectl apply -k . `

OR alternatively we can use

` kustomize build . | kubectl apply -f - `