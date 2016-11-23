## Kubernetes cluster deployment on CentOS 7

The playbook deploy the components for kubernetes cluster.

The playbook has the following roles in order:

* common role: common tasks (set up ntp, disable firewalld, etc.)
* etcd role: ETCD key-value datastore on the master
* flannel role: flannel overlay network on each minion
* master role: kubernetes master
* docker role: docker container engine
* registry role: a private docker registry on the master
* minion role: kubernetes minions

The order is important. The etcd role should be played before flannel role
since flannel depends on etcd.

The default storage backend of docker on CentOS is loopback-lvm which is not production-ready. It is recommended to use direct-lvm.
So the docker role sets up direct-lvm storage. 
Edit roles/docker/vars/main.yml to set up the partition to use for docker lvm.

To use,
 1. copy 'hosts.example' to 'hosts' and edit the 'hosts' file.
 2. Download and put the latest kubernetes binaries into the right places.

        roles/minion/files/kubelet
        roles/minion/files/kube-proxy
        roles/master/files/kube-apiserver
        roles/master/files/kube-scheduler
        roles/master/files/kubectl
        roles/master/files/kube-controller-manager

        You can download kubernetes binaries at https://github.com/kubernetes/kubernetes/releases/

 3. Edit vars files in each role and global group_vars and host_vars.
 4. Run the playbook:

        ansible-playbook -i hosts site.yml
If you want to run only for the certain group, use --limit option:

    ansible-playbook -i hosts site.yml --limit=master
    ansible-playbook -i hosts site.yml --limit=minion


To run a specific role on a specific host:

    ansible-playbook -i hosts run_role.yml -e "ROLE=<role>" -e "TARGET=<hostname>"

