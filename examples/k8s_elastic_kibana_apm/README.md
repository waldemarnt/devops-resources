# Setup Example of Elastic stack on Kubernetes

This example include all the necessary configuration to run Elasticsearch + Kibana + Elastic APM server and a Node.js app on Kubernetes.
It was tested on Minikube so some configurations might change to run in a Cloud provider but it's expected that these resources will cover
the most common cases.

## How to run

1. Apply/Create the kubernetes configuration:
```
$ kubectl create -f elastic/apm-server.yaml
```
```
$ kubectl create -f elastic/elasticsearch.yaml
```
```
$ kubectl create -f elastic/kibana.yaml
```

2. You will have to add the proper APM for your application, this example includes a yaml for a Node.js APP.
It expects a docker image called `node-app` built on the VMs host [if you don't know how to build on the VM's host have a read here](https://medium.com/@brianbmathews/getting-started-with-minikube-docker-container-images-for-testing-kubernetes-locally-on-mac-e39adb60bd41)

Your application have to be writing metrics using the [Node.js Agent](https://www.elastic.co/guide/en/apm/agent/nodejs/index.html), when configuring the agent make sure that the `serverUrl` is `http://apm-server:8200` which is the APM server url on the cluster.

Then once the agent is configured in your app and you've built the Docker image for the app, apply the config:

```
$ kubectl create -f node/node.yaml
```

*Note* For non Node.js apps the logic is the same, just add the proper elastic agent, point the the APM server and deploy the app on the cluster.
