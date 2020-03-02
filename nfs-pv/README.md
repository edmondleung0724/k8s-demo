**Create pv**    
[root@minikube01 nfs-pv]# kubectl create -f nfs-pv.yaml  
persistentvolume/nfs-pv created

[root@minikube01 nfs-pv]# kubectl get pv  
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE  
nfs-pv   100Mi      RWX            Retain           Available                                   3s  

**Create pvc**    
[root@minikube01 nfs-pv]# kubectl create -f nfs-pvc.yaml  
persistentvolumeclaim/nfs-pvc created

[root@minikube01 nfs-pv]# kubectl get pv  
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM             STORAGECLASS   REASON   AGE  
nfs-pv   100Mi      RWX            Retain           Bound    default/nfs-pvc                           20s  

[root@minikube01 nfs-pv]# kubectl get pvc  
NAME      STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE  
nfs-pvc   Bound    nfs-pv   100Mi      RWX                           12s  

**Create pods**  
[root@minikube01 nfs-pv]# kubectl create -f my-pod-1-with-pv.yaml  
pod/my-pod-1 created

[root@minikube01 nfs-pv]# kubectl create -f my-pod-2-with-pv.yaml  
pod/my-pod-2 created

[root@minikube01 nfs-pv]# kubectl get pod  
NAME       READY   STATUS    RESTARTS   AGE  
my-pod-1   1/1     Running   0          11s  
my-pod-2   1/1     Running   0          4s  

**Create file in pod**  
[root@minikube01 nfs-pv]# kubectl exec -it my-pod-1 -- /bin/bash  
root@my-pod-1:/app# ls  
Dockerfile  index.js  node_modules  package.json  
root@my-pod-1:/app# df -h  
Filesystem               Size  Used Avail Use% Mounted on  
overlay                   80G  6.2G   74G   8% /  
tmpfs                     64M     0   64M   0% /dev  
tmpfs                    3.9G     0  3.9G   0% /sys/fs/cgroup  
192.168.134.119:/data     80G  5.3G   75G   7% /demo-nfs/pv-test  
/dev/mapper/centos-root   80G  6.2G   74G   8% /etc/hosts  
shm                       64M     0   64M   0% /dev/shm  
tmpfs                    3.9G   12K  3.9G   1% /run/secrets/kubernetes.io/serviceaccount  
root@my-pod-1:/app# echo "hello world" >> /demo-nfs/pv-test/my-pod-1.txt  
root@my-pod-1:/app# cat /demo-nfs/pv-test/my-pod-1.txt  
hello world  
root@my-pod-1:/app# exit  
exit  

**Delete pod**  
[root@minikube01 nfs-pv]# kubectl delete pod my-pod-1  
pod "my-pod-1" deleted  

**Create pod again**  
[root@minikube01 nfs-pv]# kubectl create -f my-pod-1-with-pv.yaml  
pod/my-pod-1 created  

[root@minikube01 nfs-pv]# kubectl get pod  
NAME       READY   STATUS    RESTARTS   AGE  
my-pod-1   1/1     Running   0          11s  
my-pod-2   1/1     Running   0          3m18s  

**File still exists in pod**  
[root@minikube01 nfs-pv]# kubectl exec -it my-pod-1 -- /bin/bash  
root@my-pod-1:/app# cat /demo-nfs/pv-test/my-pod-1.txt  
hello world  

**Create a.test file in pod-1**  

[root@minikube01 ~]# kubectl exec -it my-pod-1 -- /bin/bash
root@my-pod-1:/app# for i in `seq 1 1000`; do echo $i >> /demo-nfs/pv-test/a.test; sleep 1; done

**Edit the same file in pod-2**  

[root@minikube01 nfs-pv]# kubectl exec -it my-pod-2 -- /bin/bash
root@my-pod-2:/app#  for i in `seq 2001 9000`; do echo $i >> /demo-nfs/pv-test/a.test; sleep 1; done

**Check the file content from the nfs server**  
[root@nfs01 data]# tail -10 a.test  
57  
2024  
58  
2025  
59  
2026  
60  
