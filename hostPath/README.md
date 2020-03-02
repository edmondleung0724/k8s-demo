# k8s-Volumes (hostPath)

**Create pod**    

[root@minikube01 hostPath]# kubectl create -f hostpath-example.yaml  
pod/apiserver created

[root@minikube01 hostPath]# kubectl get pod  
NAME        READY   STATUS    RESTARTS   AGE  
apiserver   1/1     Running   0          13s

**Enter into the pod**    

[root@minikube01 hostPath]# kubectl exec -it apiserver -- /bin/bash

root@apiserver:/app# echo "Hello World 123" >> /tmp/test.txt  
root@apiserver:/app# cat /tmp/test.txt  
Hello World 123  
root@apiserver:/app# exit  
exit

**Check the file in Node**    

[root@minikube01 hostPath]# cat /tmp/test.txt  
Hello World 123

**Remove the pod**    

[root@minikube01 hostPath]# kubectl delete pod apiserver  
pod "apiserver" deleted

**File still exit**    

[root@minikube01 hostPath]# cat /tmp/test.txt  
Hello World 123
