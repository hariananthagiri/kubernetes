# KUBERNETES
* it is Platform As A Service(PAAS)
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
* kubernetes contains lot of components to see all existing components use below command
```
kubectl get <component-name>
```
* to watch the status of component use below command
```
watch kubectl get <component-name>
```
* to watch the status of pods use below command
```
watch kubectl get pods
```
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
* to find the pods in particular namespace use below command
```
kubectl get <pod_name> -n <namespace_name>
kubectl get pods -n <namespace_name>
```
* **KUBENS**
* to overcome above problem of giveing -n  install kubens
* **INSTALLATION**
```SH
#!/bin/bash
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```
* after installing kubens use below command to change and set a new default namespace
```
kubens <namesace-name>
```
* to go back to the default namespace use below command
```
kubens default
```
* to login to single container use below command
```
kubectl exec -it <pod-name> -- bash
```
* to login to specific container if we have muliple containers use below command
```
kubectl exec -it <pod-name> -c <container_name> -- bash
```
* to known the full information of a label use below command
```
kubectl describe pod <pod-name> 
```
* to see the ful information of pods like name, ip-address, nodes, 
```
kubectl get pod -o wide
```
##### Workload components
1. Namespace
    * it is like a project in your kubernetes cluster to provision your project resources. it is isolated 
    * in kubernetes every resource is YAML
```yaml
apiVersion: v1  # mostly it is v1
kind: Namespace
metadata: # it is nothing but information
  name: hello-pod
```

2. PODs
    * In Kubernetes, a "pod" is the smallest and most basic unit of deployment.It represents a single instance of a running process within the cluster.
    * A pod encapsulates one or more containers, storage resources, network configurations, and other options required to run a specific set of containers together.
    * Pods are typically created and managed using higher- level abstractions such as Deployments, ReplicaSets, or StatefulSets, which provide additional features like scaling, rolling updates, and self-healing capabilities.
    * Pods provide several benefits, including resource isolation, flexible deployment strategies, easy scaling, and enhanced reliability.
    * it is a space where your containers will run
    * pod name must be uniq if we use same name it through an error
    * **pod Vs container** 
    * A pod can contain multiple containers, it is the smallest thing in kuberenetes
    * pod is a collection of containers
```yaml
apiVersion: v1  # mostly it is v1
kind: Pod
metadata: # it is nothing but information
  name: hello-pod
```
* to create pod in specific namespace use below syntax
```yaml
apiVersion: v1  # mostly it is v1
kind: Pod
metadata: # it is nothing but information
  name: hello-pod
  namespace: <namespace-name>
  ```
  * if we won't mention it is created in default namespace
* **RESOURCES IN POD**
* VM vs containerisation
* VM --> 2GB 2CPU --> resources are blocked irrespective of usage
* containerisation --> containers don't block resources, they are used dynamically
* we can restrict the resources consumed by containers...
**NOTE**
  * in pod we use configmap and secretes by using env 

3. spec
    * it is nothing but specifications
    * it gives the information about our pod 
    * to login to a specific container use below command
```yaml
apiVersion: v1  # mostly it is v1
kind: Pod
metadata: # it is nothing but information
  name: hello-pod
spec:
containers: # list of containers      
- name: container-1 # container1
  image: nginx 
  ports:
    - containerPort: 80
- name: container-2 # container2
  image: almalinux:8 
  command: ["sleep","100"]
```

4. labels
  * it is used to select other kubernetes resources
  * lables are used in matadata section
* use below syntax for lables
```yaml
lables:
key: <key-name>
value: "<key-value>"
```
* to provide multiple lables use below syntax
```yaml
lables:
key1: "<value1>"
key2: "<value2>"
```
* to known the full information of a label use below command
```
kubectl describe pod <pod-name> 
```
* to see all the labels available use below command
```
kubectl get pods --show-labels
```

5. annotations
  * annotaions ==> no limit on length and special charecters also can be used...
  * annotations are used in matadata section
* use below syntax for annotation
```yaml
annotations:
  key: "value"
```
* to provide multiple annotations use below syntax
```yaml
annotations:
  key1: "<value1>"
  key2: "<value2>"
```
  * **labels vs annotaions**
      * labels ==> have some limitation on the length and charecters of key and values
      * annotaions ==> no limit on length and special charecters also can be used...
      * labels are used to select other kubernetes resources.
      * annotaions are used to select external resources to kubernetes.
