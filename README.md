# 📊 Démo : Monitoring Kubernetes avec Uptime Kuma

Ce guide contient le déroulé pas à pas pour la présentation du déploiement d'une solution de monitoring native sur Kubernetes.

---

## 🛠️ 1. Préparation du Cluster

On commence par ajouter le dépôt officiel et préparer l'environnement.

**Ajout du repo Helm :**

```bash
helm repo add uptime-kuma https://dirsigler.github.io/uptime-kuma-helm/
helm repo update
```

**Déploiement du service de test (Nginx) :**

```bash
kubectl apply -f setup-demo.yaml
```

---

## 🏗️ 2. Déploiement d'Uptime Kuma

On utilise le fichier `values.yaml` pour configurer la persistance et l'accès.

```bash
helm upgrade --install uptime-kuma uptime-kuma/uptime-kuma \
  --namespace monitoring \
  --create-namespace \
  -f values.yaml
```

---

## 🌐 3. Configuration des Sondes

Accédez aux interfaces via votre navigateur :

* **Uptime Kuma :** [http://localhost:30001](https://www.google.com/search?q=http://localhost:30001)
* **Nginx Demo :** [http://localhost:30002](https://www.google.com/search?q=http://localhost:30002)

**Sondes à ajouter dans l'interface UI :**

1. **Sonde Interne (DNS K8s) :** `http://nginx-demo.monitoring.svc.cluster.local`
2. **Sonde Externe (Web) :** `https://www.cloudnativedays.fr/`

---

## 🔔 4. Configuration de l'Alerting Slack

Dans l'interface Uptime Kuma, allez dans **Paramètres > Notifications > Configurer une notification**.

| Champ | Valeur |
| --- | --- |
| **Type de notification** | Slack |
| **Nom d'affichage** | `Indisponibilité du service NGINX` |
| **Webhook URL** | `` |
| **Nom d'utilisateur** | `Uptime Kuma` |
| **Nom du salon (Channel)** | `k8s` |
| **Emoji icône** | `:sauropod:` (optionnel) |

---

## 🧪 5. Le Crash Test (Simulation)

Démonstration de la réactivité d'Uptime Kuma et de l'envoi de la notification Slack vers le canal dédié.

**🔴 Simuler une coupure :**

```bash
kubectl scale --replicas=0 deploy/nginx-demo -n monitoring
```

**🟢 Rétablir le service :**

```bash
kubectl scale --replicas=1 deploy/nginx-demo -n monitoring
```

---

## 🏁 6. Conclusion

* **Simplicité :** Déploiement via Helm en une commande.
* **Persistance :** Données sauvegardées malgré les redémarrages (Volume 2Gi).
* **DNS Natif :** Surveillance des micro-services via le réseau interne du cluster.
* **Alerting Push :** Notification immédiate sur les outils collaboratifs.

---

### 🔍 Aide & Debug

* **Alias :** `alias k="kubectl"`
* **Set Namespace :** `kubectl config set-context --current --namespace=monitoring`
* **Logs :** `k logs -f deploy/uptime-kuma -n monitoring`
* **Pods :** `k get po -n monitoring`