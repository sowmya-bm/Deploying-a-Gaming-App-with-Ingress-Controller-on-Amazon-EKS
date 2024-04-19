# Deploying-a-Gaming-App-with-Ingress-Controller-on-Amazon-EKS
Implemented DevOps practices to deploy a scalable gaming app on Amazon EKS. Utilized Fargate, Kubernetes resources, Ingress, and ALB for external access. Configured Ingress Controller, reducing load balancer costs. Achieved scalability, availability, and security while optimizing performance and cost-efficiency.

Pre-requisites:

1. kubectl: https://kubernetes.io/docs/tasks/tools/

2. eksctl: https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html

3. awscli: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

4. helm: https://helm.sh/docs/intro/install/

5. Create and configure an IAM user who can access the EKS cluster

COMMANDS USED:

' eksctl create cluster --name gaming2048 --region us-east-1 --fargate '
'aws eks update-kubeconfig --name gaming2048'


'eksctl create fargateprofile \
    --cluster gaming2048 \
    --region us-east-1 \
    --name alb-gaming-app \
    --namespace game-2048'



So the cluster is up and running and is ready for the application to be deployed. In the next blog we shall move onto deploying the resources in the EKS cluster
Once we have created the namespace, we shall now move on to creating the namespace in our EKS cluster through manifest.

apiVersion: v1
kind: Namespace
metadata:
  name: game-2048
