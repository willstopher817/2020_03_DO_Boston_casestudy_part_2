# 1. Tools, prerequisites and environment #

1. Metal
	1. Vagrant
2. Vagrant VM
	1. Microk8s -Kubernetes
	2. openssh
	3. snapd
	4. net-tools
	5. Grafana
3. Master VM
	1. [Jenkins](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04)
	2. Ansible
	3. ssh 

# 2. Metal #
1. [Install Vagrant](https://learn.hashicorp.com/collections/vagrant/getting-started)
2. vagrant init to generate Vagrantfile
3. Modify Vagrantfile (see the Vagrangtfile) 
4. Create and boot up a Vagrant VM according to the Vagrantfile
5. Set up the installation for snapd and microk8s in order to run the microk8s for Kubernetes

# 3. Master VM - SSH #
1. Install Ansible on the Master VM
2. sudo nano /etc/hosts to set up the alias for vagrant
3. sudo nano /etc/ansible/hosts
4. ssh-keygen and ssh-copy-id vagrant@vagrant to enable master VM ssh into Vagrant VM
5. ssh vagrant@vagrant to exam the ssh works in order to use ansible later 
6. sudo su root & sudo su jenkins, then do the same ssh set up from jenkins user

# 3. Master VM - Kubernetes.yaml #
(See Kubernetes.yaml)

# 4. Master VM - Ansible #
1. Under the “2020_03_DO_Boston_casestudy_part_2” directory, create an ansible playbook called “ansible-playbook-setup.yaml”, modify it to let it install some dependencies and apply the Kubernetes.yaml file.
(see ansible-playbook-setup.yaml)

# 5. Vagrant VM – Grafana – Set Up #
1. sudo microk8s kubectl proxy & 
2. sudo microk8s kubectl port-forward -n monitoring service/prometheus-k8s --address 0.0.0.0 9090:9090 &
3. sudo microk8s kubectl port-forward -n monitoring service/grafana --address 0.0.0.0 3000:3000 &

# 6. Vagrant VM - Stress #
1. sudo stress --cpu 2 --timeout 5
