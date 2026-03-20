# 🎮 2048 App

---

## 🔗 Create Fargate profile

```bash id="farg8p"
eksctl create fargateprofile \
--cluster demo-cluster \
--region us-east-1 \
--name alb-sample-app \
--namespace game-2048
```

---

## 🚀 Deploy the deployment, service and Ingress

```bash id="dep2048x"
kubectl apply -f https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.5.4/docs/examples/2048/2048_full.yaml
```

---

## 🌐 Access the application

```bash id="getalb1"
kubectl get ingress -n game-2048
```

👉 Copy the **ADDRESS** (ALB DNS) and open it in browser

---

## 🎯 Output

You should see the **2048 Game UI** running on browser 🎮
