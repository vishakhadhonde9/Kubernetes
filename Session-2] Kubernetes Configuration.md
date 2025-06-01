# Configure K8s -

#### Followings are the command to setup kubernetes -
##### Ports -
- 6443
- 8080
- 30000–32767

##### Instance type- 
- t2.medium

#### (Run on both nodes)

      sudo swapoff -a
      
      cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
      overlay
      br_netfilter
      EOF
      
      sudo modprobe overlay
      sudo modprobe br_netfilter
      
      
      cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
      net.bridge.bridge-nf-call-iptables  = 1
      net.bridge.bridge-nf-call-ip6tables = 1
      net.ipv4.ip_forward                 = 1
      EOF
      
      sudo sysctl --system
      
      sudo apt-get update -y
      
      sudo apt-get install -y software-properties-common curl apt-transport-https ca-certificates gpg
      
      sudo curl -fsSL https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/cri-o-apt-keyring.gpg
      
      echo "deb [signed-by=/etc/apt/keyrings/cri-o-apt-keyring.gpg] https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/deb/ /" | sudo tee /etc/apt/sources.list.d/cri-o.list
      
      sudo apt-get update
      
      sudo apt-get install -y cri-o
      
      sudo systemctl daemon-reload
      
      sudo systemctl enable crio --now
      
      sudo systemctl start crio.service
      
      curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
      
      echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
      
      sudo apt-get update -y
      
      sudo apt-get install -y kubelet="1.29.0-*" kubectl="1.29.0-*" kubeadm="1.29.0-*"
      
      sudo systemctl enable --now kubelet
      
      sudo systemctl start kubelet
      
      sudo kubeadm config images pull
       
#### On Master-

      sudo kubeadm init
      
      mkdir -p "$HOME"/.kube
      sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
      sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config
      
      kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml
      
      kubeadm token create --print-join-command


 
#### On Worker:
      Use kubeadm join command with token (copy the command from master and paste on worker)
    
#### On Master:
        kubectl get nodes
   
