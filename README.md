# Minikube Cluster creation steps

# kubectl (latest version)
    
    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    chmod +x kubectl
    mkdir -p ~/.local/bin
    mv ./kubectl ~/.local/bin/kubectl
    kubectl version --client --output=yaml   
  

# Install docker & git:

     sudo yum install docker -y	
     sudo groupadd docker
     sudo usermod -aG docker $USER

# Install cri-dockerd:
    git clone https://github.com/Mirantis/cri-dockerd.git

    wget https://storage.googleapis.com/golang/getgo/installer_linux
    chmod +x ./installer_linux
    ./installer_linux
    source ~/.bash_profile

    cd cri-dockerd
    mkdir bin
    go build -o bin/cri-dockerd
    mkdir -p /usr/local/bin
    install -o root -g root -m 0755 bin/cri-dockerd /usr/bin/cri-dockerd
    cp -a packaging/systemd/* /etc/systemd/system
    sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
    systemctl daemon-reload
    systemctl enable cri-docker.service
    systemctl enable --now cri-docker.socket

# Install cri-tools:

     VERSION="v1.26.0" # check latest version in /releases page
     wget https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
     sudo tar zxvf crictl-$VERSION-linux-amd64.tar.gz -C /usr/local/bin
     rm -f crictl-$VERSION-linux-amd64.tar.gz

# Minikube setup (latest):
	curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
	chmod +x minikube
	sudo mv minikube /usr/local/bin/
	yum install conntrack -y
	export PATH=/usr/local/bin:$PATH
	minikube start --driver=none
---	
## Errors:
 ![New Picture](https://user-images.githubusercontent.com/110303586/227715127-027c1fb5-8d97-4028-8eda-4518ec21604f.jpg)

    sudo setfacl --modify user:<user name or ID>:rw /var/run/docker.sock
---
# Other Versions:

# Install kubectl
```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
# Install Docker 
    sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
    sudo mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    sudo groupadd docker
    sudo usermod -aG docker $USER
# install minikube
    curl -LO https://storage.googleapis.com/minikube/releases/v1.22.0/minikube-linux-amd64
    sudo install minikube-linux-amd64 /usr/local/bin/minikube
# Start VM
    sudo apt-get install -y conntrack
    sudo minikube start --vm-driver=none 
    kubectl cluster-info //Check the Status
    
---

# kubectl (install)
	curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.20.4/2021-04-12/bin/linux/amd64/kubectl
	chmod +x ./kubectl
	mkdir -p $HOME/bin
	cp ./kubectl $HOME/bin/kubectl
	export PATH=$HOME/bin:$PATH
	echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
	source $HOME/.bashrc
	kubectl version --short --client
	
	curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.14.6/2019-08-22/bin/linux/amd64/kubectl
	chmod +x ./kubectl
	mkdir -p $HOME/bin
	cp ./kubectl $HOME/bin/kubectl
	export PATH=$HOME/bin:$PATH
	echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
	source $HOME/.bashrc
	kubectl version --short --client
	
# Minikube setup: (in case of latest version errors)
	curl -Lo minikube https://github.com/kubernetes/minikube/releases/download/v1.25.2/minikube-linux-amd64
	chmod +x minikube
	sudo mv minikube /usr/local/bin/
	yum install conntrack -y
	export PATH=/usr/local/bin:$PATH
	minikube start --driver=none
