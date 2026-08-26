# K8s Security -
- A container is a process running on a Linux system.
- That process has an identity and permissions.
- For example, on a Linux machine, we have permissions for users, Kubernetes Security Context allows us to control these things for containers.

          apiVersion: v1
          kind: Pod
          metadata:
            name: wepod
          spec:
            containers:
            - name: busybox
              image: busybox
              command: ["sleep", "3200"]
              securityContext:
                capabilities:
                  add: ["SYS_TIME"]



- Suppose we create:

      kubectl run mypod --image=busybox

- Kubernetes creates a Pod containing a BusyBox container. The process inside the container runs with whatever user/security settings are defined by the image and Kubernetes/runtime defaults. We can inspect the user:

      kubectl exec -it mypod -- id

- You might see something such as:

        uid=0(root) gid=0(root)

- uid=0 means the process is running as root.

        securityContext:
          runAsUser: 1000

- Now Kubernetes tries to run the container process as Linux user 1000.

# 1. runAsUser
- runAsUser specifies the Linux user ID (UID) under which the container's processes should run.
- By default, depending on the container image and configuration, a process may run as root. Using runAsUser allows us to explicitly specify a non-root user.
  
      securityContext:
        runAsUser: 1000

This tells Kubernetes that Run the container process as Linux user 1000.


      apiVersion: v1
      kind: Pod
      metadata:
        name: user-demo
      spec:
        securityContext:
          runAsUser: 1000
        containers:
          - name: app
            image: busybox
            command: ["sleep", "3600"]

- Here, the sleep process runs with UID 1000.



## Kubernetes Security Context

| Security Context | What it means | Simple way to remember |
|---|---|---|
| `runAsUser` | Specifies **which Linux user** runs the container process | **Who are you?** |
| `runAsGroup` | Specifies **which Linux group** the process belongs to | **Which group do you belong to?** |
| `runAsNonRoot` | Ensures the container does **not run as root** | **Don't be administrator** |
| `capabilities.add` | Adds a **specific Linux privilege** to the container | **Give me this special permission** |
| `capabilities.drop` | Removes a Linux privilege from the container | **Take away this permission** |
| `privileged` | Gives the container **very broad privileges** | **Give me administrator-level access** |
| `allowPrivilegeEscalation` | Controls whether a process can **gain more privileges** | **Can you increase your permissions?** |
| `readOnlyRootFilesystem` | Makes the container's root filesystem **read-only** | **You can read, but can't modify the main filesystem** |









