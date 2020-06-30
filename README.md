HelloWorld Google Kubernetes Engine
===================================

Simple example to demonstrate gke deployment.


docker:
=======
* To Build:

    ````gcloud builds submit --tag gcr.io/mytestgkeproject/helloworld-gke .````

    ensure you have gcloud local setup on the system you build. 

Kubernetes Engine Cluster:
==========================

* Cluster
    ````gcloud container clusters create helloworld-gke --num-nodes 1 --enable-basic-auth --issue-client-certificate```` 
    ````--zone europe-west1-b````
    
    Creating cluster helloworld-gke in europe-west1-b... Cluster is being health-checked (master is healthy)...done.
    Created [https://container.googleapis.com/v1/projects/mytestgkeproject/zones/europe-west1-b/clusters/helloworld-gke].
    To inspect the contents of your cluster, go to: 
    https://console.cloud.google.com/kubernetes/workload_/gcloud/europe-west1-b/helloworld-gke?project=mytestgkeproject
    kubeconfig entry generated for helloworld-gke.
    NAME            LOCATION        MASTER_VERSION  MASTER_IP      MACHINE_TYPE   NODE_VERSION    NUM_NODES  STATUS
    helloworld-gke  europe-west1-b  1.14.10-gke.36  34.78.148.106  n1-standard-1  1.14.10-gke.36  1          RUNNING


* Kubernetes

    > kubectl get nodes
                
    NAME                                            STATUS   ROLES    AGE    VERSION
    gke-helloworld-gke-default-pool-fef814c5-h5cx   Ready    <none>   108s   v1.14.10-gke.36
    
    > kubectl apply -f deployment.yaml
    
    deployment.extensions/helloworld-gke created
    
    > kubectl get deployments

    NAME             READY   UP-TO-DATE   AVAILABLE   AGE
    helloworld-gke   1/1     1            1           21s
    
    > kubectl get pods

    NAME                              READY   STATUS    RESTARTS   AGE
    helloworld-gke-576c55d567-5xp5c   1/1     Running   0          51s
    
    > kubectl apply -f service.yaml

    service/hello created
    
    > kubectl get services

    NAME         TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
    hello        LoadBalancer   10.51.243.130   34.77.108.37   80:32525/TCP   13m
    kubernetes   ClusterIP      10.51.240.1     <none>         443/TCP        28m
    
    > curl 34.77.108.37

    Hello World!