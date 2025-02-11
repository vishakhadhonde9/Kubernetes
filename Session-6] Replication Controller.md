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


# Probes -
- Probes are mechanisms that Kubernetes uses to determine whether containers in a pod are running properly.

## Types of probes:

### 1] Liveness Probe -
- Liveness Probe in Kubernetes is used to check whether a container in a pod is still running and healthy.
- If the liveness probe fails, Kubernetes will restart the container to try and recover from an unhealthy state.


#### Types of Liveness Probes -
**a] HTTP Request Probe-**

            livenessProbe:
              httpGet:
                path: /healthz
                port: 8080
              initialDelaySeconds: 3
              periodSeconds: 5

**b] TCP Socket Probe-**

            livenessProbe:
              tcpSocket:
                port: 8080
              initialDelaySeconds: 3
              periodSeconds: 5

**c] Exec Probe-**
- Kubernetes runs a command inside the container and checks the exit code.

            
            livenessProbe:
              exec:
                command:
                  - "ls /usr/share/nginx/html"
              initialDelaySeconds: 3
              periodSeconds: 5



### 2] Readiness Probe -
- The Readiness Probe in Kubernetes is used to determine if a container is ready to accept traffic.
- It helps ensure that a container is fully initialized and can handle requests before Kubernetes routes any traffic to it. - If the readiness probe fails, Kubernetes will stop sending traffic to that container but will not restart it.

            apiVersion: v1
            kind: Pod
            metadata:
              name: myapp
            spec:
              containers:
                - name: myapp-container
                  image: myapp-image
                  readinessProbe:
                    httpGet:
                      path: /readiness
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 10
                    timeoutSeconds: 2
                    failureThreshold: 3
                    successThreshold: 1


### 3] Startup Probe -
- Startup Probe is a special type of probe in Kubernetes designed to detect if an application within a container has successfully started




























            apiVersion: v1
            kind: Pod
            metadata:
              name: myapp
            spec:
              containers:
                - name: myapp-container
                  image: myapp-image
                  livenessProbe:
                    httpGet:
                      path: /healthz
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 10
                    timeoutSeconds: 2
                    failureThreshold: 3
                    successThreshold: 1




