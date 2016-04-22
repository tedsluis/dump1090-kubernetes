# dump1090-kubernetes
Demo: Run dump1090-mutability on a Kubernetes cluster (in the cloud).  
    
* Create a single pod with a dump1090 instance.
* Create a replication controller with multiple dump1090 instances.
* Create a deployment of multiple dump1090 instances.
* Create a load balancer service for multiple dump1090 instances with an external IP address.
* Scale up and scale down dump1090 instances.
* Do a rolling update to a new version of dump190.
* Roll back to a previous version.   
   
### Prepare a Kubernetes cluster   
   
If you don't like to set up a Kubernetes cluster yourself, you can use a 60 days/$300 free trail on Google Cloud: https://cloud.google.com  
All you need is less than 5 minutes to signup and add your credit card details before you can start.   
Note: After the free trail period your access to the cluster automaticly ends without any payment.  
Start with: https://cloud.google.com/container-engine/docs/before-you-begin or follow this brief instructions:       
  
After selecting 'Container Engine', you can create a new 'Container Cluster'. Choose the defaults is just fine.    
Launch a Google Cloud Shell from your Google Cloud Console (indicated with the green arrow in the picture below).   
Tell the command-line interface which Google Cloud Platform project to use:   
````
gcloud config set project YOUR_GOOGLE_CLOUD_PROJECT
gcloud config set compute/zone YOUR_GOOGLE_COMPUTE_ZONE
gcloud config set container/cluster YOUR_KUBERNETES_CLUSTER_NAME
gcloud container clusters get-credentials YOUR_KUBERNETES_CLUSTER_NAME
gcloud config list
````
   
