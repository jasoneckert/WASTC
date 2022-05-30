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

NOTE: You can add the `-o yaml` output to most `kubectl` to obtain the information about a component in YAML format. This output if often called a ***manifest*** as it can also be saved to a file and used alongside the `kubectl apply -f filename` command to automatically configure components in Kubernetes.

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
Up until now, you've had to access the pods that comprise webapp using http://UbuntuIP:port (where port is the port exposed from our NodePort service). This is because K3S comes preconfigured with the Traefik ingress controller that allows external traffic to enter your Kubernetes cluster and interact with exposed services. However, most Kubernetes clusters do not come with a preconfigured ingress controller, and you must configure either a load balancer service or an ingress controller to provide access to a service from outside the Kubernetes cluster. 
- To use a load balancer, you just need to change from using NodePort to LoadBalancer and pay for the load balancing service on your cloud provider. The cloud provider then gives you an external IP that you can use when creating a DNS record for your Web app. 
- Alternatively, you can use an ingress controller (Traefik, Nginx, HAProxy, etc.) that accepts and reroutes traffic to your services. The easiest way to install an ingress controller in your Kubernetes cluster is by using the ***Helm package manager*** which uses ***Helm charts*** to define what components to download and how to connect them to other components within Kubernetes.

NOTE: To configure a different ingress controller in K3S, you must edit /etc/systemd/system/k3s.service, add the `--disable traefik` option and restart K3S with `systemctl restart k3s.service`.

Let’s install an Nginx ingress controller in our Kubernetes cluster and configure it to allow external access to our Web app. The easiest way to install additional components in a Kubernetes cluster is by using the Helm package manager, which uses helm charts to store the information needed to add/configure the appropriate software. There are many different repositories on the Internet that provide helm charts for different Kubernetes components. The following commands install the Helm package manger, add the Nginx Helm repository, and install the Nginx ingress controller:

  -  that are collected container)
  -  service webapp
