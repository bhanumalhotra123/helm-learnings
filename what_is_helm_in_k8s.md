Helm is a package manager for Kubernetes, allowing users to easily manage, share, and deploy applications on Kubernetes clusters using pre-configured packages called charts. 
It simplifies the process of managing complex applications by providing versioning, templating, and release management capabilities.


#### Helm 2 Architecture  

![helm2](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/cffa22bb-40c1-4aa5-9c17-54b56bc80506)

#### Helm 3 Architecture  
Helm client interacts directly with kubernetes api-server

![helm3](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/f3d99093-77e4-4745-b8b0-c1bb642253fc)

#### Why we need helm charts?

![helm-need](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/c0ff34ca-fa37-407d-8301-d2b562c71ef4)

- Standardise and Reusable
- Reduces deployment complexity
- Improves developer productivity
- The helm install command is used to deploy a Helm chart onto a Kubernetes cluster.It takes a Helm chart package (either a .tgz file or a chart reference) and installs it onto the cluster, generating Kubernetes resources based on the templates provided within the chart.
- In Helm, the _helpers.tpl file is a special template file used to define reusable template helpers. These helpers are pieces of Go template code that can be used across multiple templates within a Helm chart.

__Example_Chart_directory__
```

mychart/
  Chart.yaml         # Metadata about the chart
  values.yaml        # Default configuration values
  charts/            # Directory containing dependencies (sub-charts)
  templates/         # Directory containing Kubernetes manifests
    deployment.yaml  # Example Kubernetes Deployment manifest
    service.yaml     # Example Kubernetes Service manifest
  charts/            # Directory containing dependencies (sub-charts)
  _helpers.tpl       # Optional: Template helpers
  ...

```

Helm Workflow

![helm_workflow](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/b83458e6-2e10-411b-84c5-4f57aa121c0c)
