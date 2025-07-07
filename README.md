# Argo CD Bootstrap for K3s Cluster

This repository bootstraps a GitOps-based deployment for a K3s cluster using [Argo CD](https://argo-cd.readthedocs.io/). It includes:

- Ansible playbook to install and configure Argo CD.
- GitOps folder structure to manage cluster applications declaratively.
- Initial setup of core infrastructure apps like NGINX Ingress Controller and Sealed Secrets.

---

## 📁 Repository Structure

```
.
├── ansible
│   ├── ansible.cfg                  # Local Ansible config
│   ├── playbooks
│   │   └── setup-argocd.yml         # Entry point to install Argo CD
│   └── roles
│       └── argocd
│           └── tasks
│               └── main.yml         # Tasks to install Argo CD via kubectl
├── bootstrap
│   └── root-application.yaml        # Argo CD root Application to manage everything
└── clusters
    └── k3s                          # Folder for the 'k3s' cluster
        └── infra
            ├── argocd
            │   ├── application.yaml
            │   └── values.yaml      # Self managed ArgoCD
            └── sealed-secrets
                ├── application.yaml
                └── values.yaml      # Sealed Secrets controller via Helm
```

---

## 🚀 Getting Started

1. **Provision your cluster (separate Terraform repo)**
   - Set up an OCI VM and install K3s (already handled separately).

2. **Install Argo CD**
   - Run the Ansible playbook:
     ```bash
     ansible-playbook -i <inventory> ansible/playbooks/setup-argocd.yml
     ```

3. **Bootstrap GitOps**
   - Once Argo CD is running, it will automatically sync `bootstrap/root-application.yaml`.
   - This sets up core infrastructure via Helm-based Argo CD Applications.

---

## 📝 TODO

- [ ] Add ExternalDNS + DuckDNS for dynamic subdomain mapping.
- [ ] Add Dex for SSO login to Argo CD.
- [ ] Configure Argo CD Ingress with HTTPS via Let's Encrypt.
- [ ] Add app layer (e.g., whoami, jobwinner).
- [ ] Enable RBAC policies if needed for multi-user environments.

---

## 🧠 Notes

- This repo assumes a **single cluster** named `k3s`.
- All cluster resources (infra/apps) are defined declaratively under `clusters/k3s`.
- Secrets should be encrypted using **Sealed Secrets**.

---

## 🔗 References

- [Argo CD Official Docs](https://argo-cd.readthedocs.io/)
- [Bitnami Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)

---

