# Introduction 
This is instruction to install K8s cluster using Kubeadm

===================== Install Cluster ==================

### Get the Docker gpg key:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
### Add the Docker repository:
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
### Get the Kubernetes gpg key:
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
### Add the Kubernetes repository:
```
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
### Update your packages:
```
sudo apt-get update
```
### Install Docker, kubelet, kubeadm, and kubectl:
```
sudo apt-get install -y docker-ce=5:19.03.12~3-0~ubuntu-bionic kubelet=1.18.5-00 kubeadm=1.18.5-00 kubectl=1.18.5-00
```
### Hold them at the current version:
```
sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```
### Add the iptables rule to sysctl.conf:
```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
```
### Enable iptables immediately:
```
sudo sysctl -p
```
trên đây là mình chạy cả 3 con là master và node lẫn node2
còn lại phía dưới là mình chạy cho master thôi
### Initialize the cluster (run only on the master):
```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address {ip add of master node}
```
###{ip add of master node} mình đặt 3 con server master node1 và node2 lúc này mình chọn master là con chính và master có địa chỉ ip là 192.168.33.201 thì lúc này sẽ viết lại là 

```
sudo swapoff -a
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address 192.168.33.201
```
sau khi chạy 2 đoạn lệnh trên thì trong server master nó sẽ show ra như vầy
```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.33.201:6443 --token k2qwk0.yxtblta5yr07utup --discovery-token-ca-cert-hash sha256:3b762026f8f5f696f74a9c01015b05a0a68665f6c00dcd65e0831f4ceb8d585e

```

lúc này mình lưu key này lại
```
kubeadm join 192.168.33.201:6443 --token k2qwk0.yxtblta5yr07utup --discovery-token-ca-cert-hash sha256:3b762026f8f5f696f74a9c01015b05a0a68665f6c00dcd65e0831f4ceb8d585e
```

### Set up local kubeconfig (run only on the master):
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### Apply Calico CNI network overlay (run only on the master):
```
kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```
tới đây mình qua 2 con server node1 và node2 chạy
```
sudo swapoff -a
sudo kubeadm join 192.168.33.201:6443 --token k2qwk0.yxtblta5yr07utup --discovery-token-ca-cert-hash sha256:3b762026f8f5f696f74a9c01015b05a0a68665f6c00dcd65e0831f4ceb8d585e
```
### Join the worker nodes to the cluster:
```
sudo kubeadm join [your unique string from the kubeadm init command]
```
### Verify the worker nodes have joined the cluster successfully:
```
kubectl get nodes
```
### Running end to end test
|  kubectl run nginx  |   |   |   |   |
|---|---|---|---|---|
|   |   |   |   |   |
|   |   |   |   |   |
|   |   |   |   |   |

### Tips

In installation process maybe you will due to error or warning message relate to Swap.
In some case recommendation is turn off Swap using this command 
```
swapoff -a 
``` 

