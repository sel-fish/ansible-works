# ansible-works
works with ansible

## Prerequisites

- ansible

## ssh_nopass.yml

This playbook aims to make any node in the group can establish ssh connection with others wihout prompting for password.

### Usage



1. Prepare the `console` node

	Pick up a node as the role of `console`, let's say it is `n0`.  
	Run following command if the public key `~/.ssh/id_rsa.pub` is not ready :
	
	```
	ssh-keygen -b 2048 -t rsa -q -N '' -f ~/.ssh/id_rsa
	```
	
	After the public key is ready, clone the project
	
	```
	git clone https://github.com/sel-fish/ansible-works.git
	cd ansible-works
	```

2. Fill up nodes in `hosts`

	```
	cat hosts
	
	[daily]
	n1:5191
	n2:5191
	n3:5191
	n4:5191
	```

	`n1`/`n2`/`n3`/`n4` are hostnames of nodes.  
	`5191` is specified ssh port, drop it when use the default `22` as ssh port. 

3. Run the command

	```
	ansible-playbook -i hosts ssh_nopass.yml -e "hosts=daily user=sel-fish" -k
	```
	
	This playbook will execute the following tasks:
	- Deliver SSH public key of `n0` to each node in the group, this step can be avoided if you don't actually need a console.
	- Generate SSH public keys in each node
	- Grab SSH keys of each node and save to local file
	- Deliver Grabbed SSH public keys to each node
	- Turn off StrictHostKeyChecking
	
	When there are new nodes in the group, just add them to `hosts`, and run this playbook again, it's reenterable.



