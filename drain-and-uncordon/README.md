**Create pod**  

[root@minikube01 drain-and-uncordon]# kubectl create -f hello-deployment.yaml  
deployment.apps/helloworld-deployment created  

**Check pod status**  

[root@minikube01 drain-and-uncordon]# kubectl get pod  
NAME                                    READY   STATUS    RESTARTS   AGE  
helloworld-deployment-6bd884767-gtcsx   1/1     Running   0          11s  
helloworld-deployment-6bd884767-r44pp   1/1     Running   0          11s  
helloworld-deployment-6bd884767-xvrlg   1/1     Running   0          11s  

[root@minikube01 drain-and-uncordon]# kubectl get nodes  
NAME                   STATUS   ROLES    AGE     VERSION  
minikube01.lab.local   Ready    master   5d22h   v1.17.3  

[root@minikube01 drain-and-uncordon]# kubectl get deploy,pods,nodes  
NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/helloworld-deployment   3/3     3            3           57s  

NAME                                        READY   STATUS    RESTARTS   AGE  
pod/helloworld-deployment-6bd884767-gtcsx   1/1     Running   0          57s  
pod/helloworld-deployment-6bd884767-r44pp   1/1     Running   0          57s  
pod/helloworld-deployment-6bd884767-xvrlg   1/1     Running   0          57s  

NAME                        STATUS   ROLES    AGE     VERSION  
node/minikube01.lab.local   Ready    master   5d22h   v1.17.3    

**Execute kubectl drain**  

[root@minikube01 drain-and-uncordon]# kubectl drain minikube01.lab.local  
node/minikube01.lab.local cordoned  
evicting pod "helloworld-deployment-6bd884767-r44pp"  
evicting pod "helloworld-deployment-6bd884767-gtcsx"  
evicting pod "helloworld-deployment-6bd884767-xvrlg"  
evicting pod "coredns-6955765f44-6p68j"  
evicting pod "coredns-6955765f44-cxlct"  
pod/coredns-6955765f44-cxlct evicted  
pod/coredns-6955765f44-6p68j evicted  
pod/helloworld-deployment-6bd884767-gtcsx evicted  
pod/helloworld-deployment-6bd884767-r44pp evicted  
pod/helloworld-deployment-6bd884767-xvrlg evicted  
node/minikube01.lab.local evicted    

**Check pod status**  

[root@minikube01 drain-and-uncordon]# kubectl get pod  
NAME                                    READY   STATUS    RESTARTS   AGE  
helloworld-deployment-6bd884767-npxzf   0/1     Pending   0          71s  
helloworld-deployment-6bd884767-p82gt   0/1     Pending   0          71s  
helloworld-deployment-6bd884767-t9vvz   0/1     Pending   0          71s  

[root@minikube01 drain-and-uncordon]# kubectl get deploy,pods,nodes  
NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/helloworld-deployment   0/3     3            0           2m53s  

NAME                                        READY   STATUS    RESTARTS   AGE  
pod/helloworld-deployment-6bd884767-npxzf   0/1     Pending   0          89s  
pod/helloworld-deployment-6bd884767-p82gt   0/1     Pending   0          89s  
pod/helloworld-deployment-6bd884767-t9vvz   0/1     Pending   0          89s  

NAME                        STATUS                     ROLES    AGE     VERSION  
node/minikube01.lab.local   Ready,SchedulingDisabled   master   5d22h   v1.17.3  

**Execute kubectl uncordon**  

[root@minikube01 drain-and-uncordon]# kubectl uncordon minikube01.lab.local  
node/minikube01.lab.local uncordoned  

[root@minikube01 drain-and-uncordon]# kubectl get deploy,pods,nodes  
NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/helloworld-deployment   0/3     3            0           3m42s  

NAME                                        READY   STATUS              RESTARTS   AGE  
pod/helloworld-deployment-6bd884767-npxzf   0/1     ContainerCreating   0          2m18s  
pod/helloworld-deployment-6bd884767-p82gt   0/1     ContainerCreating   0          2m18s  
pod/helloworld-deployment-6bd884767-t9vvz   0/1     ContainerCreating   0          2m18s  

NAME                        STATUS   ROLES    AGE     VERSION  
node/minikube01.lab.local   Ready    master   5d22h   v1.17.3  

[root@minikube01 drain-and-uncordon]# kubectl get deploy,pods,nodes  
NAME                                    READY   UP-TO-DATE   AVAILABLE   AGE  
deployment.apps/helloworld-deployment   3/3     3            3           3m51s  

NAME                                        READY   STATUS    RESTARTS   AGE  
pod/helloworld-deployment-6bd884767-npxzf   1/1     Running   0          2m27s  
pod/helloworld-deployment-6bd884767-p82gt   1/1     Running   0          2m27s  
pod/helloworld-deployment-6bd884767-t9vvz   1/1     Running   0          2m27s  

NAME                        STATUS   ROLES    AGE     VERSION  
node/minikube01.lab.local   Ready    master   5d22h   v1.17.3  
