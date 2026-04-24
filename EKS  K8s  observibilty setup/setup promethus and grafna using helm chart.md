### Prerequisites
- Download and Install AWS Cli - Please Refer [this]("https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html") link.
- Setup and configure AWS CLI using the `aws configure` command.
- Install and configure eksctl using the steps mentioned [here]("https://eksctl.io/installation/").
- Install and configure kubectl as mentioned [here]("https://kubernetes.io/docs/tasks/tools/").




```bash
eksctl create cluster --name=observability \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup
```
```bash
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster observability \
    --approve
```
```bash
eksctl create nodegroup --cluster=observability \
                        --region=us-east-1 \
                        --name=observability-ng-private \
                        --node-type=t3.medium \
                        --nodes-min=2 \
                        --nodes-max=3 \
                        --node-volume-size=20 \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access \
                        --node-private-networking

# Update ./kube/config file
aws eks update-kubeconfig --name observability
```




## 🗑️ 1) Helm completely remove (permanent)

### 🔹 Binary delete karo

```bash id="y0s8v2"
sudo rm -f /usr/local/bin/helm
```

Check:

```bash id="2m2v0p"
helm version
```

👉 “command not found” aaye to remove ho gaya

---

### 🔹 Helm ka local data bhi delete karo

```bash id="n6b8h1"
rm -rf ~/.config/helm ~/.cache/helm ~/.local/share/helm
```

---

## ⚠️ Optional (agar Helm se installs bhi hatani hain)

Helm uninstall sirf tool ko hataata hai; cluster ke andar jo releases (apps) install kiye the wo reh sakte hain.

List dekh lo:

```bash id="kq9l8e"
helm list -A
```

Uninstall example:

```bash id="o7c4d1"
helm uninstall monitoring -n monitoring
```

---

## ⬇️ 2) Helm fresh install (correct way)

### 🔹 Official script (easy method)

```bash id="w2x7k3"
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

### 🔹 Verify

```bash id="c9p3u5"
helm version
```

---


### 🧰 Step 2: Install kube-prometheus-stack
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### 🚀 Step 3: Deploy the chart into a new namespace "monitoring"
```bash
kubectl create ns monitoring
```
```bash
cd day-2

helm install monitoring prometheus-community/kube-prometheus-stack \
-n monitoring \
-f ./custom_kube_prometheus_stack.yml
```

### ✅ Step 4: Verify the Installation
```bash
kubectl get all -n monitoring
```
- **Prometheus UI**:
```bash
kubectl port-forward service/prometheus-operated -n monitoring 9090:9090
```

**NOTE:** If you are using an EC2 Instance or Cloud VM, you need to pass `--address 0.0.0.0` to the above command. Then you can access the UI on <instance-ip:port>

- **Grafana UI**: password is `prom-operator`
```bash
kubectl port-forward service/monitoring-grafana -n monitoring 8080:80
```
- **Alertmanager UI**:
```bash
kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093
```

### 🧼 Step 5: Clean UP
- **Uninstall helm chart**:
```bash
helm uninstall monitoring --namespace monitoring
```
- **Delete namespace**:
```bash
kubectl delete ns monitoring
```
- **Delete Cluster & everything else**:
```bash
eksctl delete cluster --name observability
```
