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

NOTE: You can add `-o yaml` to most `kubectl` commands to obtain configuration information in YAML format. This information is called a ***manifest*** as it can also be saved to a file and used alongside the `kubectl apply -f filename` command to automatically configure components in Kubernetes.

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

# Monitoring 
You can also use graphical apps to monitor and manage Kubernetes. The most common app for this is Lens (https://k8slens.dev), which is quite powerful. Unfortunately, it can also be quite daunting for those new to Kubernetes as it can view/edit all of the core concepts (deployments, services, HPA, etc.) as well as advanced ones.

Most administrators use a combination of command line tools and templating tools (e.g. Terraform) for managing Kubernetes, but use graphical tools for monitoring it. There are many cloud-based monitoring tools (e.g. Datadog) that can be integrated into Kubernetes for a fee, as well as free tools that you can install directly in your Kubernetes cluster, such as Prometheus and Grafana. Prometheus monitors the events in your cluster and sends the data to Grafana for visualization. Even the graphs and metrics shown in the Lens app require Prometheus (they are empty otherwise).

To install both Prometheus and Grafana, you can install the Prometheus stack using a helm chart:
   - `snap install helm --classic`
   - `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   - `helm repo update`
   - `helm install prometheus prometheus-community/kube-prometheus-stack`

This will install a series of pods, including a pod called prometheus-grafana. Since these pods take several minutes to start, watch the output of kubectl get pods periodically to know when they are ready. Next, open the Lens app, locate the prometheus-grafana pod, and scroll down until you find the URL with port 3000. Click the appropriate button/link to set up a port forward (this opens your default Web browser to access it much like minikube service does). Alternatively, you could run the following commands to expose the service:

kubectl expose service prometheus-grafana --type=NodePort --target-port=3000 --name=pgservice
minikube service prometheus-grafana
Next, log into the Grafana Web app as the user admin (default password is prom-operator) and view the different available monitoring templates by navigating to Dashboards > Browse (click on each one). Following is the compute resource (pod) dashboard:

If you’ve browsed around the Lens app, you’ve probably noticed that we’ve only scratched the surface of Kubernetes configuration, and only to provide a very rough proof-of-concept style overview only. There’s a lot more that you’ll likely want to know at this stage, and you should now have enough basic knowledge to search for the relevant information in the Kubernetes documentation: https://kubernetes.io/docs/home/

# Additional Core Concepts (for further exploration)
- You can integrate Kubernetes directly with a DNS provider (~Dynamic DNS) using https://github.com/kubernetes-sigs/external-dns
- For HTTPS/TLS, you can connect your ingress controller to https://cert-manager.io so that it can automatically get certificates for each service. You can install it using Helm or a downloadable manifest you can apply (see instructions on the website for details on either method). Next, follow the instructions to connect to a CA (e.g. Letsencrypt) and modify your ingress controller settings to list cert-manager. Of course, you’ll also need a publicly-resolvable DNS record for your cluster for this to work.
- ***Cronjobs*** are often used by Web apps to do things like reindexing DBs, maintenance tasks, clearing caches, and so on. Kubernetes has a CronJob resource that can do this. Simply create a manifest that lists the cron schedule, as well as a container it can spawn (e.g. Alpine/Busybox) to perform the commands you specify.
- For persistent block storage needed by containers, you need to create a ***PVC (Persistent Volume Claim)*** to create a resource that doesn’t disappear when you restart your cluster. Many block storage volumes can only attach to 1 pod at time, which makes it difficult to scale. Rook/Ceph and GlusterFS can do this but are complex to configure. NFS is a simple method that can be used to access persistent block storage. To use NFS, install and configure the NFS Client Provisioner and configure it to connect to an NFS share on another container or NFS server. If the block storage is only used for hosting databases, there are many different persistent and cloud native solutions (e.g. CockroachDB) available on the market that may be worth the money depending on your use case.
- ***Namespaces*** limit the scope of resources in Kubernetes. I’ve been using the default namespace for everything so far, but you typically create namespaces for related resources that comprise a Web app. You can use kubectl create namespace lala to create a lala namespace, and add -n lala to other kubectl commands to limit their functionality to that namespace. If you run kubectl delete namespace lala, all resources associated with that namespace will also be deleted.

