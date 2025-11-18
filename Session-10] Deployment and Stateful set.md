# There are two main ways to deploy applications in Kubernetes:
Deployment – for stateless applications
StatefulSet – for stateful applications

## Stateless applications -
- Does not store any data or session information about previous interactions.
- Each request is treated independently, and once a request is completed, the application does not retain any information related to it.
## Stateful applications-
- A stateful application is an application that maintains data or state across sessions and requests.
- Unlike stateless applications, a stateful application cannot treat every request as independent, because it relies on information that was created or stored during earlier interactions.

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

# Stateful Set -
- A StatefulSet is a Kubernetes object used to manage stateful applications—applications that need:
       - Stable (fixed) pod names
       - Stable network identity
       - Persistent storage for each pod
       - Ordered deployment and deletion



                        apiVersion: apps/v1
                        kind: StatefulSet
                        metadata:
                          name: web
                        spec:
                          serviceName: "web"
                          replicas: 3
                          selector:
                            matchLabels:
                              app: web
                          template:
                            metadata:
                              labels:
                                app: web
                            spec:
                              containers:
                              - name: web
                                image: nginx
                                volumeMounts:
                                - name: data
                                  mountPath: /usr/share/nginx/html
                          volumeClaimTemplates:
                          - metadata:
                              name: data
                            spec:
                              accessModes: ["ReadWriteOnce"]
                              resources:
                                requests:
                                  storage: 1Gi



- First you need to create headless service.


apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  clusterIP: None
  selector:
    app: mysql
  ports:
    - port: 3306
      
