# Volume -
- Volumes are a way to persist data that is used by containers within Pods.
- By default, when a container is deleted, any data stored inside it is also lost. But, if you use a volume, the data can persist beyond the container's life.
- If you have multiple containers in a Pod, they can share a volume, making it easier to pass data between them.


## Types of Volumes -

### 1] emptyDir -
- emptyDir is a temporary storage area inside a Kubernetes Pod.
- It is created when a Pod is scheduled (started) and deleted when the Pod is removed. The data inside it doesn’t persist after the Pod is gone.
- If a Pod has multiple containers, all those containers can share the same emptyDir volume. They can read and write to the same space.
- It's useful for storing temporary data that doesn't need to be kept after the Pod is deleted, like caching or temporary files that can be recreated.


        apiVersion: v1
        kind: Pod
        metadata:
          name: example-pod
        spec:
          containers:
          - name: container1
            image: nginx
            volumeMounts:
            - mountPath: /tmp
              name: temp-storage
          volumes:
          - name: temp-storage
            emptyDir: {}


### 2] hostPath -
- hostPath is a way to use storage from the host machine and make it available to a container inside a Pod.
- It allows a container to access a specific directory or file from the host machine's filesystem.
- This storage is shared between the host and the Pod, meaning the container inside the Pod can read from and write to files on the host.

      apiVersion: v1
      kind: Pod
      metadata:
        name: example-pod
      spec:
        containers:
        - name: container1
          image: nginx
          volumeMounts:
          - mountPath: /mnt/logs
            name: log-storage
        volumes:
        - name: log-storage
          hostPath:
            path: /var/log
            type: Directory


## 3] PersistentVolume (PV) 
- PersistentVolume (PV) in Kubernetes is a piece of storage in your cluster that is independent of Pods.
- It’s like hard drive or storage device that exists at the cluster level and can be used by applications running in Pods.
- 












