# gitasservice-k8S
A lightweight Git Server Docker image built with Alpine Linux. Available on [GitHub](https://github.com/samuel-sujith/gitasservice-k8s) and [Docker Hub](https://hub.docker.com/r/samuelsujith/gitindocker/)


### Basic Usage

Copy the ssh-key id_rsa.pub from the server which will connect to the git and replace in the configmap in deploy.yaml

How to use a public key:
    Copy them to keys folder: 
	- From kubernetes: $ kubectl cp <pod-name-from-where-to-access-gitservice>:~/.ssh/id_rsa.pub .
					   $ kubectl cp ./id_rsa.pub <pod-name-for-gitservice>:~/git-server/repos

Run the below
 kubectl apply -f deploy.yaml
 kubectl apply -f svc.yaml
	
How to check that container works (you must to have a key):

	$ ssh git@<git-svc-address>
	...
	Welcome to git-server-docker!
	You've successfully authenticated, but I do not
	provide interactive shell access.
	...

How to create a new repo:

On a local machine

	$ mkdir myrepo.git
	$ cd myrepo.git
	$ git init --baare

How to upload a repo to kubernetes

	From host:
	$ kubectl cp ./myrepo.git <pod-name-for-gitservice>:/git-server/repos -n <git workspace

How clone a repository:

	$ git clone ssh://git@<git-svc-address>:/git-server/repos/myrepo.git

### Arguments

* **Expose ports**: 22
* **Volumes**:
 * */git-server/keys*: Volume to store the users public keys
 * */git-server/repos*: Volume to store the repositories

### SSH Keys

How generate a pair keys in client machine:

	$ ssh-keygen -t rsa

How upload quickly a public key to host volume:

	$ scp ~/.ssh/id_rsa.pub user@host:~/git-server/keys

### Build Image

How to make the image:

	$ docker build -t git-server-docker .
	