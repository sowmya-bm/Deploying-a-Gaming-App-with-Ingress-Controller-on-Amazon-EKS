
"Deploying a Gaming App with Ingress Controller on Amazon EKS"

1. **Content:**
   - Implemented DevOps practices to deploy a scalable gaming app on Amazon EKS.
   - Utilized Fargate, Kubernetes resources, Ingress, and ALB for external access.
   - Configured Ingress Controller, reducing load balancer costs.
   - Achieved scalability, availability, and security while optimizing performance and cost-efficiency.

2. Pre-requisites:
   - "kubectl: [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/)"
   - "eksctl: [AWS EMR on EKS Development Guide](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html)"
   - "awscli: [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)"
   - "helm: [Helm documentation](https://helm.sh/docs/intro/install/)"

4. **Commands Used:**
   - "To create the cluster: `eksctl create cluster --name gaming2048 --region us-east-1 --fargate`"
   - "To connect to the cluster: `aws eks update-kubeconfig --name gaming2048`"
   - "To create a fargate profile: `eksctl create fargateprofile --cluster gaming2048 --region us-east-1 --name alb-gaming-app --namespace game-2048`"
   - "To create a new namespace resource: `kubectl apply -f namespace.yaml`"
   - "To create a deployment resource: `kubectl apply -f deployment.yaml`"
   - "To create a service resource: `kubectl apply -f service.yaml`" 
   - "To create an ingress resource: `kubectl apply -f ingress.yaml`"
   - "To connect to the OIDC provider: `eksctl utils associate-iam-oidc-provider --cluster gaming2048 --approve`"
   - "To download IAM permissions required for the role: `curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/install/iam_policy.json`"
   - "To create the role: `aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json`"
   - "To create a service account and attach it to the role: `eksctl create iamserviceaccount --cluster=gaming2048 --namespace=kube-system --name=aws-load-balancer-controller --role-name AmazonEKSLoadBalancerControllerRole --attach-policy-arn=arn:aws:iam::211125556539:policy/AWSLoadBalancerControllerIAMPolicy --approve`"
   - "To download and update the helm for load balancer: `helm repo add eks https://aws.github.io/eks-charts` `helm repo update eks`"
   - "To install the load balancer: `helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=gaming2048 --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller --set region=us-east-1 --set vpcId=vpc-050b1e93673dbfcf4`"

_**Visit my blog https://sowmya-mb-techblogs.hashnode.dev for more detailed explaination and POCs**_
