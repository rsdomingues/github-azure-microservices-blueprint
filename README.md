# winops-london
Repository to hold my demo files for [my talk](https://www.winops.org/london/agenda/bluegreen.php) at WinOps London 2018


## Blue/Green and Canary Deployments with Azure DevOps, Istio and AKS

Room: CTRL

Talk Time: 2:45 - 3:30pm

In this demo-driven talk I will show how you can implement advanced DevOps concepts like blue/green and canary deployment using Azure Pipelines targeting a polyglot application deployed to an Azure Kubernetes Cluster using Helm. Istio is used to shape traffic to different versions of the same microservice giving full control on what your users see and controlling the flow of releases throughout the pipeline.

### Building the app with Azure Container Registry

```bash
export ACR_NAME=theregistry
export VERSION=1.0
az acr build -t microsmack/smackapi:$VERSION -r $ACRNAME ./smackapi
az acr build -t microsmack/smackweb:$VERSION -r $ACRNAME ./smackweb

```

### Install istio

```bash
curl -L https://git.io/getLatestIstio | sh -
cd istio-1.0.3
kubectl apply -f install/kubernetes/helm/istio/templates/crds.yaml
kubectl apply -f install/kubernetes/istio-demo-auth.yaml

kubectl label namespace default istio-injection=enabled

check all is good:
kubectl get pods -n istio-system
kubectl get svc -n istio-system
```

### Install istioctl on mac

```bash
brew tap ams0/istioctl
brew install istioctl
```

### Add a DNS entry for `istio-ingressgateway`

```bash
kubectl get svc istio-ingressgateway -n istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
az network dns record-set a add-record -g dns -z cookingwithazure.com -n *.mesh --value <IP>
```




### Useful links

[Smackapp source](https://github.com/chzbrgr71/microsmackv2)