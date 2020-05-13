This Readme will help to build the Tekton pipeline

https://itnext.io/explore-different-methods-to-build-and-push-image-to-private-registry-with-tekton-pipelines-5cad9dec1ddc

### Create Secret

```shell
kubectl create secret docker-registry regcred --docker-server=quay.io/nicolaso/frontend --docker-username=XXXXXXX --docker-password=XXXXXXX --docker-email=XXXXXX
```

### Edit the Service Account pipleine (default) and add the accoiunt
```shell
oc edit sa pipeline

secrets:
- name: pipeline-token-6zj9m
- name: pipeline-dockercfg-jtsx5
- name: regcred


```

### ServiceAccount

If you don't specify a service account to be used for running the TaskRun or PipelineRun, the default service account. OpenShift by default does not allow the default service account to modify objects in the namespace. Therefore you should either explicitly grant permission to the default service account (by creating rolebindings) or create a new service account with sufficient privileges and specify it on the TaskRun or PipelineRun.

You can do the former via oc and running the following command, replacing <namespace> with your target namespace:


```shell
oc policy add-role-to-user edit -z default -n socks-shop
```
### Troubeshoot

tkn pipeline start build-and-deploy
tkn pipelinerun logs build-and-deploy-run-z2rz8 -f -n pipelines-tutorial

```shell
tkn pipeline ls
tkn resource ls
tkn pipeline logs -f
tkn pipeline start [PIPELINENAME] -n [Project] -s [SERVICEACCOUNT]

```

If you want to re-run the pipeline again, you can use the following short-hand command to rerun the last pipelinerun again that uses the same pipeline resources and service account used in the previous pipeline run:

```shell
tkn pipeline start build-and-deploy --last
```


```shell
oc adm policy add-cluster-role-to-user cluster-admin -z socks-shop
```
### Socks shop pods also need to run as priviliaged containers, so grant 'priviliged' Security Context Constrains (SCC) for 'socks-shop' service account

```shell
oc adm policy add-scc-to-user privileged -z socks-shop
```
### Socks app has an init daemon that has to run as UID 0, so grant 'anyuid' SCC for 'default' service account

```shell
oc adm policy add-scc-to-user anyuid -z default
oc adm policy add-scc-to-user anyuid -z build-registry
oc adm policy add-role-to-user edit system:serviceaccount:socks-shop:build-registry
```
### Deploy tkn pipeline
to be validate 

```shell
oc create -f https://raw.githubusercontent.com/NicolasO/microservices-demo/master/deploy/tekton-pipeline/TektonRunScript.yaml
```

### Deploy tkn for Production 

```shell
oc create -f https://raw.githubusercontent.com/NicolasO/microservices-demo/master/deploy/tekton-pipeline/Tekton-Deploy-only.yaml
```

### Expose the Front end Service :

```shell
oc expose service front-end
```


### to delete application in OpenShift using oc tools

```shell
oc delete -f complete-demo.yaml
```
OR
```shell
oc delete -f https://raw.githubusercontent.com/NicolasO/microservices-demo/master/deploy/openshift/complete-demo.yaml
```
