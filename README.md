# dump1090-kubernetes
Demo: Run dump1090-mutability on a Kubernetes cluster.  

* Create a deployment of multiple dump1090 instances.
* Create a load balancer service for multiple dump1090 instances with an external IP address.
* Scale up and scale down dump1090 instances.
* Do a rolling update to a new version of dump190.
* Roll back to a previous version.
   
### Download yaml files   
   
Download the yaml files to you Kubernetes cluster management host where you can run kubectl.

* wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-deployment-v1.yaml
* wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-deployment-v2.yaml
* wget https://raw.githubusercontent.com/tedsluis/dump1090-kubernetes/master/dump1090-services.yaml   
   
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
   
### Create service with external IP address for dump1090   
   
This services creates a load balancer for the dump1090 web servers. It also provides a external IP address.
   
````
kubectl create -f dump1090-services.yaml   
````
note: It will take a minut before you get an external IP address!   
   
Check the service:   
   
````
kubectl get services
````
    
````
kubectl describe services dump1090
````
   
### Try dump1090 in you browser   
   
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
  
### Live view
   
Try my dump1090 fork with heatmap and rangeview in the Google cloud: http://130.211.186.77/dump1090/gmap.html
(This dump1090 runs on a 60-day free trail that is available until 20 june 2016, more info at https://cloud.google.com/free-trial/)   
   
Ted Sluis   
ted.sluis@gmail.com   
https://github.com/tedsluis   
https://hub.docker.com/r/tedsluis   
https://www.youtube.com/tedsluis   
http://flightaware.com/adsb/stats/user/tedsluis   
