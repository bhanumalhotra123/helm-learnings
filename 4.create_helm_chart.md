Run the Helm Create Command: 

```
helm create mychart
```

This command generates the basic structure for a Helm chart within a directory named mychart.

```
mychart/
├── .helmignore
├── Chart.yaml
├── charts
├── templates              
│   ├── NOTES.txt         ---------> this will be shown as output when you run helm related commands
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml

```



Customize Your Chart: You can now customize your Helm chart according to your application's requirements. This includes modifying Chart.yaml, values.yaml, and the template files in the templates/ directory.

Package Your Chart: Once you've customized your chart, you can package it using the Helm package command:

```
helm package mychart/
```

Install or Share Your Chart: You can install the packaged chart using helm install, or share the packaged chart with others for installation on their Kubernetes clusters.
  
  
  
  
  

The test-connection.yaml file is typically included within the templates/tests directory of a Helm chart. This file is used to define a Kubernetes Job resource that serves as a test to verify connectivity or functionality within the deployed application.

Here's an example of what a test-connection.yaml file might contain:

```
apiVersion: batch/v1
kind: Job
metadata:
  name: {{ include "mychart.fullname" . }}-test
  labels:
    {{- include "mychart.labels" . | nindent 4 }}
spec:
  template:
    metadata:
      labels:
        {{- include "mychart.selectorLabels" . | nindent 8 }}
    spec:
      containers:
        - name: test-connection
          image: busybox:latest
          command: ['sh', '-c', 'echo "Testing connection"; sleep 5']
      restartPolicy: Never

```

- It defines a Kubernetes Job named {{ include "mychart.fullname" . }}-test. The {{ include "mychart.fullname" . }} is a Helm template function that generates the full name of the chart and appends -test to it.
- The Job contains a single container named test-connection, which uses the busybox:latest image.
- The container's command is set to ['sh', '-c', 'echo "Testing connection"; sleep 5'], which simply echoes "Testing connection" and then waits for 5 seconds.
- The Job has a restart policy of Never, meaning that the Job won't be restarted if it fails.


Now _helpers.tpl file consist of functions
for example all the files of a particular app may have same labes, it would be kind of repetation and more over while changing you will have to make changes to each file.


helm lint  ./mychart/    #command will be usefult to test if there are any mistakes in our charts

helm install --dry-run --debug ./mychart/



- helm install: This is the main command for installing a Helm chart.
- --dry-run: This flag tells Helm to simulate the installation process without actually deploying anything to the cluster. It generates the Kubernetes manifest files that would be applied during a real installation, but it doesn't apply them.
- --debug: This flag tells Helm to print out debug information during the installation process. It provides detailed output, including the generated Kubernetes manifest files and any error messages encountered.

this may give 


'Error: must either provide a name or specify --generate-name' in Helm"


A Release is an instance of a chart running in a Kubernetes cluster.


now say you are actually installing helm charts on your cluster

but assume your service.yaml have clusterip defined but not the nodeport

helm install example ./mychart/ --set service.type=NodePort

when you run this your Notes.txt will be displayed


helm list will give you list of releases and you should find one named example

also with kubectl get pods you will be able to see all the objects in your templates created



helm uninstall example 

now everything related to this will be deleted.
