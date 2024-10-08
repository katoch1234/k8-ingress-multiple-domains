Kubernetes Ingress with AWS ALB Ingress Controller

Kubernetes Ingress is an API resource that allows you manage external or internal HTTP(S) access to Kubernetes services running in a cluster. Amazon Elastic Load Balancing Application Load Balancer (ALB) is a popular AWS service that load balances incoming traffic at the application layer (layer 7) across multiple targets, such as Amazon EC2 instances, in a region. ALB supports multiple features including host or path based routing, TLS (Transport Layer Security) termination, WebSockets, HTTP/2, AWS WAF (Web Application Firewall) integration, integrated access logs, and health checks.

The open source AWS ALB Ingress controller triggers the creation of an ALB and the necessary supporting AWS resources whenever a Kubernetes user declares an Ingress resource in the cluster. The Ingress resource uses the ALB to route HTTP(S) traffic to different endpoints within the cluster. The AWS ALB Ingress controller works on any Kubernetes cluster including Amazon Elastic Kubernetes Service (Amazon EKS).

***************************aws eks update-kubeconfig --region us-east-1 --name eks-ingress****************************************** Command to update kube-config


Step 1: eksctl create cluster -f eksctl-create-cluster.yaml



Step 2 : eksctl utils associate-iam-oidc-provider --cluster=k8-ingress --approve --region us-east-1

Step 3 : kubectl apply -f cluster-role.yaml

Step 4: kubectl apply -f cluster-role-binding.yaml

Step 5: kubectl apply -f service-account.yaml

Step 6 aws iam create-policy --policy-name ALBIngressControllerIAMPolicy --policy-document file://iam-policy.json

Next, create a Kubernetes service account and an IAM role (for the pod running the AWS ALB Ingress controller) by substituting $PolicyARN with the recorded value from the previous step:

Step 8: eksctl create iamserviceaccount --cluster=k8-ingress --namespace=kube-system --name=alb-ingress-controller --attach-policy-arn=arn:aws:iam::595496445232:policy/ALBIngressControllerIAMPolicy --override-existing-serviceaccounts --approve --region us-east-1

Step 9: helm repo add eks https://aws.github.io/eks-charts

Step 10: helm repo update eks

Step 11: helm install aws-load-balancer-controller eks/aws-load-balancer-controller   -n kube-system   --set clusterName=k8-ingress   --set serviceAccount.create=false   --set serviceAccount.name=alb-ingress-controller --set region=us-east-1 --set vpcID=vpc-09ae946dcd4438a21 --set ingressClass=alb

Verify the ingress class: kubectl get deployment aws-load-balancer-controller -n kube-system -o yaml

command to delete load-balancer-controller: helm delete aws-load-balancer-controller -n kube-system

Step 12: Check logs kubectl logs alb-ingress-controller-7559759587-jxnsp -n kube-system
Step 13: kubectl apply -f namespace.yaml 
Step 14: kubectl apply -f clusterip-wp-service.yaml, kubectl get all -n game

Step 15: kubectl apply -f service-apache.yaml
Step 16: kubectl apply -f deployment.yaml
         kubectl apply -f apache-deployment.yaml

Step 17: kubectl apply -f ingress-resource.yaml



Some useful commands to troubleshoot

kubectl get service node-port-service -n game

kubectl get endpoints node-port-service -n game
