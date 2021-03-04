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
1. vagrant ssh
2. top
1. sudo stress --cpu 2 --timeout 5

# 7. Challenges #
1. By following the instruction for Vagrant, I was using image “hashicorp/bionic64” for my Vagrant VM in the first place, It was running ok but when it talks to ansible playbook and does “microk8s kubectl apply –f Kubernetes.yaml”,  some errors would interrupt the script, so, I changed the box image from “hashicorp/bionic64” to “peru/ubuntu-20.04-server-amd64” to make the Vagrant VM fits the environment.
2. For openssh , in order to let my master VM to ssh into my Vagrant VM, I need this piece of configuration on the right to set up a bridged adaptor.
3. The default Vagrant password is “vagrant”
4. In “ansible-playbook-setup.yaml”, I got this error message as the screenshot on the right, so, I realize it’s the matter of the path, I don’t have “Kubernetes.yaml” file in my Vagrant VM’s directory, so, I did the git clone to clone my case study repo, then, I did the “ls” to show the folders in the current directory.I can see the repo has successfully cloned to my current directory. The next step is cd into that repo, as I cd into there, I did the “ls” command, but it seemed I didn’t successfully cd into that repo and there is no file called “Kubernetes.yaml”. To solve this problem, I directly specified the path of the “Kubernetes.yaml” file and then I made it worked out. 
5. Command to solve issue 4: microk8s kubectl apply -f ./2020_03_DO_Boston_casestudy_part_2/Kubernetes.yaml
