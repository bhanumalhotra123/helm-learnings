
#### Architecture and Tiller:

Helm 2 relies on a server-side component called Tiller to manage releases on Kubernetes clusters. Tiller had elevated privileges, which posed security risks.
Helm 3 removes the need for Tiller entirely, shifting to a client-only architecture. This simplifies installation, reduces security concerns, and makes Helm easier to use in air-gapped or restrictive environments.




#### Helm 2 uses 2 way strategic merge patch

When upgrading, helm2 compares diff b/w proposed chart and previous chart manifest. 
If there are any changes it is applied on k8s cluster

Any changes applied on cluster using say kubectl scale, kubectl edit etc those changes were not considered.
This caused manifest not able to rollback as when using these commands no changes are made to charts

Example -

- helm install very_important_app ./very_important_app
- kubectl scale --replicas=0 deployment/very_important_app          -----> Performed by Employee A
- helm rollback very_important_app                                 -----> Performed by Employee B thinking something went wrong with new deployment

  Now rollback should take it back to the previous state? But there is no change in charts as the change has been brought using kubectl command.  

#### Improved upgrade strategy: 3 way strategic merge patch

  In helm3 it considers previous state of helm charts, present state of helm charts and kubernetes present state.



Assume 

containers 
  - name: server
    image: nginx:2.0.0

Someone updates the deployment manually:

containers 
  - name: server
    image: nginx:2.0.0
  - name: my-inject-sidecar
    image: my-cool-mesh:1.0.0

A devops engineer updates the version for nginx:2.1.0


containers 
  - name: server
    image: nginx:2.1.0




what is the output in the end?

containers 
  - name: server
    image: nginx:2.1.0
  - name: my-inject-sidecar
    image: my-cool-mesh:1.0.0



#### Release names are now scoped to the namespace

![namespaces-helm](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/8e077b39-218f-4eaf-ae03-93fa1788c8fe)

#### Secrets as default storage driver

#### Validating chart values with json schema

Whenever you do helm install, helm upgrade, helm lint, helm template the values are tested against json template

 ![schema-validate](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/0a4c1f15-926a-4c9a-9850-d9d6b4613df9)

 Next is now you can use charts.yaml only for the things you place in requirements.yaml
![requirementsnomore](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/a5a664dd-79ae-4e79-beb2-c99dfd883aea)



#### Name(or --generate-name) is now required on install


#### CLI command renames
helm delete  -> helm uninstall
helm inspect -> helm show
helm fetch -> helm pull


#### Automatically creating namespaces

