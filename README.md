# kubernetes-notes  


Remember : Cluster = VPC, Node = EC2   
Cluster - Node - Pod - Container - App   

Each node and each master has its own IP. Containers within a node don't have individual IPs.   

Lecture 48 - Technical Guftgu  
Declarative - Create file and have all instructions in it  
Imperative - Step by step entering commands  
Minikube - Single node cluster. Master and worker components on a single node.  

Get updates and install Docker as mentioned above before installing kubectl.  
Install Kubectl before installing minikube  
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
```
chmod +x ./kubectl
```
```  
sudo mv ./kubectl /usr/local/bin/kubectl
``` 

To locate where kubectl is installed  
```
which kubectl
```
To check kubectl version  
```
kubectl version
```

Install Minikube  
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```

Install conntrack  
```
apt install conntrack  
```

To start minikube  
```
minikube start --vm-driver=none
```

To check minikube status  
```
minikube status
```

To check status of master and worker nodes in minikube  
```
kubectl get nodes  
```

More information about node
```
kubectl describe node ip-172-xx-xx-xx  
```

More info about deployment  
```
kubectl describe deploy name_of_deployment  
```

More info about pods    
```
kubectl describe pod name_of_pod  
```

More info about replica controller  
```
kubectl describe rc name_of_replica  
```
More info about a service  
```
kubectl describe svc name_of_service  
```

To create pod from yml file  
```
kubectl apply -f pod1.yml  
```
To check status of pods  
```
kubectl get pods  
```
To check status of replica controller  
```
kubectl get rc  
```
To check status of replica set   
```
kubectl get rs    
```
To check status of deployment    
```
kubectl get deployment   
```
To check more details of pods like IP address of pod and node  
```
kubectl get pods -o wide  
```
To check status of persistent volume  
```
kubectl get pv  
```
To check status of persistent volume claim   
```
kubectl get pvc    
```
To check logs for pod with one container  
```
kubectl logs -f testpod(name of pod)
```
To check logs for pod with multiple containers  
```
kubectl logs -f testpod(name of pod) -c c00(name of container)
```
To delete pods keeping yml file   
```
kubectl delete pod name_of_pod  
```
To delete pods along with yml file  
```
kubectl delete -f name_of_file.yml
```

To get IP of a pod  
```
kubectl exec testpod -c c00 -- hostname -i
```
To go inside container  
```
kubectl exec name_of_pod -it -c c00 -- /bin/bash
```

To check environment variable, first go to inside pod with following command. Refer environment_variable.yml file in this repo.  
```
kubectl exec environments(name of pod) -it -- /bin/bash  
```
Then type command below. Output should be BHUPINDER
```
echo $MYNAME  
```

To check if the port is exposed or not, find pod IP. Then use curl as below. It should show output as 'It works!'. Refer to port_exposed.yml file in this repo.  
```
curl <port IP>:80  
```
To see labels of a pod. Refer labels.yml file in this repo.  
```
kubectl get pods --show-labels  
```
To apply a label to an existing pod  
```
kubectl label nodes ip-xxx.xx.xx.xx.xx hardware=t2.medium  
```
To list all the pods matching a label  
```
kubectl get pods -l env=development  
```
To list all the pods without a particular label  
```
kubectl get pods -l env!=development  
```
To delete pods with a particular label  
```
kubectl delete pods -l env=development  
```
To get pods with a set of labels (selectors)  
```
kubectl get pods -l 'env in (dev,testing)'  
```
To get pods without a set of labels  
```
kubectl get pods -l 'env notin (dev,testing)'   
```

NODE SELECTOR EXAMPLE - Apply label and then select it based on it.

kind: Pod  
apiVersion: v1  
metadata:  
  name: nodelabels  
  labels:  
    env: development  
spec:  
    containers:  
       - name: c00  
         image: ubuntu  
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]  
    nodeSelector:                                           
       hardware: t2-medium    

*****************************************************************************************************
EXAMPLE OF REPLICATION CONTROLLER  
Replication controller is an object that enables to create multiple pods and make sure that number of pods always exist.  

kind: ReplicationController               
apiVersion: v1
metadata:
  name: myreplica
spec:
  replicas: 2            
  selector:        
    myname: Bhupinder Rajput                             
  template:                
    metadata:
      name: testpod6
      labels:            
        myname: Bhupinder
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]  

To scale replicas  
```
kubectl scale --replicas=8 rc -l myname=bhupinder
```

****************************************************************************************************************
EXAMPLE OF REPLICA SET


kind: ReplicaSet                                    
apiVersion: apps/v1                            
metadata:
  name: myrs
spec:
  replicas: 2  
  selector:                  
    matchExpressions:                             # these must match the labels
      - {key: myname, operator: In, values: [Bhupinder, Bupinder, Bhopendra]}
      - {key: env, operator: NotIn, values: [production]}
  template:      
    metadata:
      name: testpod7
      labels:              
        myname: Bhupinder
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]

**************************************END*****************************   
To check rollout history  
```
kubectl rollout history deployment name_of_deployment  
```
To roll back deployment  
```
kubectl rollout undo deploy/name_of_deployment   
```
  
Emptydir - To share volume between caontainers within a pod. If a pod is deleted, volume is gone.  
Hostpath - To access content of your pod's volume from your host.  
Persistent volume - Cluster-wide volume that remains beyond lifetime of a pod. Can be accessed by multiple nodes (EC2) within a cluster.  
Services - It is a method for exposing a network application.  Enable connectivity between pods. Types of Service - ClusterIP, Nodeport, LoadBalancer.  
ClusterIP - Default service when you don't specify Nodeport or LoadBalancer. Exposes the service only within the cluster.  
Nodeport - Makes a service accessible from outside of a cluster.  
![image](https://github.com/dhruv14385/kubernetes-notes/assets/83332524/8171e2ba-dfe2-4298-8307-41a6fc3e9023)

LoadBalancer -   
Ingress - 


