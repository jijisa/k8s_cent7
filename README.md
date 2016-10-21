## Kubernetes cluster deployment on CentOS 7

These playbooks deploy the components for kubernetes cluster.
* common tasks (set up ntp, disable firewalld, etc.)
* ETCD key-value datastore on the master
* flannel overlay network on each minion
* kubernetes master
* docker container engine
* a private docker registry on the master
* kubernetes minions

To use, 
1. copy 'hosts.example' to 'hosts' and edit the 'hosts' file.

2. Download and put the latest kubernetes binaries into the right places:

    roles/minion/files/kubelet
    roles/minion/files/kube-proxy
    roles/master/files/kube-apiserver
    roles/master/files/kube-scheduler
    roles/master/files/kubectl
    roles/master/files/kube-controller-manager

You can download kubernetes binaries at 
https://github.com/kubernetes/kubernetes/releases/


Then run the playbook like this:

    ansible-playbook -i hosts site.yml

To run a specific role:

    ansible-playbook -i hosts run_role.yml -e "ROLE=<role>" \
    -e "TARGET=<hostname>"