* **syntax**
```yaml
apiVersion: v1  # mostly it is v1
kind: Pod
metadata: # it is nothing but information
  name: labels-demo
  labels:   
    key: <key-name>
    value: "give-value"
  annotations:
    jenkinsurl: "https://jenkins.com"  
```

6. env
* we can use these environment variables with in the container
* go to the container and apply **env** command to see all stored env variables
```yaml
spec:
  # list of containers
  containers:
  - name: hello-pod
    image: nginx
    env:
    - name: DEMO GRRETING
      value: "hello from the environment"
```

7. resources
  * to reques the cpu and memory we use it
  * we contains two types here requests and limits
  * **requests** is soft reuest **limit** it is heard request both will have memory and cpu options
  * **LIMIT >= REQUEST**
  * **syntax**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
spec:
  # list of containers
  containers:
  - name: hello-pod
    image: nginx
    #here with this line port will not be opened, just for information
    ports:
    - containerPort: 80
    resources:
      requests:
        cpu: "100m"
        memory: "68Mi"
      limits:
        cpu: "200m"
        memory: "128Mi"
```
* where mb is known as Mi here and m is for microns in Mi m mut be capital
* limit is always greter than or equal to request [limit >= request]

8. ConfigMap Of Pod
  * it is used to store the configureation in the form of key value pair
  * to create configmap use below syntax & kubectl create command
* **SYNTAX**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <configmap-name>
data:
  key1: "value1"
  key2: "value2"
  key3: "value3"
```
* to see the available configmaps use below command
```
kubectl get configmaps
```
* to inspect or describe configmaps use below command
```
kubectl describe configmap <configmap-name>
```
* to refer the configMap from pod use the below syntax
* **SYNTAX**
```yaml
env:
- name: course1 # user defined name you casn can as you like
  valueFrom:
    configMapKeyRef: # M, K, R are capital
      name: configMap-name
      key: key-name1
- name: course2 # user defined name you casn can as you like refereing key2
  valueFrom:
    configMapKeyRef:
      name: configMap-name
      key: key-name2
# in the same way if i pass 10 key values the gode get incresed to avide that use below syntax
```
* by using above syntax our code gets incresed by more no of lines
* to avide this configmaps provide other future for it as shown in below
```yaml
envFrom:
  - configMapRef:
    name: <configmap-name>
```
* we can also mount a file using configmap instad of passing key value pair
* give a key and entire file data has value 
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: <configmap-name>
data:
  key-name: |
    file-data
```
* to do that we have to mount the data to valumes
9. valumes
  * we have to cretev a valume at containers level 
  * we can mount this valumes all the containers available if we want to mount
* using configmap in volumes 
```yaml
volumes:
- name: nginx-conf
  configMap:
    name: web
    items:
      - key: nginx.conf
        path: nginx.conf
```
* volumes is list type we can create n no of volumes as above

* to do volumemount use below syntax
```yaml
volumeMounts:
- name: <volumemount-name>
  mountPath: /etc/nginx/nginx.conf
  subPath: nginx.conf
  readOnly: true
```
* volumemount is a list type we can mount n no of volumes like above 
* in mount M must be capital in Path P must be capith in readOnly O must be capital in syntax
* **volume-name = volumemount-name** they both must have same name
* **ACCESS MODES**
1. readOnly 
2. ReadWriteOnly
3. ReadWriteOnce
4. ReadWriteMany
5. ReadWritePod
* to create valumes and mountvolumes use below syntax
```yaml
containers:
  - name: web
    image: joindevops/web:v1
    imagePullPolicy: Always
    volumeMounts:
    - name: <volume-name>
      mountPath: /etc/nginx/nginx.conf
      subPath: nginx.conf
      readOnly: true
    volumes:
  - name: <volume-name>
    configMap:
      name: web
      items:
        - key: nginx.conf
          path: nginx.conf
```

10. Secret Of Pod
  * it is used to store the confidential or sensitive data secretly like username, password, e.t.c...
  * to crete secretkeys we use below syntax and kubctl create command
  * it has to start stright from matadata section and no spec: section if we give it through an error
* **syntax**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: <secret-key name>
  namespace: <namespace-name>
type: Opaque
data:
  key1: "value1"
  key2: "value2"
```
* where
  * Opaque - it is nothing but you can not see 
