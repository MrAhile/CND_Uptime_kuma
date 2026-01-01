### Alias

```bash
alias k="kubectl"
```

### Deploy Helm

```bash
helm upgrade --install uptime-kuma uptime-kuma/uptime-kuma \
  --namespace monitoring \
  --create-namespace \
  -f values.yaml

```

### Deploy nginx

```bash
kubectl apply -f setup-demo.yaml
# Vérification rapide du DNS interne
k get svc -n monitoring

```

### Check

```bash
k get po -n monitoring

k get svc -n monitoring

k logs -f deploy/uptime-kuma -n monitoring

```

### Services

```bash
k scale --replicas=0 deploy/nginx-demo -n monitoring

# Rétablir le service (Up)
k scale --replicas=1 deploy/nginx-demo -n monitoring
```

#### Delete

```bash
helm uninstall uptime-kuma --namespace monitoring
k delete -f setup-demo.yaml
```

---

### Liens

* **Accès UI :** `http://<NODE_IP>:30001`
* **Import :** `Settings` > `Import/Backup` > Choisir le fichier `.json`.
* **DNS interne :** `http://nginx-demo.monitoring.svc.cluster.local`