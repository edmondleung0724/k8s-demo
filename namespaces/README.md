**Create namespace**    

[root@minikube01 namespaces]# kubectl create -f hellospace.yaml  
namespace/hellospace created  
resourcequota/compute-quotas created  
resourcequota/object-quotas created  

[root@minikube01 namespaces]# kubectl get namespaces  
NAME                   STATUS   AGE  
default                Active   4d7h  
hellospace             Active   9s  
kube-node-lease        Active   4d7h  
kube-public            Active   4d7h  
kube-system            Active   4d7h  
kubernetes-dashboard   Active   4d6h  

**Check resource quotas**    

[root@minikube01 namespaces]# kubectl get resourcequotas -n hellospace  
NAME             CREATED AT  
compute-quotas   2020-02-29T12:04:44Z  
object-quotas    2020-02-29T12:04:44Z  

[root@minikube01 namespaces]# kubectl describe resourcequotas compute-quotas -n hellospace  
Name:            compute-quotas  
Namespace:       hellospace  
Resource         Used  Hard  
--------         ----  ----  
limits.cpu       0     1  
requests.cpu     0     1  
requests.memory  0     10Gi  

[root@minikube01 namespaces]# kubectl describe resourcequotas object-quotas -n hellospace  
Name:                   object-quotas  
Namespace:              hellospace  
Resource                Used  Hard  
--------                ----  ----    
configmaps              0     1  
replicationcontrollers  0     10  
secrets                 1     1  
services                0     2  
services.loadbalancers  0     1  

**Try to create secret**    

[root@minikube01 namespaces]# kubectl create secret generic mysecret --from-literal=aGVsbG93b3JsZA== -n hellospace  
Error from server (Forbidden): secrets "mysecret" is forbidden: exceeded quota: object-quotas, requested: secrets=1, used: secrets=1, limited: secrets=1