You can find the correct settings in your Google Cloud Console (as indicated with the red arrows in the picture below).   
[![Google Cloud Console](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/container_cluster.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/container_cluster.png)
   
### Download the demo yaml files   
   
Download the yaml files to your Google Cloud Shell where you can run kubectl.
````
wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-pod.yaml
wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-replication-controller.yaml
wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-deployment-v1.yaml
wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-deployment-v2.yaml
wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-services.yaml   
````
   
Or clone the repo and move the repo directory:   
````
git clone https://github.com/tedsluis/dump1090-kubernetes.git    
   
cd dump1090-kubernetes   
````
   
### Create a single pod   
   
Prove that there are no running pods:   
````
kubectl get pods   
````   
[![Get pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods1.png)
   
Create a single pod with one dump1090 container:   
````
kubectl create -f dump1090-pod.yaml    
````
[![kubectl create -f dump1090-pod.yaml](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/create_pods1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/create_pods1.png)
   
List running pods:   
````
kubectl get pods
````
[![Get pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods2.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods2.png)
   
List details pod:   
````
kubectl describe pods   
````
[![kubectl describe pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/describe_pods1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/describe_pods1.png)   
The pod is running!   
   
### Create service with external IP address for dump1090   
   
This services creates a load balancer for the dump1090 web server(s). It also provides a external IP address.
   
Check to running services:   
````
kubectl get services
````
[![Get services](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services1.png)
   
Create the service:   
````
kubectl create -f dump1090-services.yaml   
````
[![kubectl create -f dump1090-services.yaml](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services1.png)
   
Check the service:   
````
kubectl get services
````
[![Get services](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services2.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services2.png)    
Note: It will take a minute before you get an external IP address!   
    
After a minute:
````
kubectl get services
````
[![Get services](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services3.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services3.png)
Do you see external IP address for dump1090? Use that in your internet browser!    
   
Show details services:   
````
kubectl describe services dump1090
````
[![kubectl describe services dump1090](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/describe_services1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/describe_services1.png)
   
### Try dump1090 in you browser   
   
Check dump1090 in you browser. You may need to refresh you browser a view times!   
````
http://external_ip_address_of_service/dump1090   
````
Note: Everyone on the internet can use this dump1090 now!
   
[![dump1090](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/browser.png)](http://104.197.44.48/dump1090/gmap.html)
   
### Delete the pod 
   
For the next example we first have to delete this our pod:   
````
kubectl delete pods dump1090
````
[![kubectl delete pods dump1090](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/delete_pod1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/delete_pod1.png)   
note: We leave the service with the external IP address as it is for the next examples.   
   
List pods:   
````
kubectl get pods
````
[![get pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods3.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods3.png)   
   
### Create replication controller   
   
Create a replication controller that exists of 3 pods:   
````
kubectl create -f dump1090-replication-controller.yaml
````
[![kubectl create -f dump1090-replication-controller.yaml](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/create_rc1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/create_rc1.png)   
   
List the pods:   
````
kubectl get pods
````
[![get pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods4.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods4.png)   
   
Scale up the replication controller to 5 dump1090 instances:
````
kubectl scale --replicas=5 rc dump1090
````
[![kubectl scale --replicas=5 rc dump1090](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/scale_rc1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/scale_rc1.png)   
You can scale down the number of pods in the same way.  
   
Get the replication controller:   
````
kubectl get rc
````
[![kubectl get rc](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_rc1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_rc1.png)   
Are are 5 pods running!   
   
Every pod has the dump1090 label:   
````
kubectl get pods --show-labels
````
[![kubectl get pods --show-labels](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods5.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods5.png)  

This 'dump1090' label is used by the load balancer to identify pods. That label was attached to the pods and service in the yaml configuration files.   
Note that the dump1090 service is still active.  
All pods are accessable using the external IP address (port 80) dune to the label 'dump1090'.   
The services balances the load over the pods:   
````
kubectl get services
````
[![get services](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services4.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_services4.png)   
   
Check dump1090 in you browser. You may need to refresh you browser a view times!
````
http://external_ip_address_of_service/dump1090
````
   
### Remove the replication controller
   
Remove the replication controller before you start the next demo example:   
````
kubectl delete rc dump1090
````
[![kubectl delete rc dump1090](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/delete_rc1.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/delete_rc1.png)
   
The pods are gone:   
````
kubectl get pods
````
[![get pods](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods6.png)](https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/pics/get_pods6.png)
   
### Deploy dump1090-mutability, version 1   
   
This deployment starts 3 pods with dump1090.
      
````
kubectl create -f dump1090-deployment-v1.yaml   
````
   
Check the deployment:   
   
````
kubectl get pods   
````
   
````
kubectl get deployments   
````
   
````
kubectl describe deployments   
````
    
Check dump1090 in you browser. You may need to refresh you browser a view times!
   
````
http://external_ip_address_of_service/dump1090
````
   
### Rolling update: deploy dump1090-mutability version 2   
  
This deployment replaces 3 pods with an other version of dump1090.
    
````  
kubectl apply -f dump1090-deployment-v2.yaml   
````
   
Check the deployment:   
   
````
kubectl get pods   
````
   
````
kubectl get deployments   
````
   
````
kubectl describe deployments   
````
   
Check dump1090 in you browser. You may need to refresh you browser a view times!   
   
````
http://external_ip_address_of_service/dump1090   
````
   
### Rolling back to dump1090-mutability version 1   
  
This deployment replaces 3 pods with version 1 of dump1090.
    
````  
kubectl apply -f dump1090-deployment-v1.yaml   
````
   
Check the deployment:   
   
````
kubectl get pods   
````
   
````
kubectl get deployments   
````
   
````
kubectl describe deployments   
````
   
Check dump1090 in you browser. You may need to refresh you browser a view times!   
   
````
http://external_ip_address_of_service/dump1090   
````
   
### sources 
   
Used docker container images:
https://hub.docker.com/r/tedsluis/dump1090-mutability/  (version v1.15_heatmaprangeview and v1.15)   
Github sources:   
https://github.com/tedsluis/dump1090   
https://github.com/mutability/dump1090   
  
### Live view
   
Try my dump1090 fork with heatmap and rangeview in the Google cloud: http://130.211.186.77/dump1090/gmap.html
(This dump1090 runs on a 60-day free trail that is available until 20 june 2016, more info at https://cloud.google.com/free-trial/)   
   
Ted Sluis   
ted.sluis@gmail.com   
https://github.com/tedsluis   
https://hub.docker.com/r/tedsluis   
https://www.youtube.com/tedsluis   
http://flightaware.com/adsb/stats/user/tedsluis   
