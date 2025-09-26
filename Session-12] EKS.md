# Elastic Kubernetes Cluster -
- Amazon EKS (Elastic Kubernetes Service) is a managed Kubernetes service provided by AWS.
- Normally, if you install Kubernetes yourself, you have to manage the control plane (API server, etcd, scheduler, controllers), upgrades, and security patches.

# How EKS Works-
- 1. You create an EKS cluster.
- 2. AWS provisions the Kubernetes control plane (you don’t manage it).
- 3. You add worker nodes (EC2 or Fargate) to the cluster.
- 4. You deploy your apps using kubectl or YAML manifests.
- 5. To access apps, you use Kubernetes Services.

# Implementation -
### 1] Create EKS Cluster -
- Go to EKS service → click Add cluster → Create.
    - Cluster name → my-eks-cluster
    - Kubernetes version → latest recommended (e.g., 1.29).
    - Cluster IAM Role:
        - Choose EKS as use case → attach AmazonEKSClusterPolicy.
    - Cluster Endpoint Access  → Public.
    - Create → then select it.
### 2] Configure EC2 instance as WN -
- Create EC2 instance and conncet via SSH.
- Install AWS cli, kubcectl and eksctl.

#### Kubectl Installation -

      # Download kubectl (Linux AMD64)
      curl -LO "https://dl.k8s.io/release/v1.30.0/bin/linux/amd64/kubectl"
      
      # Make it executable
      chmod +x ./kubectl
      
      # Move to PATH
      sudo mv ./kubectl /usr/local/bin/
      
      # Verify
      kubectl version --client


#### Eksctl installation -

      # Download latest eksctl
      curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz
      
      # Move to PATH
      sudo mv eksctl /usr/local/bin/
      
      # Verify
      eksctl version

- Check EKS Cluster status -

      aws eks describe-cluster --name my-eks-cluster --region us-east-1 --query "cluster.status"

      e.g.- aws eks describe-cluster --name eksc1 --region us-east-1 --query "cluster.status"

## 3] Verify Worker Nodes (kubectl) -
- Once the cluster is ACTIVE and you have updated kubeconfig:

      aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster

      e.g.- aws eks --region us-east-1 update-kubeconfig --name eksc1

      kubectl get svc

## 4] Create Worker Nodes (Node Group) -
- Once cluster is Active, Go to Compute → Add Node Group.
   - Node group name → linux-nodes.
   - Node IAM Role: Attach policies:

        AmazonEKSWorkerNodePolicy
        AmazonEKS_CNI_Policy
        AmazonEC2ContainerRegistryReadOnly

## 5] Create Resources -

            apiVersion: v1
            kind: Pod
            metadata:
              name: my-pod
            spec:
              containers:
              - name: my-container
                image: nginx


# AWS Fargate -
- AWS Fargate is a serverless compute engine for containers.
- It allows you to run Docker containers without managing EC2 instances.
- You don’t have to worry about:
    - Provisioning servers
    - Scaling nodes
    - Patching operating systems

- Works with:
    - Amazon ECS (Elastic Container Service)
    - Amazon EKS (Elastic Kubernetes Service)
- pay only for the container CPU & memory you use

## Implementation-

#### Create a Fargate Profile -
- Go to Cluster → Compute → Fargate Profiles → Add profile
    - Name: my-fargate-profile
    - Pod Execution Role:
            - policy AmazonEKSFargatePodExecutionRolePolicy
    - Select Subnets:
    - Choose 2 or more subnets in different AZs
    - Pod Selector:
    - Namespace: e.g., default (all pods in default namespace run on Fargate)
    - Labels: optional (for selective pods)







