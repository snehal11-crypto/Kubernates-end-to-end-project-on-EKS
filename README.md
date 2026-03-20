# 🚀 End-to-End Project: AWS EKS + App Deployment + ALB Ingress

## 📌 Overview

This project demonstrates how to deploy a containerized application on **AWS EKS (Elastic Kubernetes Service)** and expose it using **AWS Application Load Balancer (ALB) Ingress**.

---

## 🧰 Tech Stack

* AWS EKS
* Kubernetes
* AWS Load Balancer Controller
* Helm
* kubectl

---

## 🏗️ Architecture

User → ALB (Ingress) → Service → Pods → EKS Cluster

---

## 🪜 STEP 1: Prerequisites

Install the following tools:

```bash
aws --version
kubectl version --client
eksctl version
helm version
```

---

## 🔑 STEP 2: Configure AWS

```bash
aws configure
```

Enter:

* Access Key
* Secret Key
* Region: ap-south-1

---

## ☸️ STEP 3: Create EKS Cluster

```bash
eksctl create cluster \
--name demo-cluster \
--region ap-south-1 \
--node-type t2.micro \
--nodes 2
```

⏳ Wait 10–15 minutes for cluster creation.

---

## 🔗 STEP 4: Connect kubectl

```bash
aws eks --region ap-south-1 update-kubeconfig --name demo-cluster
kubectl get nodes
```

✅ Nodes should be in **Ready** state.

---

## 🔐 STEP 5: Configure IAM OIDC Provider

```bash
eksctl utils associate-iam-oidc-provider \
--region ap-south-1 \
--cluster demo-cluster \
--approve
```

---

## 📜 STEP 6: Create IAM Policy (ALB Controller)

```bash
curl -o iam_policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/install/iam_policy.json

aws iam create-policy \
--policy-name AWSLoadBalancerControllerIAMPolicy \
--policy-document file://iam_policy.json
```

---

## 👤 STEP 7: Create IAM Service Account

```bash
eksctl create iamserviceaccount \
--cluster demo-cluster \
--namespace kube-system \
--name aws-load-balancer-controller \
--attach-policy-arn arn:aws:iam::<YOUR-ACCOUNT-ID>:policy/AWSLoadBalancerControllerIAMPolicy \
--approve
```

---

## 📦 STEP 8: Install AWS Load Balancer Controller

```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
-n kube-system \
--set clusterName=demo-cluster \
--set serviceAccount.create=false \
--set serviceAccount.name=aws-load-balancer-controller \
--set region=ap-south-1 \
--set vpcId=<YOUR-VPC-ID>
```

---

## ✅ Verify Controller

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

---

## 🎮 STEP 9: Deploy 2048 Game App

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/main/docs/examples/2048/2048_full.yaml
```

---

## 📊 Check Resources

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
```

---

## 🌍 STEP 10: Access Application

```bash
kubectl get ingress
```

👉 Example Output:

```
k8s-xxxx.ap-south-1.elb.amazonaws.com
```

👉 Open in browser → 🎉 **2048 Game Running**