* by using above syntax our code gets incresed by more no of lines
* to avide this secret provide other future for it as shown in below
```yaml
envFrom:
    - secretRef:
        name: <secretkey-name>
```
* to refer the secret keys using pod use below syntax
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-pod
spec:
  # list of containers
  containers:
  - name: secret-pod
    image: nginx
    #here with this line port will not be opened, just for information
    ports:
    - containerPort: 80
    envFrom:
    - secretRef:
        name: devops-secret
```
* we will use base 64 encoded values here 
* to genrete the usernameand password we use below command
```
echo -n "amdin" | base64
o/p :- YWRtaW4=
```
* to decode the values use below command
```
echo -n "YWRtaW4=" | base64 --decode
o/p :- amdin
```
* but the above mehod is not secured it was encoded not encrypted

11. Services
  * it used to communicate with pod
  * to achive pod to pod communications we use services
  * lables are very usefull components in kubernetes they are used as selecters 
  * we use lables to connect with services
  * if you want to expose pods to other applications or outside we must use services..
    * expose to other apps or outside world
    * load balancing
    * service mesh
  * To create services use below syntax
* **SYNTAX**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: <service-name>
spec:
  type: ClusterIP/NodePort/LoadBalancer
  selector:
    lable1: value1
    label2: value2
  ports:
  - protocol: TCP
    port: 80 #service-port
    targetPort: 80 #container-port
```
* services starts in spec: section it contains type, selector, and ports 
* port is list type it contains portocal port and targetport
* to see the services available use below command
```
kubectl get services
kubectl get svc (shortcut for service)
```
* if we perform above command it gives a cluster-ip but it works with in the kubernetes cluster it is purely internal
* where
  * selector ==> it specify the service where to get attached 
    * in selectors we give labele key : key-name, value: key-value in selector option and this source get attached to that lables pod
  * service port ==> user defind port we can give what ever we want
  * target port ==> it is nothing but container-port 
  * if anyone hits the service port in port-no:80 it send the request to container port or target-port of port-no:80 
* we have 3 types of services they are:
  1. ClusterIP
    * purely internal to kubernets
    * by default it takes cluster-ip
    * when we use clusterip it provides an ip address it will change frequently instad of using that use service-name
    * we can repalce ip-address with service-name because service-name is perminent it wont change
  * to get ip-address use below command
```
kubectl get services
```

  2. NodePort
    * ClusterIP is a subset of NodePort
    * you can expose to outside world
  3. LoadBalancer
    * NodePort is a subset of LoadBalancer
    * you can expose to outside world
* **NOTE**
  * By using type we can decide which type of service we want
  * if we wont specify type component by default it takes ClusterIP
```
type: ClusterIP/NodePort/LoadBalancer
```

* **Sets**
* basically sets are starts from spec: section 
12. ReplicaSet
  * it is a set of pods to create multiple pods at a time we use ReplicaSet
  * pod is a subset of ReplicaSet
  * here lables plays a key role 
  * the syntax contains two parts replicaset defination  + pod defination
  * to crete a ReplicaSet use below syntax 
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
  labels:
  app: nginx
  tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector: # replica-set labels
    matchLabels:
      app: nginx
      tier: frontend
  template: # pod-definition
    metadata:
      labels:
        app: nginx
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx:mainline-alpine3.18-perl
```
* to see all existing ReplicaSet use below command
```
kubectl get replicaset
kubectl get rs (shortcut ReplicaSet)
```
* name for ReplicaSets which we created using above syntax is created in below formate
```
ReplicaSet-<random-id>
```
* **matchLabels**
  * it is used to match with other lables it compires two different labels
  * the condition here is the reference and matchLabels must be same if not it through an error if matchLabels and reference lables are not same
```yaml
matchLabels:
  key:value
