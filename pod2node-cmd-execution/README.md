# Pod to Node Command Execution
Intention here is there mighe me scenarios we might want to run commands on Kubernetes nodes may be for node boot strapping, preparing nodes for specific environments, security config etc. So we use script or command from Pod to execute on Node.

Create namespace and apply DaemonSet config, so Pod is created on each node
```
$ kubectl create ns pd-test
$ kubectl apply -f pod2node-cmd.yaml 
```

## Cluster Nodes
Get list of nodes in the cluster
```
$ kubectl get nodes -o wide
NAME                       STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-nodepool1-36165035-5   Ready    agent   60d   v1.12.8   10.240.0.8    <none>        Ubuntu 16.04.6 LTS   4.15.0-1049-azure   docker://3.0.4
aks-nodepool1-36165035-6   Ready    agent   60d   v1.12.8   10.240.0.10   <none>        Ubuntu 16.04.6 LTS   4.15.0-1055-azure   docker://3.0.4
aks-nodepool1-36165035-7   Ready    agent   60d   v1.12.8   10.240.0.5    <none>        Ubuntu 16.04.6 LTS   4.15.0-1049-azure   docker://3.0.4
```

## DaemonSet & Pod List
Get DaemonSet and running Pod details, we can see Pod is scheduled on each node
```
$ kubectl get ds -n pd-test
NAME           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
pod2node-cmd   3         3         3       3            3           <none>          15m

$ kubectl get po -A -o wide |grep -i pod2node-cmd
NAMESPACE   NAME                      READY   STATUS             RESTARTS   AGE    IP             NODE                       NOMINATED NODE
pd-test     pod2node-cmd-8grxj        1/1     Running            0          110s   10.240.0.5     aks-nodepool1-36165035-7   <none>
pd-test     pod2node-cmd-mwl5d        1/1     Running            0          110s   10.240.0.10    aks-nodepool1-36165035-6   <none>
pd-test     pod2node-cmd-prwzn        1/1     Running            0          110s   10.240.0.8     aks-nodepool1-36165035-5   <none>
```

## Pod Shell
Get shell of the Pod
```
$ kubectl exec -it pod2node-cmd-8grxj -n pd-test -- sh
# bash
root@aks-nodepool1-36165035-7:/# hostname -I
10.240.0.5 172.17.0.1 10.244.0.1
root@aks-nodepool1-36165035-7:/# hostname
aks-nodepool1-36165035-7
root@aks-nodepool1-36165035-7:/# ls -ltr /home
total 0
root@aks-nodepool1-36165035-7:/# pwd
/
root@aks-nodepool1-36165035-7:/#
```

When we add user using `adduser` command from Pod, user is not reflected on the Node/VM, not sure how kube-proxy executes `iptables` command each time when there is change in service or associated backends.
