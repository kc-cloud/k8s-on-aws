# Kubernetes on AWS
- Install kops
- Install kubectl
- Create a IAM user with the following policies:
```
AmazonEC2FullAccess
AmazonRoute53FullAccess
AmazonS3FullAccess
IAMFullAccess
AmazonVPCFullAccess
AmazonSQSFullAccess
AmazonEventBridgeFullAccess
```
- Configure the user locally
- Create s3 bucket for storing the kops state (For example, s3://<your-cluster-name>-<your-domain-name>-state-store)
- Setup the following environment variables:
```
export CLUSTER_NAME=<your-cluster-name>.<your-domain-name>.com (Make sure that you have a domain procured in AWS)
export KOPS_STATE_STORE=s3://<your-cluster-name>-<your-domain-name>-state-store
```

## Create Kubernetes on AWS

Use the following cops command to create the cluster:

```
kops create cluster --name=${CLUSTER_NAME} \
    --state ${KOPS_STATE_STORE} \
    --zones us-east-1a \
    --cloud aws \
    --network-cidr 10.0.0.0/16 \
    --master-size t2.large \
    --node-size t2.medium \
    --node-count 2 \
    --networking calico \
    --topology private
```

Above command creates a VPC (with private/public subnets, IGW, NGW, Route Tables) and a k8s cluster with 1 master node and 2 worker nodes. 