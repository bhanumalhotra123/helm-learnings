![helm-commands](https://github.com/bhanumalhotra123/helm-learnings/assets/144083659/bad788d9-6f43-453d-9220-b8a72081a13d)


-  completion

```
  helm completion bash
  source <(helm completion bash)
```

Usage: The helm completion command is used along with a shell type (bash, zsh, fish, etc.) to generate the completion script for that particular shell.
In simple terms, the helm completion command helps you type commands faster and with fewer mistakes when you're using Helm in your terminal. 
It does this by generating a special script that tells your terminal how to automatically finish your commands and options when you press the Tab key. 

- create

```
helm create mychart
```
This command creates a new directory named mychart with the necessary files and directories for your Helm chart.
If you run this twice it will overwrite the older values so be careful !

  
- dependency
  
```
helm dependency build myapp
```
This command fetches dependencies specified in Chart.yaml and updates the Chart.lock file.

In the Chart.yaml file of your Helm chart, you can specify dependencies under the dependencies section. 
Each dependency includes the name of the chart, the repository (optional if it's from a known repository), and the version constraint.

Example Chart.yaml:

```
dependencies:
  - name: appmesh-controller
    version: 0.4.0
    repository: https://github.com/aws/eks-charts

```

The lock file pins the exact versions of dependencies used in the Helm chart. 
When you run helm dependency build, Helm resolves the dependencies specified in Chart.yaml and records the exact version of each dependency in the lock file. 
This ensures that subsequent builds or installations of the chart use the same versions of dependencies unless explicitly updated.

You are inside your chart directory for the following:
```
helm dependency build .
```
This command will fetch the App Mesh Controller dependency specified in your Chart.yaml file and update the Chart.lock file with the exact version and any other dependencies required by the App Mesh Controller.


Fetched charts are saved locally in the charts/ directory alongside your parent Helm chart. 
These charts are then used when packaging or installing your Helm chart to ensure that all dependencies are included.

  
To list the dependencies:
```
helm dep list test
```


Where to look for helm charts?
### https://artifacthub.io/


Now assume you wish to add another dep in Chart.yaml

```
dependencies:
  - name: artifactory
    version: 1.0.0
    repository: https://charts.jfrog.io
```


Now to load this new dep

```
helm dep update test
```

Now if you check your charts folder you will see two dependences

Even for the version change you do the same!

- helm env

```
HELM_BIN: /usr/local/bin/helm
HELM_HOME: /home/user/.helm
KUBECONFIG: /home/user/.kube/config
HELM_PLUGINS: /home/user/.helm/plugins
KUBE_CONTEXT: minikube
```

It typically includes details such as the Helm home directory, Kubernetes configuration file, and Kubernetes context.


- helm get
The helm get command in Helm is used to retrieve information about a deployed release.
Assume you created a named release of your chart
for ex -
```
helm install release_1 test_chart      # this will deploy your objects 
```
```
helm get manifest RELEASE_NAME: Fetches the manifest files associated with a deployed release.
helm get values RELEASE_NAME: Fetches the configuration values associated with a deployed release.
helm get hooks RELEASE_NAME: Fetches the hooks associated with a deployed release.
helm get notes RELEASE_NAME: Fetches any release notes associated with a deployed release.
helm get all RELEASE_NAME: Fetches all information about a deployed release, including manifest files, configuration values, hooks, and notes.
Replace RELEASE_NAME with the name of the release you want to retrieve information for.
```

So, using the -a flag with helm get values would provide you with a comprehensive view of all values associated with the specified release, including both user-defined and system-defined/default values.


- helm history release_1
  shows all the info about release like dates of rollback etc

assume you updated something in the chart and upgraded the release using:
```
helm upgrade release_1 test_chart
```

and now when you show 

```
REVISION    UPDATED                     STATUS          CHART                   APP VERSION     DESCRIPTION
1           Sun Jan  9 15:42:44 2022    superseded      mychart-0.1.0           1.0             Initial install
2           Sun Jan  9 15:48:12 2022    deployed        mychart-0.1.0           1.0             Upgrade to 0.1.0

```
by default it shows 256 in table even if it is higher
helm history release_1 --max 1 (say you only want to see 1)



In Helm, one release is associated with one specific chart version. However, one chart version can indeed be used across multiple releases.

- helm del release_1 release_2
  This will uninstall releases

- --set

Now say in your template of service.yaml we want to change the type of service from cluster ip to nodeport when we install the chart
```
helm install release_3 test_chart --set service.type=NodePort
```

This won't make change in chart

- --no-hooks    if you want to ignore hooks that are set
```
helm install release_4 test_chart --set service.type=NodePort --no-hooks
```

- --generate-name
If you use the following it will give you error to give the release name
```
helm install test_chart 
```
you can use --generate-name here to give the release a random name

- To install a chart from hub directly
```
helm install artifactory-oss --version 2.2.2 --repo https://charts.jfrog.io --generate-name
```

- helm lint
  ```
  helm lint my_chart --strict          fails the check even if there is a warning
  ```

- helm list
  to list all the releases


#### kubectl config set-context --current --namespace=default      
this is to set the namespace in the cluster

#### kubectl config view 
this will tell you in which namespace you are

- helm install my-app test_chart --namespace kube-system
