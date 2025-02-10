#  Replication Controller-
- ReplicationController (RC) is a Kubernetes resource that ensures the desired number of identical Pods are running at any given time.
- If a Pod crashes the ReplicationController will automatically create a new Pod to replace it, ensuring that your application is always running with the desired number of Pods.
- If a Pod is deleted or crashes, the ReplicationController will recreate it.
- Scaling: You can scale your application by changing the number of replicas (Pods).
- Load Balancing: When combined with services, ReplicationControllers help distribute traffic among Pods.

            apiVersion: v1
            kind: ReplicationController
            metadata:
              name: nginxpod
            spec:
              replicas: 3  
              template:
                metadata:
                  labels:
                    app: nginx 
                spec:
                  containers:
                  - name: mycont1
                    image: nginx  
                    ports:
                    - containerPort: 80  





## List ReplicationControllers:

       kubectl get replicationcontrollers

## View Details of a ReplicationController:

       kubectl describe replicationcontroller / rc podname

## Scale the ReplicationController:
- To change the number of replicas (e.g., scale to 5 Pods):

       kubectl scale rc podname --replicas=5

## Delete a ReplicationController:

       kubectl delete replicationcontroller podname










