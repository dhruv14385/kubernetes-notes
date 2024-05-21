Get updates on ubuntu  
```
sudo su
```
```
apt-get update
```
```
apt-get install apt-transport-https
```

Install and start Docker  
```
apt install docker.io -y
```
```
docker --version
```
```
service docker status
```
```
systemctl start docker
```
```
systemctl enable docker
```

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
