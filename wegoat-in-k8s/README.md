
Start minikube
```
minikube start
minikube start --kubernetes-version 1.12.1
$ minikube start --kubernetes-version v1.12.0
Starting local Kubernetes v1.12.0 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Downloading kubelet v1.12.0
Downloading kubeadm v1.12.0
Finished Downloading kubeadm v1.12.0
Finished Downloading kubelet v1.12.0
Setting up certs...
Connecting to cluster...
Setting up kubeconfig...
Starting cluster components...
Kubectl is now configured to use the cluster.
Loading cached images from config file.
```

Check minikube status
```
minikube status
```

Enable ingress add-on
```
minikube addons enable ingress
```

#Fresh minikube install (MacOS)
```
minikube delete
brew cask uninstall minikube
rm -rf /usr/local/bin/minikube
rm -rf ~/.minikube 
brew cask install minikube
```

#Deploy webgoat (any k8s cluster)
```
kubectl create -f deployment_wgoat.yaml 
kubectl create -f svc-wgoat.yaml 
kubectl create -f ingress-wgoat.yaml 
kubectl get ingress,svc --all-namespaces
```
#Access WebGoat
```
minikube ip
http://192.168.99.100/WebGoat/
```

#References
https://kubernetes.io/docs/setup/minikube/
