
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
  

