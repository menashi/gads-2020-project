#LAB: Google Cloud Fundamentals: Getting Started with Compute Engine
## OBJECTIVES
	- Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
	- Create a Compute Engine virtual machine using the gcloud command-line interface.
	- Connect between the two instances.

## STEPS:
## 1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
	- To create the VM instance called my-vm-1
		gcloud beta compute --project=qwiklabs-gcp-03-d58112a0b586 instances create my-vm-1 --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=347301316199-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=my-vm-1 --reservation-affinity=any
	- Set the firewall rule that allows HTTP traffic
		gcloud compute --project=qwiklabs-gcp-03-d58112a0b586 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

## 2. Create a Compute Engine virtual machine using the gcloud command-line interface.
	- To display a list of all the zones in the region us-central1
		gcloud compute zones list | grep us-central1
	- To set your default zone, to the zone you wouldve selected.
		gcloud config set compute/zone us-central1-b
	- To create the VM instance called my-vm-2
		gcloud compute instances create "my-vm-2" \
			--machine-type "n1-standard-1" \
			--image-project "debian-cloud" \
			--image "debian-9-stretch-v20190213" \
			--subnet "default"

## 3. Connect between the two instances.
## Use PING command to reach the target VM
	- From my-vm-2
		gcloud beta compute ssh --zone "us-central1-a" "my-vm-2" --project "qwiklabs-gcp-03-d58112a0b586"
	- Ping my-vm-1 from my-vm-2
		ping -c 4 my-vm-1
		
	- Use SSH command to connect to my-vm-1 and use command prompt
		ssh my-vm-2
	- Install the Nginx web server
		sudo apt-get install nginx-light -y
	- Use the nano text editor to add a custom message to the home page of the web server
		sudo nano /var/www/html/index.nginx-debian.html
	- Use the arrow keys to move the cursor to the line just below the h1 header, add text like this, with your name
		Hi from menashi
	- Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor, 
	- Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command
		curl http://localhost/
		- Result:
			The response will be the HTML source of the web server's home page, including your line of custom text.
	- To exit the command prompt on my-vm-1, execute this command
		exit
	- You will return to the command prompt on my-vm-2
	- To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command
		curl http://my-vm-1/
		- Result:
			The response will again be the HTML source of the web server's home page, including your line of custom text.
			
## Copy the External IP address for my-vm-1 
	gcloud computeinstances list --zone us-central1-b
## Paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text.
	- Result
		You will see your web servers home page, including your custom text.
