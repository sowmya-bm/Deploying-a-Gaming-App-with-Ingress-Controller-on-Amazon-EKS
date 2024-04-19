# Deploying-a-Gaming-App-with-Ingress-Controller-on-Amazon-EKS
Implemented DevOps practices to deploy a scalable gaming app on Amazon EKS. Utilized Fargate, Kubernetes resources, Ingress, and ALB for external access. Configured Ingress Controller, reducing load balancer costs. Achieved scalability, availability, and security while optimizing performance and cost-efficiency.

Pre-requisites:

1. kubectl: https://kubernetes.io/docs/tasks/tools/

2. eksctl: https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html

3. awscli: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

4. helm: https://helm.sh/docs/intro/install/

5. Create and configure an IAM user who can access the EKS cluster

COMMANDS USED:
To create the cluster: `eksctl create cluster --name gaming2048 --region us-east-1 --fargate`

To connect to the cluster: `aws eks update-kubeconfig --name gaming2048`

To create a fargate profile: `eksctl create fargateprofile \
    --cluster gaming2048 \
    --region us-east-1 \
    --name alb-gaming-app \
    --namespace game-2048`
    
To create a new namespace resource: `kubectl apply -f namespace.yaml`

To create a deployment resource: `kubectl apply -f deployment.yaml`

To create a service resource: `kubectl apply -f servcie.yaml`

To create an ingress resource: `kubectl apply -f ingress.yaml`

To connect to the OIDC provider: `eksctl utils associate-iam-oidc-provider --cluster gaming2048 --approve`

To download IAM permissions required for the role : `curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json`

To create the role:  `aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam_policy.json`

To reate a service account and attch it to the role: `eksctl create iamserviceaccount \
  --cluster=gaming2048 \         
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::211125556539:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve`

To download and update the helm for loadbalancer: `helm repo add eks https://aws.github.io/eks-charts`
`helm repo update eks`

To install the load balancer: `helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system \ 
  --set clusterName=gaming2048 \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=us-east-1 \
  --set vpcId=vpc-050b1e93673dbfcf4`


