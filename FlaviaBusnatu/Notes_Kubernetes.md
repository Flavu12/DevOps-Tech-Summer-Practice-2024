# Kubernetes
- open source container orchestration tool
- helps you manage containerized applications in different deployment environments

## What features do orchestration tools offers?
- High Availability (the application has no downtime so it's always accessible by the users)
- Scalability (it loads fast and the users have a very high response rates from the application)
- Disaster recovery (backup and restore)
  
## Kubernetes Components

1. Node 
 - a simple server, a physical or virtual machine
 - run the containers of your application

2. Pod 
-  the basic component 
- the smallest unit of kubernetes
- abstraction of container
- it creates this running environment or a layer on top of the container
- usually 1 application per Pod
- each Pod gets its own IP adress (NOT THE CONTAINER)

1. Service 
- pods communicate with each other using a service 
- permanent IP adress
- lifecycle of Pod and Service are not connected 
1. Ingress   
- Instead of service, the reqest goes first to ingress and it does the forwarding then to the service
1. ConfigMap
- external configuration of your application 
1. Secret 
- used to store secret data
- base64 encoded 
  
### Data Storage
**Volumes**
- component of kubernetes 
- attaches a physical storage on a hard drive to your pod 
- storage on local machine or remote, outside of the K8s clust
  
A storage is an external hard drive plugged in into the kubernetes cluster  
K8s doesn't manage data persistance  

**Deployment**
- blueprint for my-app pods 
- you would not be creating pods, you create deployments
- you can specify how many replicas do you want
- abstraction of pods
- **DB can't be replicated via Deployment**
  
**StatefulSet**
- specifically for applications like databases
- would take care of replicating the pods but making sure the database reads and writes are synchronized
  
## Kubernetes Architecture

Types of Nodes:  
1. Master
2. Slave
   
- each Node has multiple Pods on it 
- 3 processes must be installed on every Node
- Worker Nodes do the actual work 
  
1. The first process that needs to run on every node is the **container runtime**(because application pods have containers running inside a container runtime needs to be installed on every node)  
2. The process that actually schedules those pods and the containers in underneath is **Kubelet**(a process of kubernetes that has interface with both-the container and the node)  
   - must be installed on every node
   - Kubelet starts the pod with a container inside 
3. The third process that is responsible for forwarding requests from services to pods is **Kube Proxy**
   - also must be installed on every node
   - makes sure that the communication also works 
  
These managing processes are done by Master Nodes
## Master processes
1. API Server 
   - when you as a user want to deploy a new application in a kubernetes cluster you interact with the API Server
   - cluster gateway 
   - acts as a gatekeeper for authentification
   - only 1 entrypoint to the cluster
2. Scheduler 
   - has this intelligent way of deciding on which specific worker node the next pod will be scheduled
   - it's going to go through the worker nodes and see the available resources on each one of them(it chooses the least busy node)
   - **Scheduler just decides on which Nod new Pod should be scheduled**
3. Controller manager 
   - detect cluster state changes(crashing of pods) 
   - it makes a request to the Scheduler to reschedule those dead pods
4. etcd 
   - a key Value Store of a cluster State 
   - the cluster brain 
   - Cluster changes get sored in the key value store
   - **Application data is not stored in etcd**

## MiniKube
- is basically one node cluster where the master processes and the work processes both run on one node
- this node will have a Docker container runtime pre-installed
- creates a virtual box on your laptop
- for testing purposes 
  
## Kubectl 
- a command line tool for K8s cluster
- the way to talk to the API server is through different clients(UI, API, Kubectl)
- used to interact with Minikube cluster, Cloud cluster
  
## Commands 
`minikube get notes` -> get status of nodes  
`minikube status`  
`kubectl get pod`-> to check the pods  
`kubectl get services` -> to check the services  
`kubectl create deployment [name] --image-img` -> to create a deployment  
You can't create a pod  
**Replicaset** is managing the replicas of a pod  

`kubectl edit deployment [name]` -> we get an auto-generated configuration file with default values  

`kubectl logs [pod name]`  
`kubectl describe pod [pod name]`  
`kubectl exec` ->for debbuging or test something  
`kubectl delete deployment [name]` ->to delete a deployment  

CRUD Operations happens on the deployment level and everything underneath just follows automatically  

`kubectl apply -f [file name]` (config-file.yaml) -> to apply a configuration file  
K8s knows when to create or pdate depoyment  

## YAML Configuration file
Each configuration file has 3 parts 
1. Metadata
2. Specification
3. status -> Automatically generated and added by kubernetes
   
Pod should have its own configuration inside of deployments configuration file  
Metadata part contains the labels and the specification part contains the selectors  

`kubernetes get all` ->shows all the components that are inside the cluster

### How to make it an External Service?
- type: LoadBalancer -> assigns service an external IP adress and so accepts external requests  
- nodePort -> the port where this external IP adress will be open(the port i have to put in the browser to access this service)
    - must be between 30000-32767

## Namespace
 - organise resources in namespaces 
 - virtual cluster inside a kubernetes clusters
 - when you create a cluster by default kubernetes gives you namespaces
  
1. kubernetes-dashboard
   - only with minikube
2. kube-system
   - **do not create or modify in kube-system**
   - system processes
   - Master & Kubectl processes
3. kube-public 
   - publicely accessible data
   - a configmap, which contains cluster informations
4. kube-node-lease
   - heartbeats of nodes
   - each node has associated lease object in namespace
   - determines the availability of a node
5. default 
   - resources you create are located here

`kubectl create namespace [name]` -> to create a namespace  
You can create namespaces with a configuration file  

Should not use for smaller projects  
### Use Cases when to use Namespaces
- Structure your components 
- Avoid conflicts between teams
- Share services between different environments
- Share services between different environments
- Access and Resource Limits on Namespaces Level 

Each Namespace must define own ConfigMap, Secret
Volume and Node can't be created within a Namespace  

## Ingress 
 
You need an implementation -> Ingress Controller  
Ingress Controller:
- evaluates all the rules
- manages redirections
- entrypoint to cluster
- many third-party implementations

No server in K8s cluster is accessible from outside!  

`minikube addons enable ingress` -> automatically starts the K8s Nginx implementation of Ingress Controller  
