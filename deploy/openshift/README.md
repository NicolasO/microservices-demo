This deployment has been tested on OCP 4.3.

### Create a dedicated project for socks shop then apply policy changes needed to run socks shop:

```shell
oc new-project socks-shop
```
### Socks shop pods need full access to Kubernetes API via 'socks-shop' service account

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
```
### Deploy application in OpenShift using oc tools

```shell
oc create -f complete-demo.yaml
```
OR
```shell
oc create -f https://raw.githubusercontent.com/NicolasO/microservices-demo/master/deploy/openshift/complete-demo.yaml
```



### Deployment of the Front-end Micro Service :
in the privous deploiment the front end service is not yet deployed. you have to deploy it :

First Deploy the Deployment Config

```shell
oc create -f https://raw.githubusercontent.com/NicolasO/front-end/master/deployment/DeploymentConfig-front-end.yaml
```

Then Deploy the Service
```shell
oc create -f https://raw.githubusercontent.com/NicolasO/front-end/master/deployment/Service-front-end.yaml
```



### Expose the Front end Service :

```shell
oc expose service front-end
```
### Process to demo color change

Go and see the portal
Please make your change in https://github.com/NicolasO/front-end/blob/master/public/css/style.blue.css

```shell
#top {
  background: #555555;
  padding: 10px 0;
}
```

by
```shell
#top {
  background: #01a982;
  padding: 10px 0;
}
```

Commit your change

```shell
oc rollout latest dc/front-end
```


### to delete application in OpenShift using oc tools

```shell
oc delete project socks-shop
```
OR
```shell
oc delete -f https://raw.githubusercontent.com/NicolasO/microservices-demo/master/deploy/openshift/complete-demo.yaml
```
