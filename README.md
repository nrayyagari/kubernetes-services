[![microservice1-build-push-master](https://github.com/nrayyagari/kubernetes-microservices/actions/workflows/microservice1-build-push.yaml/badge.svg?branch=main)](https://github.com/nrayyagari/kubernetes-microservices/actions/workflows/microservice1-build-push.yaml)
[![microservice2-build-push-master](https://github.com/nrayyagari/kubernetes-microservices/actions/workflows/microservice2-build-push.yaml/badge.svg?branch=main)](https://github.com/nrayyagari/kubernetes-microservices/actions/workflows/microservice2-build-push.yaml)


# kubernetes-services

eksctl create iamserviceaccount \
  --cluster=eks-cluster-1 \
  --namespace=kube-system \
  --name=aws-load-balancer-controller \
  --role-name AmazonEKSLoadBalancerControllerRole \
  --attach-policy-arn=arn:aws:iam::927862243533:policy/AWSLoadBalancerControllerIAMPolicy \
  --approve

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=eks-cluster-1 \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller 

  eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster eks-cluster-1 \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --role-only \
  --role-name AmazonEKS_EBS_CSI_DriverRole
