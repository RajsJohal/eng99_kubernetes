# Kubernetes 

## What is it?
- Open source container orchestration platform, enables the operation of an elastic web server framework for cloud applications. 
- Can support data center outsourcing to publlic cloud service providers or can be used for web hosting at scale.
- In simple terms Kubernetes can manage a large amount of containers in orer to build an application. 

## SetUp 
- Navigate to docker and start kubernetes in the settings, this will start a download
- Once the download is completed, an indicator on the bottom of dockers UI will show a kubernetes symbol turn green to indicate it is running
- Open a terminl as admin and run the commands:
    - `kubectl version`
    - `kubectl get svc` - shows the cluster IP 

## Using Kubernetes 
- Create yml file 
- One to deploy and the other to declare the service
- Nginx-Deploy file:
```
#io.k8s.api.apps.v1.Deployment(v1@deployment.json)
# K8 works with API versions to declare resources
# We have to declare the apiVersion and the kind of service/component
# services: deployment, service, pods, replicasets, crobjob, autoscalinggroup, horizontal pod scaling group (HPA)


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment # naming the deployment



spec:
  selector:
    matchLabels:
      app: nginx # look for this label to match with k8 service


  replicas: 2

  # template to use its label for k8 service or any other 
  template: 
    metadata:
      labels:
        app: nginx # this label connects to the service or any other k8 components
    
    # defines the container spec
    spec:
      containers:
      - name: nginx
        image: rajsjohal/eng99_my_nginx:latest
        ports:
        - containerPort: 80

# Create a kubernetes nginx-service.yml
```
- This file creates 2 containers from the specified image name.

- Nginx-Service file:
```
---
# Select the type of API version and type of service/object
apiVersion: v1
kind: Service

metadata:
  name: nginx-svc
  namespace: default

# Specification to include ports Selector to connect to the deployment
spec: 
  ports:
  - nodePort: 30442 #range is 30000-32768
    port: 80
    protocol: TCP
    targetPort: 80

  selector:
    app: nginx # this label connects this to the nginx-deploy yml file, they are running within the same cluster

  type: LoadBalancer
```
- This file will expose the correct port in order to access the container from locahost 
- Nginx container can be seen on web browser from localhost after running the serive .yml file
- Deleting one container pod will just revive the pod due to kubernetes self healing option which will recover the pod. 

## Delete Kubernetes deployment
- `kubectl delete deploy <Deploy-Name>`