```
* **NOTE**
* ReplicaSet is not usefull in the changeing or updateing the version of the application it is used only to maintain the replicas to overcome this we have a component called **Deployment**
* cluster-ip < node-port < load-balancer
* pod < replicaset < deployment

13. Deployment
  * pod and replicasets are subsets of deployment
  * genrally it is known as deployment-set
  * it is used to update the app version dynamically
* **Deployment ==> replicaset ==> pods**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
    demo: deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      demo: deployment
  template:
    metadata:
      labels:
        app: nginx
        demo: deployment
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
    demo: deployment
  ports:
  - protocol: TCP
    port: 80 #service-port
    targetPort: 80 #container-port
```
* **Genral Kubernetes Syntax**
```yaml
apiVersion: v1  # mostly it is v1
kind: Pod
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
    - containerPort: 80
    resources:
      requests:
        cpu: "100m"
        memory: "68Mi"
      limits:
        cpu: "200m"
        memory: "128Mi"
  - name: container-2 # container2
    image: almalinux:8 
    command: ["sleep","100"]
    env:
    - name: DEMO GRRETING
      value: "hello from the environment"
    env:
    - name: course
      valueFrom:
        configMapKeyRef:
          name: devops-config
          key: course
    envFrom:
    - secretRef:
        name: devops-secret
```
* **where**
    1. image
        * image is pulled from docker hube directly if it is a public image
        * if it is private image we have to push that image to docker hub and then pull it from docker hub
* to pull the private image from docker hub use below command
```
image: <url>/<user-name>/<image:version>
```
* **NOTE**
  * In kind first letter must be capital letter [A-Z]
    ``` kind: [A-Z]a-z```
  * in containerPort **P** must be capital 
  ```- containerPort```
  * in apiVersion **V** is capital
  ```apiVersion```
  * most of the components and commands are ends with **s**
* **NAMESPACE ===> PODS ===> CONTAINERS**

#### EKSCTL
* eks is amazon elastic kubernetes service
* it is nothing but eks control
* we can create eks cluster by using below command
```
eksctl create cluster --config-file=your.yamlfile
```
* we can delete eks cluster by using below command
```
eksctl delete cluster --config-file=your.yamlfile
```
###### IMAGE PULL POLICY
1. **IfNotPresent**
  * the image is pulled locally only if it is not alrady present locally
  * if the image is not exist in the node then only it will pull if it exist then it won't pull

2. **Always**
  * if the image exist or doesn't exist in the node it doesn't matter everytime it pull the image irrespective of presence of image
  * it is recommended to give Always
3. **Never**
    * if the image exist or doesn't exist in the node it doesn't matter everytime it won't pull the image irrespective of presence of image
    * it wont pull the image
* **SYNTAX**
```yaml
spec:
  containers:
    imagePullPolicy: Always/Never/IfNotPresent
```
* **NOTE**
  * when we use telnet command to come out of it use below command
```
ctrl + ]
```
* containers is list type and env is list type 
```yaml
containers:
- name: list1
  env:
  - name: list1
```
* to run two different programs in a single code use below syntax 
```yaml
pgm1
---
pgm2
```
* metadata contains labels, annotations, 
* spec contains containers it is list type, env, rersources, 
* createing indivusuals i.e configmap which contain data, secret which contains type, data, service, volumes these are list type
* combineing with pod defination is replicaset, deployment
* **NOTE**  
  * the main components in code are apiVersion, kind, metadata, spec
  * all the components are used in kubernetes are mostly used to provide additional data like lables, annotations, in metadata section
  * remaining components used in spec it contains data element too if it is secret it add type too
  * genrally we give kubernetes files as mainifest.yaml

* **STATEFUL APPLICATIONS**
  * Application which storage is mandatory are called stateful applications

* **STATELESS APPLICATIONS**
  * Application which doent have any storage are called stateless applications
  * stateless applications are not have any storage on there own

* data is getting stored in EC2 worker nodes, nodes are also ephemeral.
* if you start storeing the crusial data in ec2 worker nodes because they are temparary we may loss the data any time
* so we should not take the risk of storeing staeful applications data in ec2 worker node in this case we shold have an external storage 

* **FILEBEAT**
* in sidecar we will have filebeat it is used to ship node logs
    * **filebeat --> public image from elastic search**
    * there is some configuration for filebeat i.e i/p and o/p
      * i/p ==> is what logs to access
      * o/p  ==> is where to ship the logs
