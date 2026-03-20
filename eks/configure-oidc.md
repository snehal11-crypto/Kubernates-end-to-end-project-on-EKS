# 🔗 How to setup ALB add-on

---

## 📥 Download IAM policy

```bash id="p1a9xz"
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

---

## 🔐 Create IAM Policy

```bash id="kq2lmn"
aws iam create-policy \
--policy-name AWSLoadBalancerControllerIAMPolicy \
--policy-document file://iam_policy.json
```

---

## 👤 Create IAM Role

```bash id="z8xv3c"
eksctl create iamserviceaccount \
--cluster=<your-cluster-name> \
--
```
