# LAB: Google Cloud Fundamentals: Getting Started with GKE
## OBJECTIVES
	- Provision a Kubernetes cluster using Kubernetes Engine.
	- Deploy and manage Docker containers using kubectl.
## STEPS
1. Provision a Kubernetes cluster using Kubernetes Engine.
	- Confirm that needed APIs are enabled
		gcloud services list --available
			Scroll down in the list of enabled APIs, and confirm that both of these APIs are enabled:
				- Kubernetes Engine API
				- Container Registry API
	- Start a Kubernetes Engine cluster
	- Place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE, followed by the assigned zone
		export MY_ZONE=us-central1-a
	- Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:
		gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
	- After the cluster is created, check your installed version of Kubernetes
		kubectl version
	- View your running nodes
		gcloud compute instances list
		
2. Deploy and manage Docker containers using kubectl.
	- Run and deploy a container, launch a single instance of the nginx container. (Nginx is a popular web server.)
		kubectl create deploy nginx --image=nginx:1.17.10
	- View the pod running the nginx container
		kubectl get pods
	 - Expose the nginx container to the Internet
		kubectl expose deployment nginx --port 80 --type LoadBalancer
	- View the new service
		kubectl get services
	- After getting the external IP address from the above command, open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed
		- Result:
			Welcome to nginx! web page
	- Scale up the number of pods running on your service
		kubectl scale deployment nginx --replicas 3
	- Confirm that Kubernetes has updated the number of pods
		kubectl get pods
	- Confirm that your external IP address has not changed
		kubectl get services
	- Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding
		- Result:
			Welcome to nginx! web page