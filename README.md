# k8s-deployment

**K8S Master**
*Preinstallation*
```
yum install -y yum-utils device-mapper-persistent-data lvm2

hostnamectl set-hostname 'k8s-master'
exec bash
setenforce 0
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux

firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10251/tcp
firewall-cmd --permanent --add-port=10252/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload

modprobe br_netfilter
echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

*create hosts mapping for master and minion*
example
```
vi /etc/hosts
10.1.1.1  k8s-master
10.1.1.2  worker-node1
10.1.1.3  worker-node2
```
**disable swap**
```
swapoff -a
vim /etc/fstab
```
comment this line
`#/dev/mapper/centos-swap swap`



**add repo**
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
	https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```
