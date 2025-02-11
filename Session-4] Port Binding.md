# Port Binding -
- Port binding refers to the process of mapping a port pod to a port on the host or external environment.
- This allows communication between the container/pod and external clients such as users or other services within the cluster.

## Forward Port -
- kubectl port-forward command is to allow you to access a specific port on a Kubernetes pod or service from your local machine.
- This is helpful when you want to interact with an application or service running inside a Kubernetes cluster

        kubectl port-forward pod-name <host-port>:<pod-port>


### Access Webpage:
    curl curl http://localhost:forwarded port
    curl http://127.0.0.1:forwarded port
    curl private_ip_of_pod
