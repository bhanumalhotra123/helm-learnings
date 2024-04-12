It is a bad idea to keep files for each environment say for dev, qa, prod

It is better to keep one set of templates and change the values based on different environments they have to be installed to, say number of replicas may differ or maybe the resources.

```
helm create ui
```

```
helm template dev-dep -f .\ui\values.yaml .\ui\ > dev.yaml
```
 
```
helm template qa-dep -f .\ui\values.yaml .\ui\ > qa.yaml
```