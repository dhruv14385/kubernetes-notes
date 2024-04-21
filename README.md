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

Install kubelet, kubeadm, kubectl etc.  
apt-get install -y kubelet kubeadm kubectl kubernetes-cni  

Bootstraping the master node  (Run in master EC2)
kubeadm init  

You will see command to connect master to the node. Copy and paste it aside to use in node instance.  
kubeadm join .... 

Then paste following  
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config  

Paste following for flannel installation  
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml  

Go to node instance and paste the kubeadm join... command  

Kubernetes installation complete with the step above.  

Run below command in master to check status of nodes
kubectl get nodes  

Each node and each master has its own IP. Containers within a node don't have individual IPs.   

Lecture 48 - Technical Guftgu  
Declarative - Create file and have all instructions in it
Imperative - Step by step entering commands  
Minikube - Single node cluster. Master and worker components on a single node.

Install Kubectl before installing minikube  
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  
chmod +x ./kubectl  
sudo mv ./kubectl /usr/local/bin/kubectl  





 










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
