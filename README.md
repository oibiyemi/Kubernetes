# Kubernetes
#kubernetes installation

#!/bin/bash
# Common stages for both master and worker nodes
# This can be use as user data in launch template or launch configutions
sudo su -
sudo hostname node13
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

sudo apt update -y
sudo apt install -y apt-transport-https -y

sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -

sudo cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt update -y
sudo apt install -y kubelet kubeadm containerd kubectl
# apt-mark hold will prevent the package from being automatically upgraded or removed.

sudo apt-mark hold kubelet kubeadm kubectl containerd

sudo cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

sudo cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system

sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo systemctl restart containerd

# Enable and start kubelet service
sudo systemctl daemon-reload
sudo systemctl start kubelet
sudo systemctl enable kubelet.service


#Use the token that will be generated
#sudo kubeadm join 172.31.13.51:6443 --token nsw52x.jqv5lezwzlpfkeuq --discovery-token-ca-cert-hash sha256:289ef23de4847829acf89456fa644f07ad0eab312006a4c0d085af051dc0a83d


=============In Master node Start======================

#steps only for Kubernetes Master
   #Switch to the root user 
   #Initialize the Kubernetes master by executing
   #
 
#RUN THIS INSTEAD
#sudo kubeadm init --pod-network-cidr=10.244.0.0/16
#Once this command finishes, it will display a kubeadm join message at the end. Make a note of the whole entry. 
#This will be used to join the worker nodes to the cluster.

#Next, enter the following to create a directory for the cluster:Make sure to run it as Ubuntu user

# mkdir -p $HOME/.kube
# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
#  sudo chown $(id -u):$(id -g) $HOME/.kube/config

To get the master ready run the command below

#kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

#To initiate the cluster, run the command below
#kubeadm init  
   
 #kubectl get node
   

