# LTHTR-Assessment
# PostgreSQL Kubernetes Deployment

## Overview
A minimal deployment of PostgreSQL in Kubernetes using **Helm** (Bitnami PostgreSQL chart) managed via **HelmFile**

### Prerequisites
- kubectl
- helm
- helmfile
- ArgoCD or Flux (optional, for GitOps)


### Brief explanation of your approach
- Helmfile → declaratively deploys both PostgreSQL and External Secrets Operator charts
- External Secrets Operator → fetches the PostgreSQL password from Azure Key Vault at runtime
- Helm chart existingSecret pattern → PostgreSQL uses a Kubernetes Secret instead of in-chart credentials
- GitOps → ArgoCD (or similar) monitors Git repo and syncs changes automatically to the cluster

### Security considerations implemented

1. Secret externalization: The Passwords are managed entirely outside and not stored in the GIT repo. They are pulled dynamically from Azure Key Vault at deploy time and encrypted.
2. TLS is enabled for the PostgreSQL connections ensuring encryption in transit between the clients and the database.

### How you would extend this with GitOps

The setup is gitOps-ready since both external-secrets and postgres Helm charts are managed together by Helmfile and are stored in Git. To further enhance the continuous deployment, the setup would be integrated with ArgoCD or FluxCD. This would automatically sync the helm chart and deploy the updated charts for every commit made.

### Any trade-offs you made

Improved security and GitOps compatibility, prioritizing secret externalization over simplicity.
