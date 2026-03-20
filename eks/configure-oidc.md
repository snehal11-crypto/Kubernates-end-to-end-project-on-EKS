# 🔗 commands to configure IAM OIDC provider

---

## 📌 Set cluster name

```bash id="c1x9pl"
export cluster_name=demo-cluster
```

---

## 🔍 Get OIDC ID

```bash id="o7k2mz"
oidc_id=$(aws eks describe-cluster --name $cluster_name --query "cluster.identity.oidc.issuer" --output text | cut -d '/' -f 5)
```

---

## ✅ Check if IAM OIDC provider already exists

```bash id="q8v3ne"
aws iam list-open-id-connect-providers | grep $oidc_id | cut -d "_
```
