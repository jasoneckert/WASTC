# Kubernetes Overview (it's a black box!)


# Installing K3S
  - Open a Terminal on your Ubuntu Server virtual machine as root
  - `curl -sfL https://get.k3s.io | sh -` 
  - `kubectl get nodes`

# Working with Deployments, Pods and Services
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

# Horizontal Pod Autoscaling (HPA)
  - `kubectl top nodes` (note that K3S comes with a metrics service that collects statistics - these can be used to perform HPA)
  - `kubectl autoscale deployment webapp --min=3 --max=8 --cpu-percent=50` (create HPA configuration for your Web app that automatically scales from 3 to 8 pods when a consistent trend of more than 50% of the CPU is consumed)
  - `kubectl get hpa webapp` 

# Upgrading/Downgrading Container Images in Production
After a while, your developers will make a new container image available (e.g. jasoneckert/x86webapp:2.0 on Docker Hub). To update your Kubernetes cluster to use the new image in production, you can either:
- Edit your deployment (`kubectl edit deployment webapp`), modify the `Image` line and save your changes, or
- Run `kubectl set image deployment/webapp webapp=jasoneckert/webapp:2.0`

Kubernetes will immediately start replacing the pods with your new image in sequence until all of them are upgraded. If an upgraded image causes stability issues, you can revert to the previous image using `kubectl rollout undo deployment webapp`. You can also use `kubectl rollout history deployment webapp` to view rollout history.

# Ingress
Up until now, you've had to access the pods that comprise webapp using http://UbuntuIP:port (where port is the port exposed from our NodePort service). This is why there is no public EXTERNAL-IP listed in the output of `kubectl get service webapp`. On a cloud provider, you normally configure either a load balancer or an ingress controller to provide access to a service from outside the Kubernetes cluster. To use a load balancer, you just need to change from using NodePort to LoadBalancer and pay for the load balancing service on your cloud provider. The cloud provider then gives you an external IP that you can use when creating a DNS record for your Web app. Alternatively, you can use an ingress controller proxy (e.g. Nginx, HAProxy or Traefik). In this method, you configure a node balancer in front of your cluster that routes traffic to Nginx/HAProxy/Traefik and then your Web app.

Let’s install an Nginx ingress controller in our Kubernetes cluster and configure it to allow external access to our Web app. The easiest way to install additional components in a Kubernetes cluster is by using the Helm package manager, which uses helm charts to store the information needed to add/configure the appropriate software. There are many different repositories on the Internet that provide helm charts for different Kubernetes components. The following commands install the Helm package manger, add the Nginx Helm repository, and install the Nginx ingress controller:

  -  that are collected container)
  -  service webapp