* firebeat starts from metadata and data section as given in below syntax
* **SYNTAX**
```YAML
data:
  filebeat.yml: |
    filebeat:
      config:
        modules:
          path: /usr/share/filebeat/modules.d/*.yml
          reload:
            enabled: true
      modules:
      - module: nginx
        access:
          var.paths: ["/var/log/nginx/access.log*"]
        error:
          var.paths: ["/var/log/nginx/error.log*"]
    output:
      elasticsearch:
        hosts: ["elasticsearch:9200"]
```
* **K8S VOLUMES**
* the below two types are stores data in worker nodes or intenal volumes or ephemeral(i.e temparary) volumes
  1. **emptyDir**
    * used for sidecar patterns, we mount the volume to sidecar and main container using emptyDir. then sidecar get access to storage
* **SYNTAX** 
```YAML
volumes:
- name: nginx-logs
  emptyDir: {}   
```
* we just mount that emptyDir volume volume mount name and emptyDir volume name must be same we need to give same name for both volume mount and emptyDir volume

  2. **hostPath**
    * we need to ship all the worker node logs to external system. 
  * **DeamonSet**
    * will make sure a pod runs in each and every node.. fluentd will be deployed as deamonset that can access the underlying host logs and send them to ELK
    * make sure that your using **namespace: kube-system** we have to use kube-system namespace only because it contains administatory namespace 
* the syntax for hostpath is given below
```yaml
volumes:
- name: varlog
  hostPath:
    path: /var/log
```
* the syntax for deamonset is mentioned below
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
    version: v1
spec:
  selector:
    matchLabels:
      k8s-app: fluentd-logging
      version: v1
```
* **ACCESS MODES**
  * accessModes are list type starts from spec section
1. readOnly 
2. ReadWriteOnly
3. ReadWriteOnce
4. ReadWriteMany
5. ReadWritePod

* **STORAGE**
* we have two types of storage systems in AWS 
  1. EBS (Elastic Block Store)
    1. Static ebs
    2. dynamic ebs
  2. EFS (Elastic File System)
    1. Static efs
    2. dynamic efs
1. **EBS (Elastic Block Store)**
  * it is like hard disk
  * while createing volumes in aws the rule is valume is completely depends on server available zone
  * EBS volume in ZONE specific
    * **VOLUME = SERVER AZ**
  * EBS uses only **ReadWriteOnce** mounting access mode only because it is hard disk

* **prerequisites**
* we need to make this volume available to k8 cluster. we should install drivers. 
  * aws-ebs-csi drivers should be installed
  * a proper role should be attached to ec2 instance to access EBS.

* **EBS Drivers installation**
  * search aws ebs csi drivers on chrome
  * you can see a git hub account then go to documentation and click on drivers installation
  * there is an option called Kustomize it contain below command
```
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.35"
```
* aws-ebs-csi drivers should be installed
* these are perminent volumes
1. static provisioning
  * first we need to create storage or volume, either storage admin or k8 admin will create the storage
  * we need to make this volume available to k8 cluster. we should install drivers. 
  * aws-ebs-csi drivers should be installed
  * a proper role should be attached to ec2 instance to access EBS.
* **Drivers installation**
  * CSI(container storage interface) drivers
  * search kubernetes volumes on chrome click on volumes option 
  * now search for ebs click on ebs driver third party application
  * you goto an git hub account then go to documentation and click on drivers installation
  * there is an option called Kustomize it contain below command
```
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.35"
```
* aws-ebs-csi drivers should be installed
* container storage interface(csi)
* **kubernetes volume objects/resources**
1. **persistant volume(PV)**        
  * it represents the external storage. it is like a raper on volume
  * create a volume and copy its volume id
* **SYNTAX**
```YAML
# persistence volume creation
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ebs-static
spec:
  accessModes:
  - ReadWriteOnce #EBS volume should have readwriteonce, because it HD.
  capacity:
    storage: 10Gi
  csi:
    driver: ebs.csi.aws.com
    fsType: ext4 # (file system)
    volumeHandle: vol-0da1eaeda19787d1b
```
2. **persistant volume claim(PVC)** 
  * pods should request volume through pvc. we pod requied storage they need to claim
```YAML
# Persistent Volume Claim creation
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ebs-static
spec:
  storageClassName: "" # Empty string must be explicitly set otherwise default StorageClass will be set. for static provisioning this is empty
  volumeName: ebs-static
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```
* Attaching pvc to the pod use below syntax
```yaml
# attaching static-pvc to pod
apiVersion: v1
kind: Pod
metadata:
  name: app
  labels:
    demo: ebs-static
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-data
      mountPath: /usr/share/nginx/html
  nodeSelector:
    zone: 1b
  volumes:
  - name: nginx-data
    persistentVolumeClaim:
      claimName: ebs-static
