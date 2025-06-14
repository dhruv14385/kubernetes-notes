Select t2.medium EC2 instance which will have 2 CPUs and select 20 GB volume. Allow all traffic on security group. Connect to instance.  
Get updates on ubuntu. Don't switch to root user.

```
sudo apt-get update
```
```
sudo apt-get install apt-transport-https
```

Install and start Docker  
```
sudo apt install docker.io -y
```
```
docker --version
```
```
service docker status
```
```
sudo usermod -aG docker $USER
```
Reboot instance.  

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
sudo apt install conntrack  
```

To start minikube  
```
minikube start --driver=docker
```
```
minikube status
```
