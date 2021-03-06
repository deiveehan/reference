## VPC Creation
```
gcloud compute --project=wise-dispatcher-238019 networks create ctrl-space-vpc --description="VPC for holding common infrastructure components such as jenkins, docker etc.," --subnet-mode=custom
```
## Subnet creation
```
gcloud compute --project=wise-dispatcher-238019 networks subnets create default-subnet --network=ctrl-space-vpc --region=us-central1 --range=10.128.0.0/20
```

## Cluster creation.

```
gcloud beta container --project "wise-dispatcher-238019" clusters create "ctrl-space-cluster" --zone "us-central1-a" --no-enable-basic-auth --cluster-version "1.12.8-gke.10" --machine-type "n1-standard-1" --image-type "COS" --disk-type "pd-standard" --disk-size "100" --node-labels app=fubar --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --num-nodes "3" --enable-cloud-logging --enable-cloud-monitoring --enable-ip-alias --network "projects/wise-dispatcher-238019/global/networks/fu-vpc" --subnetwork "projects/wise-dispatcher-238019/regions/us-central1/subnetworks/fu-vpc" --default-max-pods-per-node "110" --addons HorizontalPodAutoscaling,HttpLoadBalancing --enable-autoupgrade --enable-autorepair --labels app=infrastructure

gcloud container clusters list
gcloud container clusters describe infra-cluster --zone us-central1-a
gcloud container clusters delete infra-cluster --zone us-central1-a
```

## Install helm
```
brew install kubernetes-helm
kubectl create -f rbac-config.yml
helm init --service-account tiller --history-max 200 --upgrade
```
## Jenkins
```shell script
helm install --name jenkins stable/jenkins --namespace jenkins
printf $(kubectl get secret --namespace jenkins jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
// master.servicePort=80
// master.adminUser=admin
// master.adminPassword=password

// kubectl delete deploy jenkins -n jenkins
```




 
 


