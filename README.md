# kubernetes-notes  
Before installing kubernetes (k8s), get updates on ubuntu  
sudo su
apt-get update
apt-get install apt-transport-https  

Install and start Docker  
apt install docker.io -y
docker --version
systemctl start docker
systemctl enable docker  

Add GPG keys. You should see 'OK' after running this command.
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -  

Create a file as below.  
nano /etc/apt/sources.list.d/kubernetes.list  

Paste following line in the file in the nano editor. Ctrl+X then Y then press enter to save and exit  
deb http://apt.kubernetes.io/ kubernetes-xenial main  

Get updates  
apt-get update  





EXAMPLE OF LABELS


kind: Pod
apiVersion: v1
metadata:
  name: delhipod
  labels:                                                   
    env: development
    class: pods
spec:
    containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]



***************************************************************************
NODE SELECTOR EXAMPLE

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
