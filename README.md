

### task:
install minikube on your mac\win
create docker image that will include nginx and your index.html file

push the image to docker push ctvr-dcr01.int.liveperson.net/proddemo/IMAGE[:TAG]

Create k8s deployment and service to deploy the new image

browse to your website

Inject environment variables to your Pod (container) based on k8s configmap with the key: team , value: PaaS-ops

acess to your pod and print the environment varibales

use confd to template your index.html which will display the value of the team ENV

### solution



```
    
   Note: 
   1.run on my local kubeadm installtion
   
   2.basic image with ngnix and confd  installed was forked from :
   sysboss/docker-nginx-with-confd 
   
   
   //docker build ( confd installed and run first when the pod is up)
   docker build .
    
   //docker tag 
   docker tag ngnix_confd_demo yarinkos/ngnix_confd_demo 
    
   //push to docker hub
   docker push yarinkos/ngnix_confd_demo
    
   //deployment creation  ( with the image from dockerhub
   kubectl create -f dep.yaml create deployment
   
   //configmap creation
   kubectl create configmap special-config --from-literal=team=Pass-ops-CM
   
   //service creation type nodeport 
   kubectl create -f srv.yaml
   
   //get cluster ip
   kubectl get service/my-service
   
   //view the key/vakue team: Pass-ops-CM to check later on the website  
   kubectl get configmaps special-config -o yaml
   
   //browse to the page
   curl 10.107.48.173
   
   //enter into pod container
   kubectl exec -it  nginx-deployment-5f497dd6fd-2k69x /bin/sh 
   then printenv
   
   //(not required )play with changing the env varaible and run confd the reflect it on the website
   export ENV_NAME=FooPaasOpsBar; /bin/confd -onetime -backend env // inject env
   
   
   ```
   

Tmp Testing:

docker exec -it 8e6bed5c7a61 /bin/sh

export ENV_NAME=FooPaasOpsBar; echo $ENV_NAME

/bin/confd -onetime -backend env