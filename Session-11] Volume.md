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
                    - mountPath: /usr/share/nginx/html/
                      name: v1
                  volumes:
                  - name: v1
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

  
## EFS Volume -

### Steps-
- Create efs.
- Add sg of worker node into sg of efs for nfs.
- create dir on worker node.
- and install nfs-common on WN.

        sudo apt update
        sudo apt install -y nfs-common

 
- copy mount command from nfs--> attach.
- replce efs from command by ~/dir-name.
  

      apiVersion: v1
      kind: Pod
      metadata:
        name: nginx-pod
      spec:
        containers:
          - name: nginx
            image: nginx:latest  # Nginx image from Docker Hub
            ports:
              - containerPort: 80  # Expose port 80 (HTTP) inside the container
            volumeMounts:
              - name: nfs-storage
                mountPath: /usr/share/nginx/html  # The directory inside the container where the NFS volume will be mounted
        volumes:
          - name: nfs-storage
            nfs:
              path: /  # The NFS export path on the NFS server
              server: fs-00bd1a710cc465364.efs.us-east-1.amazonaws.com  

## 3] PersistentVolume (PV) 
- PersistentVolume (PV) in Kubernetes is a piece of storage in your cluster that is independent of Pods.
- It’s like hard drive or storage device that exists at the cluster level and can be used by applications running in Pods.
- PV is used when you need storage that survives Pod restarts or deletions.
- PVs are cluster-wide resources, meaning they can be shared across multiple Pods in the cluster if needed.

#### Access Modes:
- ReadWriteOnce (RWO): Mounted as read-write by a single Pod.
- ReadOnlyMany (ROX): Mounted as read-only by many Pods.
- ReadWriteMany (RWX): Mounted as read-write by many Pods.

        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: my-pv
        spec:
          capacity:
            storage: 5Gi # Defines the storage capacity of the volume
          accessModes:
            - ReadWriteOnce # Specifies how the volume can be mounted (e.g., ReadWriteOnce, ReadOnlyMany, ReadWriteMany)
          persistentVolumeReclaimPolicy: Retain # Defines what happens to the volume when the PVC is deleted (e.g., Retain, Recycle, Delete)
          hostPath: # Example for a hostPath volume (for development/testing, not recommended for production)
            path: "/mnt/data" # The path on the host node where the data is stored
        


#### PV using EFS-

        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: nfs-pv
        spec:
          capacity:
            storage: 5Gi  # Specify the size of the volume
          accessModes:
            - ReadWriteMany  # NFS supports ReadWriteMany access mode (multiple pods can access at once)
          persistentVolumeReclaimPolicy: Retain  # Retain means the PV won't be deleted when PVC is removed
          storageClassName: nfs-sc  # Storage class name (optional)
          nfs:
            path: /  # The NFS export path on the NFS server
            server: fs-00bd1a710cc465364.efs.us-east-1.amazonaws.com  # DNS name of the EFS server

        
        

 ## 4] PersistentVolumeClaim (PVC) -
 -  PersistentVolumeClaim (PVC) is like a request for storage by a Pod in Kubernetes.
 -  When a Pod needs to store data that should persist even if the Pod is deleted or recreated, it creates a PVC.
 -  Once a PVC is created, Kubernetes will automatically bind the PVC to a PersistentVolume (PV) that matches the requested size and access mode.


          apiVersion: v1
          kind: PersistentVolumeClaim
          metadata:
            name: nfs-pvc
          spec:
            accessModes:
              - ReadWriteMany  # This must match the PV's access mode
            resources:
              requests:
                storage: 5Gi  # This must match the PV's capacity
            storageClassName: nfs-sc  # This should match the PV's storageClassName



#### Pod using PVC -

                apiVersion: v1
                kind: Pod
                metadata:
                  name: pod-with-pvc
                spec:
                  containers:
                  - name: container1
                    image: nginx
                    volumeMounts:
                    - mountPath: /data  # Path inside the container where the volume will be mounted
                      name: data-storage
                  volumes:
                  - name: data-storage
                    persistentVolumeClaim:
                      claimName: nfs-pvc # Referencing the PVC created earlier
                

## GitRepo Volume -
- GitRepo volume allows you to mount a Git repository as a volume, meaning you can pull code or files from a Git repository directly into a Kubernetes Pod.


                apiVersion: v1
                kind: Pod
                metadata:
                  name: gitrepo-example
                spec:
                  containers:
                    - name: my-container
                      image: nginx
                      volumeMounts:
                        - name: git-repo
                          mountPath: /usr/share/nginx/html  # Path where the git repo content will be mounted
                  volumes:
                    - name: git-repo
                      gitRepo:
                        repository: "https://github.com/username/repository-name.git"
                        revision: "main"  # The branch or commit ID to pull (e.g., "main" or "v1.0")
                        directory: ""  # The directory to mount inside the pod (leave empty to mount root of the repo)
                


## ConfigMap -
- ConfigMap is a way to store configuration data that can be used by Pods.
- You can mount a ConfigMap as a volume to provide the configuration data to your applications in a way that's easy to update.
- It store configuration settings (key-value pairs) that your applications need to run.
- Instead of putting these settings directly inside your app’s code, you can keep them in the ConfigMap, so you can easily change the settings without modifying the app itself.

## Creating a ConfigMap from Literal values: 

     kubectl create configmap my-configmap --from-literal key1=value1 --from-literal key2=value2

     kubectl create configmap mysql-pass --from-literal MYSQL_ROOT_PASSWORD=Pass@123

## Creating a ConfigMap Using manifest file:


      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: mysql-pass
      data:
        MYSQL_ROOT_PASSWORD: Pass@123



## Create pod referencing configMap: 

      apiVersion: v1
      kind: Pod
      metadata:
        name: mysql-pod
      spec:
        containers:
        - name: mysql
          image: mysql
          env:
           - name: MYSQL_ROOT_PASSWORD
             valueFrom:
              configMapKeyRef:
                name: mysql-pass  # Use the correct ConfigMap name
                key: MYSQL_ROOT_PASSWORD  # The key in the ConfigMap

     



## Secret -
- Secret in Kubernetes is a way to store sensitive information, such as passwords, API keys, or certificates, and make them available to your Pods (applications) securely.
- You don't want to store sensitive data directly in your code or in plain text in Kubernetes configurations.
- Secrets let you store this data in an encrypted format and use it securely within your applications.

#### Create a Secret:

                kubectl create secret generic my-secret --from-literal=password=mysecretpassword

#### Using YAML file:


        apiVersion: v1
        kind: Secret
        metadata:
          name: mysql-secret
        type: Opaque
        data:
          MYSQL_ROOT_PASSWORD: bXl1c2VyYXBhc3N3b3Jk  # Base64 encoded value of 'myuserpassword'
          MYSQL_PASSWORD: bXl1c2VyYXBhc3N3b3Jk  # Base64 encoded value of 'myuserpassword'



## Create pod by referencing secret -
                
      apiVersion: apps/v1
      kind: Deployment
      metadata:
        name: mysql-deployment
      spec:
        replicas: 1
        selector:
          matchLabels:
            app: mysql
        template:
          metadata:
            labels:
              app: mysql
          spec:
            containers:
            - name: mysql
              image: mysql:8
              env:
              - name: MYSQL_ROOT_PASSWORD
                valueFrom:
                  secretKeyRef:
                    name: mysql-secret
                    key: MYSQL_ROOT_PASSWORD
              ports:
              - containerPort: 3306
      














