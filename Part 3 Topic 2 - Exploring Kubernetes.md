# Kubernetes Overview (it's a black box!)


# Installing K3S
  - Open a Terminal on your Ubuntu Server virtual machine as root
  - `curl -sfL https://get.k3s.io | sh -` 
  - `kubectl get nodes`
  - `kubectl create deployment webapp --image=jasoneckert/x86webapp:latest`	(creates a deployment configuration used to start/manage/scale containers based on a container image)
  - `kubectl expose deployment webapp --type=NodePort --port=80` (creates a service to expose the deployment (NodePort exposes the port on every cluster node)
  - 
minikube service webapp
