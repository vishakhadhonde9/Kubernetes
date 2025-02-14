# Deployment -
- Deployment in Kubernetes is a way to manage and maintain a set of identical pods that ensure your application runs smoothly, reliably, and with the desired number of instances.
- It automatically ensures that the correct number of pod replicas are running.
- If a pod crashes or is deleted, the deployment will automatically create new pods to replace them.
- Kubernetes allows you to **update** your application without downtime by updating one pod at a time.
- If something goes wrong after an update then you can easily **roll-back** to the previous version.


        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: nginx-deployment  
        spec:
          replicas: 3  
          selector:
            matchLabels:
              app: nginx  
          template:
            metadata:
              labels:
                app: nginx  
            spec:
              containers:
              - name: nginx  
                image: nginx:latest 
                ports:
                - containerPort: 80  


## Check the Deployment: 

     kubectl get deployments

## Check Pods: 

     kubectl get pods

## Scaling: 

     kubectl scale deployment nginx-deployment --replicas=5


# Example of Rolling Update:

      spec:
        containers:
        - name: nginx
          image: nginx:1.19  # Update the image


## Check the current rollout status:

        kubectl rollout status deployment/<deployment-name>


## Rollback a deployment to a previous revision:

        kubectl rollout undo deployment/<deployment-name>


## View the history of a rollout:

        kubectl rollout history deployment/<deployment-name>


