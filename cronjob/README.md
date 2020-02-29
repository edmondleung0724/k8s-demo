Create cronjob  

[root@minikube01 cronjob]# kubectl create -f my-cronjob.yaml  
cronjob.batch/hello created  

Check cronjob  

[root@minikube01 cronjob]# kubectl get cronjobs  
NAME    SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE  
hello   */1 * * * *   False     0        <none>          4s  

[root@minikube01 cronjob]# kubectl get jobs --watch  
NAME               COMPLETIONS   DURATION   AGE  
hello-1582974000   1/1           11s        40s  

[root@minikube01 cronjob]# kubectl get pods  
NAME                     READY   STATUS      RESTARTS   AGE  
hello-1582974000-4jvqd   0/1     Completed   0          45s  

[root@minikube01 cronjob]# kubectl get pods -o wide  
NAME                     READY   STATUS      RESTARTS   AGE    IP           NODE                   NOMINATED NODE   READINESS GATES  
hello-1582974000-4jvqd   0/1     Completed   0          109s   172.17.0.3   minikube01.lab.local   <none>           <none>  
hello-1582974060-f7lsj   0/1     Completed   0          49s    172.17.0.3   minikube01.lab.local   <none>           <none>  

Check pod's log  

[root@minikube01 cronjob]# kubectl logs hello-1582974000-4jvqd  
Hi, current time is Sat Feb 29 11:00:17 UTC 2020  
[root@minikube01 cronjob]# kubectl logs hello-1582974060-f7lsj  
Hi, current time is Sat Feb 29 11:01:14 UTC 2020  

[root@minikube01 cronjob]# kubectl get pods -o wide --show-labels  
NAME                     READY   STATUS      RESTARTS   AGE     IP           NODE                   NOMINATED NODE   READINESS GATES   LABELS  
hello-1582974600-ms24r   0/1     Completed   0          2m18s   172.17.0.3   minikube01.lab.local   <none>           <none>            controller-uid=e35527d5-90e5-4f2c-860a-c8559f776ddb,job-name=hello-1582974600  
hello-1582974660-bf6m2   0/1     Completed   0          77s     172.17.0.3   minikube01.lab.local   <none>           <none>            controller-uid=cc589d39-0d7d-4c54-b409-516aeccf6fcf,job-name=hello-1582974660  
hello-1582974720-s5kvt   0/1     Completed   0          17s     172.17.0.3   minikube01.lab.local   <none>           <none>            controller-uid=93b57d6e-cc55-48ae-889e-630a82746dbe,job-name=hello-1582974720  


