Kustomize Common Transformers
- namespace
- Prefix and Suffix
- Common Labels and Annotations


Suppose we have a 10 environments which have deployment.yaml file and service.yaml file and we
want to apply some common configuration like labels=prod,or prefix or suffix ,annotations or namespaces.

1. Namespaces and Names
Suppose we want to deploy our resources in particular namespaces then we can use 
Set the Namespace for all Resources within a Project with namespace
------------------------------------------------------------------------------------------
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
namespace: my-namespace
resources:
- deployment.yaml
- service.yaml
-------------------------------------------------------------------------------------------

2.Prefix  and suffix the Names of all Resources within a Project with namePrefix and namesuffix
it helps adding preffix/suffix to names in the defined yaml files.

suffix example
# kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
nameSuffix: -september
resources:
- deployment.yaml
- service.yaml

# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  replicas: 1
  template:
    containers:
      - name: nginx
        image: nginx:latest

After applying we will be getting our deployment name as nginx-september
---------------------------------------------------------------------------------------------------------------
3. Commonlabels and Annotations

Set Labels for all Resources declared within a Project with commonLabels
Set Annotations for all Resources declared within a Project with commonAnnotations

If we want to define a common set of labels or annotations for all the Resource in a project.
for example env=prod, we can set the metadata for all resources in our project.

Add the labels declared in commonLabels to all Resources in the project.

<h3>Important: Once set, commonLabels should not be changed so as not to change the Selectors for Services or Workloads.</h3>

# kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
commonLabels:
  app: sanket
  environment: prod
  release:sept
resources:
- deployment.yaml
- service.yaml

# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: my-k8s-app
    environment: prod
spec:
  selector:
    matchLabels:
        app: my-k8s-app
        environment: prod
  template:
    metadata:
      labels:
        app: my-k8s-app
        environment: prod
    spec:
      containers:
      - name: nginx
        image: nginx

# output
After applying, app:my-k8s-app label was overiden to app:sanket and  release:sept was added in yaml file
-----------------------------------------------------------------------------------------------------------------------
Same like labels , we can use Annotations to apply for our yaml files.

apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
commonAnnotations:
  imageregistry: "https://hub.docker.com/"
resources:
- deployment.yaml
- service.yaml

After applying this our annotations will be added to our yaml files.
