<<<<<<< HEAD
# 1. Lucid Chart #
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/LucidChart.png)

# 2. Tools, prerequisites and environment #
=======
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/LucidChart.png)
# 1. Tools, prerequisites and environment #
>>>>>>> 8060d5e0bbac1fb5a8f62eb31de1055f7324d4cc
1. Metal
	1. Vagrant
	![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/VirtualBox.png)
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

# 3. Metal #
1. [Install Vagrant](https://learn.hashicorp.com/collections/vagrant/getting-started)
2. vagrant init to generate Vagrantfile
3. Modify Vagrantfile (see the Vagrangtfile)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/Vagrantfile.png) 
4. Create and boot up a Vagrant VM according to the Vagrantfile
5. Set up the installation for snapd and microk8s in order to run the microk8s for Kubernetes

# 4. Master VM - SSH #
1. Install Ansible on the Master VM
2. sudo nano /etc/hosts to set up the alias for vagrant
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/etc_hosts.png)
3. sudo nano /etc/ansible/hosts
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/etc_ansible_hosts.png)
4. ssh-keygen and ssh-copy-id vagrant@vagrant to enable master VM ssh into Vagrant VM
5. ssh vagrant@vagrant to exam the ssh works in order to use ansible later 
6. sudo su root & sudo su jenkins, then do the same ssh set up from jenkins user

# 5. Master VM - Kubernetes.yaml #
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/Kubernetes.png)

# 6. Master VM - Ansible #
1. Under the “2020_03_DO_Boston_casestudy_part_2” directory, create an ansible playbook called “ansible-playbook-setup.yaml”, modify it to let it install some dependencies and apply the Kubernetes.yaml file.
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/ansible-playbook.png)
2. sudo microk8s kubectl get all –all-namespaces 
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/all-namespaces.png)
 
# 7. Master VM - Jenkins #
1. Jenkins Config
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/jenkins-config.png)
2. Jenkins Pipeline
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/jenkins-output-1.png)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/jenkins-output-2.png)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/jenkins-output-3.png)

# 8. Vagrant VM – Grafana – Set Up #
1. sudo microk8s kubectl proxy & 
2. sudo microk8s kubectl port-forward -n monitoring service/prometheus-k8s --address 0.0.0.0 9090:9090 &
3. sudo microk8s kubectl port-forward -n monitoring service/grafana --address 0.0.0.0 3000:3000 &
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/port-forward.png)
4. Node
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/grafana-node.png)
5. Pod
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/grafana-pod.png)
6. Cluster
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/grafana-cluster.png)

# 9. Vagrant VM - Stress #
1. vagrant ssh
2. top
3. sudo stress --cpu 2 --timeout 5
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/stress-1.png)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/stress-2.png)

# 10. Challenges #
1. By following the instruction for Vagrant, I was using image “hashicorp/bionic64” for my Vagrant VM in the first place, It was running ok but when it talks to ansible playbook and does “microk8s kubectl apply –f Kubernetes.yaml”,  some errors would interrupt the script, so, I changed the box image from “hashicorp/bionic64” to “peru/ubuntu-20.04-server-amd64” to make the Vagrant VM fits the environment.
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/image.png)
2. For openssh , in order to let my master VM to ssh into my Vagrant VM, I need this piece of configuration on the right to set up a bridged adaptor.
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/public_network.png)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/vagrant%20reload.png)
3. The default Vagrant password is “vagrant”
4. In “ansible-playbook-setup.yaml”, I got this error message as the screenshot on the right, so, I realize it’s the matter of the path, I don’t have “Kubernetes.yaml” file in my Vagrant VM’s directory, so, I did the git clone to clone my case study repo, then, I did the “ls” to show the folders in the current directory.I can see the repo has successfully cloned to my current directory. The next step is cd into that repo, as I cd into there, I did the “ls” command, but it seemed I didn’t successfully cd into that repo and there is no file called “Kubernetes.yaml”. To solve this problem, I directly specified the path of the “Kubernetes.yaml” file and then I made it worked out.
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/challenges-1-1.png)
5. Command to solve issue 4: microk8s kubectl apply -f ./2020_03_DO_Boston_casestudy_part_2/Kubernetes.yaml
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/challenges-1-2.png)
![](https://github.com/willstopher817/2020_03_DO_Boston_casestudy_part_2/blob/main/screenshots/jenkins-output-3.png)