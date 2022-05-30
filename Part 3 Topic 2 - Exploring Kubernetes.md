# Kubernetes Overview (it's a black box!)


# Installing K3S
  - Open a Terminal on your Ubuntu Server virtual machine as root
  - `curl -sfL https://get.k3s.io | sh -` 
  - `kubectl get nodes`
  - `kubectl create deployment webapp --image=jasoneckert/x86webapp:latest`	(creates a deployment configuration used to start/manage/scale containers based on a container image that is pulled in from Docker Hub)
  - `kubectl get deployment`
  - `kubectl expose deployment webapp --type=NodePort --port=80` (creates a service to expose the deployment (NodePort exposes the port on every cluster node)
  - `kubectl get service` (note that the service is exposed within the internal Kubernetes network - 10.0.0.0)
  - `kubectl get pod` (note that one pod was created for the deployment - the pod represents one or more containers created by a deployment and is the smallest unit of management in Kubernetes)
  - `kubectl edit deployment webapp` (modify `Replicas=3` and save your changes)
  - `kubectl get deployment webapp` (note that 3 containers are now running in the pod based on the same container image - requests sent to the service are load balanced across these containers)
  - `kubectl get pods -l app=webapp` (to see these 3 containers)
  - `kubectl logs -f -l app=webapp --prefix=true` (this displays events related to your webapp and is often useful when troubleshooting issues with a deployment such as not being able to pull the container image)
  - `kubectl get service webapp -o yaml` (note the `nodePort:` line to see which service port is automatically exposed outside of the Kubernetes cluster)
  - Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIP:port (where port is the exposed service port from the previous command)
  - `kubectl top nodes` (note that K3S comes with a metrics service that collects statistics - these can be used to perform ***horizontal pod autoscaling (HPA)***)
  - `kubectl autoscale deployment webapp --min=3 --max=8 --cpu-percent=50` (create HPA configuration for your Web app that automatically scales from 3 to 8 pods when a consistent trend of more than 50% of the CPU is consumed)
  - `kubectl get hpa webapp` 
  -  that are collected container)
  -  service webapp
