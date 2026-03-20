# 🚀 Sample App deployment

---

## 📄 Copy the deploy.yml to your local and save it with name deploy.yml

```yaml id="depfile1"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: eks-sample-linux-deployment
  labels:
    app: eks-sample-linux-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: eks-sample-linux-app
  template:
    metadata:
      labels:
        app: eks-sample-linux-app
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/arch
                operator: In
                values:
                - amd64
                - arm64
      containers:
      - name: nginx
        image: public.ecr.aws/nginx/nginx:1.23
        ports:
        - name: http
          containerPort: 80
        imagePullPolicy: IfNotPresent
      nodeSelector:
        kubernetes.io/os: linux
```

---

## ▶️ Deploy the app

```bash id="deploycmd1"
kubectl apply -f deploy.yml
```

---

## 📄 Copy the below file as service.yml

```yaml id="svcfile1"
apiVersion: v1
kind: Service
metadata:
  name: eks-sample-linux-service
  labels:
    app: eks-sample-linux-app
spec:
  selector:
    app: eks-sample-linux-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

---

## 🌐 Deploy the service

```bash id="deploysvc1"
kubectl apply -f service.yml
```