```
* **POD ===>cs PVC ===> PV ==> STORAGE**
* **NOTE**
* EBS volume in ZONE specific our is pod is created in any of working node 
* if i have 3 working nodes one node is in 1a and other node is in 1b and last node is in 1c 
* my vm in in 1a zone i need to create the volume in 1a zone only if not it through an error but now pod may create in any of the worker node
* in this case we have to control that pod have to create in our requied region to control this in kubernetes we have a component called **kubecontroler**
* **kubecontroler** decide pod has to create in which working node 
* TO OVERCOME THIS we have to use **nodeSelector**
* **nodeSelector**
  * to is used to control where our pod should go
  * to do this we have to **LABEL OUR NODES** 
```
kubectl get nodes (to see pod-names)
kubectl label nodes <pod-name> key-name=value
```
* now to mention which node to use using nodeSelector use below command
```yaml
nodeSelector:
  key-name: value # what we labeled using above command
```
* nodeSelector used in spec section
* it is the basic option
* **where pod is created? ===> pod is created in working nodes**

2. **dynamic provisioning**
  * volume should be created automatically.
  * there is another object called storageClass that can create storage dynamically based on the request.
  * for dynamic we install storage class
  * here external volume and pv would be created automatically by storageClass..
* **STORAGECLASS**
  * here external volume and pv would be created automatically by storageClass.
  * since storageClass is cluster level resource, admins should create this.
  * use below syntax to create storage class
  * **SYNTAX**
```YAML
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-sc
provisioner: ebs.csi.aws.com
reclaimPolicy: delete/retain # by default it is in delete
volumeBindingMode: WaitForFirstConsumer
```
* **where**
  * provisioner ==> it is used to supply drivers
  * **reclaimPolicy**
    * delete ==> if pod is deleted volume also gets deleted
    * retain ==> if pod is deleted volume is not gets deleted
    * default option is delete
* to see all the storage classes use below command
```
kubectl get sc
```
* in kubernetes we have two types of resources
  1. namespace resource
  2. non-namespace resource
1. **namespace resource**
  * the resources created in the namespace
2. **non-namespace resource** 
  * the resource created without namespace i.e cluster level resource
* to see the all namespace and non-namespace resources use below command
```
kubectl api-resources 
```
* **NOTE**
* persistent volume is the physical thing
* persistent volume claim is the request

2. **EFS (Elastic File System)**
  * it is like nfs or drive
  * external volumes or persistent volumes
  * EFS size for single file is 47.9TIB
  * EFS depends on NFS the port number of NFS is 2049
  * goto EFS edit onbount rules give NFS and port no 2049 and attach our instance security group id
* **BBS Drivers installation**
  * aws-efs-csi drivers should be installed
  * search aws efs csi drivers on chrome
  * you goto an git hub account then go to documentation and click on drivers installation
  * there is an option called Manifest (private registry) it contain below command
```
kubectl kustomize \
    "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-2.0" > private-ecr-driver.yaml
```
* it will genrate a yaml file
* after gnrateing the file use below command
```
kubectl apply -f private-ecr-driver.yaml
```
* give ec2 role EFS full access

  1. **EFS static**
    * create EFS filesystem
    * edit the security group, so that it will allow port 2049 from worker nodes.
    * we should have drivers installed as shown in above
    * after installing drivers do the below steps
    * create pv
    * create pvc
    * use pvc in pod
* **EFS static PV**
* **syntax**
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: efs-static
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
  persistentVolumeReclaimPolicy: Retain
  csi:
    driver: efs.csi.aws.com
    volumeHandle: fs-016a860b1f7383cac
```

* **EFS static PVC**
* **syntax**
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: efs-static
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: ""
  volumeName: efs-static
  resources:
    requests:
      storage: 5Gi
```

* **PVC IN POD**
* **syntax**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
  labels:
    demo: efs-static
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-data
      mountPath: /usr/share/nginx/html
  nodeSelector:
    zone: 1b
  volumes:
  - name: nginx-data
    persistentVolumeClaim:
      claimName: efs-static
```
2. **EFS Dynamic**
  * to do dynamic we requied StorageClass
  * EFS access points are created dynamically
  * to create access point dynamically attach policy of amazon elastic file system full access 
  * give ec2 role EFS full access
