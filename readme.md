This lab guide will show you how to use Ansible to configure Kubernetes cluster with docker runtime on a Windows workstation.

### Install

1.	Install Hyper-v
2.	Install WSL (We gonna use WSL as our kubernetes management node)
3.	Install Ubuntu (or any other dist) from MS Store or manually for WSL. For this lab Ubuntu dist will be used.
4.	Start Ubuntu WSL
a.	Sudo apt update -y
b.	Sudo apt upgrade -y 
c.	sudo apt install ansible -y
5.	Configure our kubernetes NAT network. Open Powershell.
    ```javascript
    New-VMSwitch -SwitchName "NATSwitch" -SwitchType Internal
    New-NetIPAddress -IPAddress 192.168.0.1 -PrefixLength 24 -InterfaceAlias "vEthernet (NATSwitch)"
    New-NetNAT -Name "NATNetwork" -InternalIPInterfaceAddressPrefix  192.168.0.0/24
    ```
6.	Download ubuntu iso
7.	Create 3 VM’s (2 vCPU, 2G Ram)
a.	K8s-m = master node
b.	K8s-w1 = worker node 1
c.	K8s-w2 = worker node 2
8.	Configure eth0 on…
a.	Master: 192.168.0.2/24
b.	Worker1: 192.168.0.3/24
c.	Worker2: 192.168.0.4/24
9.	Configure Ansible
a.	Open Code on your Windows Desktop
b.	Create a project folder (example:C:\project\kube)
c.	Create an inventory file named inventory.cfg
I will assume you are using the same ssh keys, but if not… just login once to each node and accept the key.
10.	Create kube-init.yaml
11.	Run ansible-playbook -i inventory.cfg kube-init.yaml --ask-become-pass
12.	Create kube-deps.yaml
13.	ansible-playbook -i inventory.cfg kube-deps.yaml --ask-become-pass
14.	Create kube-master.yaml
15.	ansible-playbook -i inventory.cfg kube-master.yaml --ask-become-pass
16.	Create kube-worker.yaml
17.	ansible-playbook -i inventory.cfg kube-worker.yaml --ask-become-pass
18.	install kubectl on your workstation (windows)
a.	Install-Script -Name install-kubectl -Scope CurrentUser -Force
b.	install-kubectl.ps1
19.	install kubectl on WSL (ubuntu)
a.	sudo apt-get update && sudo apt-get install -y apt-transport-https
b.	curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add –
c.	echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
d.	sudo apt-get update
e.	sudo apt-get install -y kubectl
20.	copy the kube config files to your workstation.
21.	scp -r bob@192.168.0.2:/home/kubeuser/.kube $HOME/.kube
22.	run following command from WSL: kubectl get nodes
    ```
    NAME     STATUS   ROLES    AGE    VERSION
    k8s-m    Ready    master   179m   v1.14.0
    k8s-w1   Ready    <none>   176m   v1.14.0
    k8s-w2   Ready    <none>   176m   v1.14.0
    ```


### Storage
We gonna mount some local presistent storage for our lab
1.	Either you add additional disks to each node and mount it to the filesystem or use existing disk. If you choose to use existing, you can skip the following steps below.
a.	In hyper-v add additonal disk (10G+) on each node
b.	Scan bus for new disks
c.	Format the disk
d.	Partition the disk
e.	Create /data directory
f.	Mount sdb1 to /data
g.	Add to fstab
2.	Create a volume claim
3.	From your WSL run: 
kubectl apply -f pv-volume.yaml

### Deploy demo app

1.	kubectapply -f nginx.yaml
3.	To debug app: kubectl port-forward deployment.apps/nginx-deployment 8080:80
4.	app is now exposed at localhost:8080 on your workstation
