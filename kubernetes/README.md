# Running Prometheus on Kubernetes - OBSOLETE

# Install Helm
```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.11.0-linux-amd64.tar.gz
tar -xzvf helm-v2.11.0-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
kubectl create -f https://raw.githubusercontent.com/wardviaene/kubernetes-course/master/helm/helm-rbac.yaml
helm init --service-account tiller 
```
## Start Prometheus (without storage)
```
helm install --name prometheus --set server.persistentVolume.enabled=false,alertmanager.persistentVolume.enabled=false stable/prometheus
```

## Exposing prometheus port
```
export POD_NAME=$(kubectl get pods --namespace default -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace default port-forward $POD_NAME 9090 &
socat TCP4-LISTEN:9091,fork TCP4:localhost:9090 &
```



# Running Prometheus on Kubernetes - UPDATED / LATEST

## NOTE: RUN THIS FROM ANY K8s MASTER NODE
## STEPS TO INSTALL HELM in K8S & ADD REPO 
```
  wget https://get.helm.sh/helm-v3.2.3-linux-amd64.tar.gz
  
  ls -lrth
  
  tar -xf helm-v3.2.3-linux-amd64.tar.gz
  
  ls -lrth
  
  mv linux-amd64/helm /usr/local/bin/
  
  helm
  
  helm version
  
  helm repo add stable https://charts.helm.sh/stable
```  
 
  
## START PROMETHEUS / ALERT MANAGER / GRAFANA PODS
```
 helm search repo stable/prometheus
 
 helm install --generate-name stable/prometheus-operator --namespace idx --kubeconfig /home/nitinpan/certs/admin.kubeconfig
 
 # helm install --generate-name stable/prometheus-operator --namespace monitoring --kubeconfig /home/nitinpan/certs/admin.kubeconfig
WARNING: This chart is deprecated
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
NAME: prometheus-operator-1612531358
LAST DEPLOYED: Fri Feb  5 17:22:41 2021
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
NOTES:
*******************
*** DEPRECATED ****
*******************
* stable/prometheus-operator chart is deprecated.
* Further development has moved to https://github.com/prometheus-community/helm-charts
* The chart has been renamed kube-prometheus-stack to more clearly reflect
* that it installs the `kube-prometheus` project stack, within which Prometheus
* Operator is only one component.

The Prometheus Operator has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=prometheus-operator-1612531358"

Visit https://github.com/coreos/prometheus-operator for instructions on how
to create & configure Alertmanager and Prometheus instances using the Operator.

 
``` 
 
 
 ## DELETE PROMETHEUS
 
 ```
 helm delete prometheus  --namespace idx --kubeconfig /home/nitinpan/certs/admin.kubeconfig
```



## STEPS TO INSTALL NEW REPO for HELM 
```
https://helm.sh/blog/new-location-stable-incubator-charts/
```


## STEPS TO INSTALL PROMETHEUS / ALERT MANAGER / GRAFANA & EDIT SERVICES TO EXPOSE NODE PORT
```
https://dev.to/ko_kamlesh/install-prometheus-grafana-with-helm-3-on-local-machine-vm-1kgj 

Check Steps and use the NODEPORT. 

Use below credentails for login to Grafana 

username: admin
password: prom-operator

```


## VISIT THIS PAGE TO FURTHER CONFIGURE 
```

https://github.com/coreos/prometheus-operatorhttps://github.com/coreos/prometheus-operator

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

```
