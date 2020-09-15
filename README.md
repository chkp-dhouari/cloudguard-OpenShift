# Onboarding an Openshift cluster to Check Point Cloudguard CSPM (ex.Dome9)


Please go to the CloudGuard asset onboarding page in cloudguard the pick Kubernetes. enter the cluster name, namespace name and your cloudguard API token then click next pick an organization then next the STOP Enter the command below in your openshift cluster:

install git
https://git-scm.com/

```
git clone git@github.com:chkp-dhouari/cloudguard-openshift-.git
```

You should have the deployment yaml file named "cp-cloudguard-openshift.yaml" in your dir

> IMPORTANT: Please do not use the Helm chart or manual option kubectl comnands as this is for OpenShift which uses oc command to manasge the K8s cluster. Openshift implementation has different values with regards to deployments config paramaters vs traditional K8s. The deployment file and some other commands had to be customized for OpenShift

### Run the following command (only for new namespace)

```
oc create namespace
```

## PLEASE REPLACE with your the namespace name you created!

### Create a CloudGuard token or use an existing one and add to your cluster secrets

```
oc create secret generic dome9-creds --from-literal=username=1b089072-78c1-4d5a-b365-456a9a5f16b2 --from-literal=secret=ustwhdqj4h956e577kr9ewy8 --namespace
```

### Create a configmap to hold the clusterID

```
oc create configmap cp-resource-management-configmap --from-literal=cluster.id=fc6dfe53-a302-4ca7-9053-276511570e6a --namespace
```

### Run the following commands

```
oc create serviceaccount cp-resource-management --namespace
```

```
oc create clusterrole cp-resource-management --verb=get,list --resource=pods,nodes,services,nodes/proxy,networkpolicies.networking.k8s.io,ingresses.extensions,podsecuritypolicies,roles,rolebindings,clusterroles,clusterrolebindings,serviceaccounts,namespaces
```
```
oc create clusterrolebinding cp-resource-management --clusterrole=cp-resource-management --serviceaccount=prod:cp-resource-management
```



### Deploy CloudGuard agent

```
oc create -f cp-cloudguard-openshift.yaml
```

then next and wait for agent to be synced

Start running compliance on your OC cluster
