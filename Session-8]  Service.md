# Service -
- Service is an abstraction that defines a logical set of Pods and a policy to access them.
- Services enable communication between different Pods and other resources, as they provide stable network identities for Pods.
- Service port: 30000-32767

#### List Services-

         kubectl get services

####  Get Details of a Specific Service

        kubectl describe service <service-name>

#### Deleting a Service-

        kubectl delete service <service-name>




## Types of services -

### 1] ClusterIP Service -
- ClusterIP service is the default and most common type of service in Kubernetes.
- It provides a stable IP address that is only accessible from other Pods within the same Kubernetes cluster.
- The ClusterIP service is ideal for internal communication within the cluster.

         
                 apiVersion: v1
                 kind: Service
                 metadata:
                   name: service1
                 spec:
                   type: ClusterIP  
                   selector:
                     app: my-app  
                   ports:
                     - port: 80  # The port exposed by the service
                       targetPort: 80  # The port on the Pods that the service forwards traffic to
                     




#### Accessing the ClusterIP Service from Pods-

curl http://my-clusterip-service.default.svc.cluster.local:80



## 2]  NodePort -
- NodePort service is a type of Kubernetes service that exposes your application on a static port on every node in your cluster.
- This allows external clients to access your service by sending requests to <NodeIP>:<NodePort>.
- Essentially Kubernetes maps a port on each node to a port inside your container and making it accessible outside the Kubernetes cluster.

         apiVersion: v1
         kind: Service
         metadata:
           name: nginx-service
         spec:
           type: NodePort
           selector:
             app: nginx
           ports:
             - protocol: TCP    # Optional
               port: 80         # Port on which the service will be available internally
               targetPort: 80   # Port on the pod to which traffic will be forwarded
               nodePort: 30001  # External port to be used on each node
           


#### Accessing the Service from Outside the Cluster-

        http://node's IP:30001


## 3] Load Balancer-
- LoadBalancer service is a type of Kubernetes service that automatically provisions an external load balance to distribute traffic to the service's Pods.
- This is especially useful when you want to expose your application to the internet and rely on a cloud provider's infrastructure to manage external traffic.


        apiVersion: v1
        kind: Service
        metadata:
          name: my-loadbalancer-service
        spec:
          selector:
            app: my-app
          ports:
              port: 80         # The port the service will be available on inside the cluster.
              targetPort: 8080  # The port inside the container that the service will forward to.
          type: LoadBalancer   


### Accessing the LoadBalancer Service
        http://external-IP:80




# Ingress -
- Ingress in Kubernetes is a resource that manages external access to services within a Kubernetes cluster for usually HTTP or HTTPS traffic.
- It acts as an entry point for external users or systems to reach your services running inside Kubernetes.
- If you have two services:
       -  Service A: Handles the homepage (example.com).
       -  Service B: Handles the API (api.example.com).
- You can create an Ingress to route traffic to each service based on the URL or hostname:
       -  Requests to example.com go to Service A.
       -  Requests to api.example.com go to Service B.



                  apiVersion: networking.k8s.io/v1
                  kind: Ingress
                  metadata:
                    name: my-ingress
                  spec:
                    rules:
                    - host: example.com
                      http:
                        paths:
                        - path: /
                          pathType: Prefix
                          backend:
                            service:
                              name: service-a
                              port:
                                number: 80
                    - host: api.example.com
                      http:
                        paths:
                        - path: /
                          pathType: Prefix
                          backend:
                            service:
                              name: service-b
                              port:
                                number: 80
                    






  