* **storageClass**
* **SYNTAX**
```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: efs-sc-roboshop
provisioner: efs.csi.aws.com
parameters:
  provisioningMode: efs-ap
  fileSystemId: fs-0715ef3ca61bfe431
  directoryPerms: "700"
  gidRangeStart: "1000" # optional
  gidRangeEnd: "2000" # optional
  basePath: "/roboshop" # optional
```

* **PVC**
* **SYNTAX**
```YAML
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: efs-dynamic
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: efs-sc-roboshop
  resources:
    requests:
      storage: 5Gi
```

* **PVC IN DYNAMIC**
* **SYNTAX**
```YAML
apiVersion: v1
kind: Pod
metadata:
  name: efs-dynamic
  labels:
    demo: efs-dynamic
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - name: nginx-data
      mountPath: /usr/share/nginx/html
  volumes:
  - name: nginx-data
    persistentVolumeClaim:
      claimName: efs-dynamic
```
# HELM CHART
* templatise the kubernetes manifests
* package manager for kubernetes
* to parameterise the values used in manifest.yaml we use helm sheet

* **INSTALLATION**
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
### Helm Commands
* to install anything in helm chart we use below command
```
helm install <app-name> .
```
* to see the list of all components we did with helm we use below command
```
helm list
```
* after changeing values to upgrade use below command
```
helm upgrade <app-name> .
```
* to see the history how many times we deployed in helm use below command
```
helm history <app-name>
```
* to do the rollback in helm use below command
```
helm rollback <app-name>
```
* to goto particular revision in helm use below command
```
helm history relese revision
```
1. Chart.yaml
    * the first step is helm chart is Chart.yaml as shown in below
```yaml
apiVersion: v1
name: nginx
version: 0.0.2 # this the chart version
description: this is the customised helm chart for nginx
appVersion: 1.0.1 # apk version
```
2. templates
    * we definately need to give a folder named templates in helm charts all deploymentand .yaml file are automatically executed here
3. values.yaml
    * we maintain values.yaml file in root folder to import and export values 
* we declere a variable as below
```
* for number use below syntax
{{ .Values.<section-name>.<variablename> }}
```
* for alpha numaric or text use below syntax
```
"{{ .Values.<section-name>.<variablename> }}"
```
* if we give quote there while passing the value in values.yaml there also we need to give quote
* **example**
```
"nginx:{{ .Values.deployment.imageVersion}}"
```
* in values.yaml ile we pass as below
```yaml
deployment:
  replicas: 3
  imageVersion: mainline-alpine3.18-perl
```
* we can set the values.yaml file values at command line by using below command
```
--set key=value
<command> --set deployment.replicas=10
helm upgrade nginx --set deployment.replicas=10
```
* to see all the volues stored in volues.yaml file use below command
```
helm show values . 
```
* we have install ebs drivers to our helm folder as below in VM
```
helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver
helm repo update
helm upgrade --install aws-ebs-csi-driver --namespace kube-system aws-ebs-csi-driver/aws-ebs-csi-driver
helm list -n kube-system
```
* to uninstall the driver use below command
```
helm uninstall aws-ebs-csi-driver -n kube-system
```
#### Statefulset
* it is used to create stateful applications in kubernetes
* podes inside stateful applications should have static hostnames to communicate with other nodes
* stateful applications should follow orderly provisioning as well as terminaete 
* statefulset applications will have headless service.
* it definetly requied PV or PVC
* **pre-requities**
  1. install drivers
  2. provide IAM role access to ec2
  3. install storage class
```
helm upgrade - install aws-ebs-csi-driver\- namespace kube-system \aws-ebs-csi-driver/aws-ebs-csi-driver
```
* give AmazonEbcsDriverPolicy permisiion 
* **deployment**   
    * it is used to create stateless (i.e no own database) applications in kubernetes
* **headless service**
  * in database cluster to send data from one replica to other replica we shold use headless service

* **NOTE**
* ***RUNNING DATABASES INSIDE THE KUBERNETES CLUSTER IS VERY RISKY***
