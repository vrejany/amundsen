# Amundsen k8s Helm Charts

## What is this?
This is setup templates for deploying [amundsen](https://github.com/lyft/amundsen) on [k8s (kubernetes)](https://kubernetes.io/), using [helm.](https://helm.sh/) 

## How do I get started?
1. Make sure you have the following command line clients setup:
    - k8s (kubectl)
    - helm
    - tiller
2. Build out a cloud based k8s cluster, such as [amazon eks.](https://aws.amazon.com/eks/)
3. Ensure you can connect to your cluster with cli tools in step 1.

## How do I use this?
You will need a values file to merge with these templates, in order to create the infrastructure. Here is an example:
```
environment: "dev"
provider: aws
dnsZone: teamname.company.com
dockerhubImagePath: amundsendev
searchServiceName: search
searchImageVersion: 1.4.2
metadataServiceName: metadata
metadataImageVersion: 1.1.5
frontEndServiceName: frontend
frontEndImageVersion: 1.1.1
frontEndServicePort: 80
```

You may want to override the default memory usage for Neo4J. In particular, if you're just test-driving a deployment and your node exits with status 137, you should set the usage to smaller values:
```
config:
  dbms:
    heap_initial_size: 2Gi
    heap_max_size: 2Gi
    pagecache_size: 2Gi
```

With this values file, you can then setup amundsen with these commands:
```
helm install templates/helm/neo4j --values impl/helm/dev/values.yaml
helm install templates/helm/elasticsearch --values impl/helm/dev/values.yaml
helm install templates/helm/amundsen --values impl/helm/dev/values.yaml
```

## Other Notes
* For aws setup, you will also need to setup the [external-dns plugin](https://github.com/kubernetes-incubator/external-dns)
* There are exising helm charts for neo4j and elasticsearch. Future versions of amundsen may use them instead. 
