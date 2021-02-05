# Running Prometheus on Kubernetes

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


========================================
========================================
========================================

## STEPS TO INSTALL HELM in K8S
  938  wget https://get.helm.sh/helm-v3.2.3-linux-amd64.tar.gz
  939  ls -lrth
  940  tar -xf helm-v3.2.3-linux-amd64.tar.gz
  941  ls -lrth
  942  mv linux-amd64/helm /usr/local/bin/
  943  helm
  944  helm version
  978  helm repo add stable https://charts.helm.sh/stable
  
  
## START PROMETHEUS 

 981  helm search repo stable/prometheus
 984  helm install --generate-name stable/prometheus-operator --namespace idx --kubeconfig /home/nitinpan/certs/admin.kubeconfig
 
 ## DELETE PROMETHEUS
 
 helm delete prometheus  --namespace idx --kubeconfig /home/nitinpan/certs/admin.kubeconfig




## STEPS TO INSTALL NEW REPO for HELM 
https://helm.sh/blog/new-location-stable-incubator-charts/


## STEPS TO INSTALL PROMETHEUS / ALERT MANAGER / GRAFANA & EDIT SERVICES TO EXPOSE NODE PORT
https://dev.to/ko_kamlesh/install-prometheus-grafana-with-helm-3-on-local-machine-vm-1kgj 

## STEPS TO INSTALL 
