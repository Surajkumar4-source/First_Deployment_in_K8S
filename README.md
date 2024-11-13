# Deploying and Exposing an HTTPD Server Using 'kubectl'

### This guide walks through creating a deployment for an HTTPD server (`suraj-app`) and exposing it using a NodePort service to enable external access.
### The steps also include scaling the deployment up and down to handle varying loads.


<br>

## Follow the Steps to Implement

### 1. Create the Deployment
- To start, create a deployment named `suraj-app` using the official `httpd` image.

```yml
kubectl create deployment suraj-app --image=httpd
```

*This command initializes a deployment named suraj-app using the HTTPD image, which serves as our web server.*

### 2. Expose the Deployment via NodePort Service.

- Expose the deployment to allow external access through a NodePort service.

 ```yml

  kubectl expose deployment suraj-app --type=NodePort --port=80 --name=suraj-service

```

*This command creates a service named suraj-service that allows external traffic on port 80 by assigning a random NodePort.*

### 3. Verify the NodePort
- To confirm the assigned NodePort and external accessibility details, use:

 ```yml
  
  kubectl get svc suraj-service

```
*Look under the PORT(S) column to see the NodePort assigned, which will look like 80:<NodePort>/TCP.*

### 4. Access the Service Externally
- Access the service from outside the cluster using the cluster node's IP and the assigned NodePort. For example:

```yml
http://<NODE_IP>:<NodePort>
```

*Replace <NODE_IP> with your cluster's external IP and <NodePort> with the actual port obtained from the previous step.*



### 5. Scale the Deployment
- Adjust the deployment’s number of replicas as needed to manage workload:

#### a. Scale Up
- To increase the number of replicas to 10:

```yml

kubectl scale deployment suraj-app --replicas=10

```
*This command scales up the deployment, creating additional pods for suraj-app.*

- Verify the scaling:
```yml

kubectl get deployment suraj-app
kubectl get pods -o wide

```

#### b. Scale Down
- To decrease the replicas to 5:

```yml
kubectl scale deployment suraj-app --replicas=5

```
- Verify the new count:
  
```yml

kubectl get deployment suraj-app
kubectl get pods -o wide

```

## Explanation of Scaling
- Scaling Up: Creates more pods to distribute the load and ensure high availability.
- Scaling Down: Reduces the number of running pods, saving resources when demand is lower.


*This setup deploys a scalable HTTPD server accessible from outside the cluster, allowing you to manage the web server load efficiently.*
