# KUBERNETES
* Kubernetes, also known as "K8s", is an open-source container orchestration platform developed by Google.
* it is designed to automate the deployment, scaling, and management of containerized applications across a cluster of nodes.
* Kubernetes provides a consistent and reliable way to manage applications, regardless of whether they are running on-premises, in the cloud, or in hybrid environments. 
#### CLUSTER
* **MASTER**
    * Only gives instructions
    * if you dont have workers master has to do everything
    * it also known as control-plane
* **WORKER**
    * they will do work
#### MINI KUBE
* it creates a single node cluster inside the VM.
* it is good for beggainers to  learn kubernetes since you dont have to create a master and worker node to create a cluster.
* we can practice basic kubernetes function and also install kubernetes dashboard onit
#### kubeconfig 
* it is the files contains authentication information to connect kubernetes cluster
* if it is not there we have to setup ```~/.kube/config```
* **kubectl**
* it is kubernetes controler
#### kubernetes commands
* to connect kubernetes cluster we use below command
```
kubectl 
```
* kubectl will check automatically a file
```
~/.kube/config
```
* to connect with nodes with master use below command
```
kubectl get nodes
```
* kubernetes contains lot o resources
* to see all the existing namespaces use below command
```
kubectl get namespaces
kubectl get ns
```
* create resources use below command, if you use this once again, you get error
```
kubectl create -f <your.yamlfile>
```
* if not created it will create, if you use again any changes it will update or it will say unchanged. it won't say any error
```
kubectl apply -f <your.yamlfile>
```
* to see the created or existing resources use below command
```
kubectl get <resource_name>
```
* to see the created or existing pods use below command
```
kubectl get pods
```
* to delete the resources use below command
```
kubectl delete -f <your.yamlfile>
```
* to find the resource in particular namespace use below command
```
kubectl get <resource_name> -n <namespace_name>
```
* to login to specific container use below command
```
kubectl exec -it <namespace-name> -c <container_name> --bash
```
* to known the full information of a label use below command
```
kubectl describe pod <label-name> 
```
##### Workload components
1. Namespace
    * it is like a project in your kubernetes cluster to provision your project resources. it is isolated 
    * in kubernetes every resource is YAML
```yaml
apiVersion: v1  # mostly it is v1
kind: pod
metadata: # it is nothing but information
  name: hello-pod
```
2. PODs
    * In Kubernetes, a "pod" is the smallest and most basic unit of deployment.It represents a single instance of a running process within the cluster.
    * A pod encapsulates one or more containers, storage resources, network configurations, and other options required to run a specific set of containers together.
    * Pods are typically created and managed using higher- level abstractions such as Deployments, ReplicaSets, or StatefulSets, which provide additional features like scaling, rolling updates, and self-healing capabilities.
    * Pods provide several benefits, including resource isolation, flexible deployment strategies, easy scaling, and enhanced reliability.
    * it is a space where your containers will run
    * **pod Vs container** 
    * A pod can contain multiple containers, it is the smallest thing in kuberenetes
    * pod is a collection of containers
```yaml
apiVersion: v1  # mostly it is v1
kind: pod
metadata: # it is nothing but information
  name: hello-pod
```
3. spec
    * it is nothing but specifications
    * it gives the information about our pod 
    * to login to a specific container use below command
```yaml
apiVersion: v1  # mostly it is v1
kind: pod
metadata: # it is nothing but information
  name: hello-pod
spec:
containers: # list of containers      
- name: container-1 # container1
    image: nginx 
    ports:
    - containerport: 80
- name: container-2 # container2
    image: almalinux:8 
    command: ["sleep","100"]
```
4. labels
* it is used to select other kubernetes resources
5. annotation
* annotaions ==> no limit on length and special charecters also can be used...
* **labels vs annotaions**
    * labels ==> have some limitation on the length and charecters of key and values
    * annotaions ==> no limit on length and special charecters also can be used...
    * labels are used to select other kubernetes resources.
    * annotaions are used to select external resources to kubernetes.
* **syntax**
```yaml
apiVersion: v1  # mostly it is v1
kind: pod
metadata: # it is nothing but information
  name: labels-demo
  labels:   
    key: <key-name>
    value: "give-value"
  annotation:
    jenkinsurl: "https://jenkins.com"  
```
* **genral kubernetes syntax**
```yaml
piVersion: v1  # mostly it is v1
kind: pod
metadata: # it is nothing but information
  name: hello-pod
  namespace: roboshop
  labels:
    course: aws
    trainer: shiva
spec:
  containers: # list of containers      
  - name: container-1 # container1
    image: nginx 
    ports:
    - containerport: 80
  - name: container-2 # container2
    image: almalinux:8 
    command: ["sleep","100"]
```
* **where**
    1. image
        * image is pulled from docker hube directly if it is a public image
        * if it is private image we have to push that image to docker hub and then pull it from docker hub
* to pull the private image from docker hub use below command
```
image: <url>/<image:version>
```
