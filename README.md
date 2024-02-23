# EKS - Horizontal Pod Autoscaling (HPA)

## Step-01: Introduction
- What is Horizontal Pod Autoscaling?
- How HPA Works?
- How HPA configured?

## Step-02: Install Metrics Server
```
# Verify if Metrics Server already Installed
kubectl -n kube-system get deployment/metrics-server

# Install Metrics Server
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.7.0/components.yaml

# Verify
kubectl get deployment metrics-server -n kube-system
```

## Step-03: Review Deploy our Application
```
# Deploy
kubectl apply -f deploy.yaml
kubectl apply -f svc.yaml
kubectl apply -f hpa.yaml

# List Pods, Deploy & Service
kubectl get pod,svc,deploy

# Access Application using loadbalancer dns name
```

## Step-04: Create a Horizontal Pod Autoscaler resource for the "hpa-demo-deployment" 
- This hpa creates an autoscaler that targets 50 percent CPU utilization for the deployment, with a minimum of one pod and a maximum of ten pods. 
- When the average CPU load is below 50 percent, the autoscaler tries to reduce the number of pods in the deployment, to a minimum of one. 
- When the load is greater than 50 percent, the autoscaler tries to increase the number of pods in the deployment, up to a maximum of ten
```

# creating hpa
kubectl apply -f hpa.yaml

# Describe HPA
kubectl describe hpa hpa-demo-deployment 

# List HPA
kubectl get hpa
```

## Step-05: Create the load & Verify how HPA is working
```
# Generate Load
kubectl run apache-bench -i --tty --rm --image=httpd -- ab -n 500000 -c 1000 http://hpa-demo-service-nginx.default.svc.cluster.local/


# List all HPA
kubectl get hpa

# List specific HPA
kubectl get hpa hpa-demo-deployment 

# Describe HPA
kubectl describe hpa/hpa-demo-deployment 

# List Pods
kubectl get pods
```

## Step-06: Cooldown / Scaledown
- Default cooldown period is 5 minutes. 
- Once CPU utilization of pods is less than 50%, it will starting terminating pods and will reach to minimum 1 pod as configured.



## Referencess
### Metrics Server Releases
- https://github.com/kubernetes-sigs/metrics-server/releases

### Horizontal Pod Autoscaling - Scale based on many type of metrics
- https://v1-16.docs.kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
