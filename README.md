# [Backstage](https://backstage.io)

This is your newly scaffolded Backstage App, Good Luck!

To start the app, run:

```sh
yarn install
yarn start
```

---

### Setup

Create a new backstage app:
```bash
npx @backstage/create-app@latest
```

Start backstage:
```bash
yarn start
```

In the backend directory, add the GitHub auth provider:
```bash
yarn --cwd packages/backend add @backstage/plugin-auth-backend-module-github-provider
```

---

Reference:

https://github.com/backstage/backstage/blob/master/packages/catalog-model/examples/acme/team-a-group.yaml

Install mkdocs:
```bash
pipx install mkdocs-techdocs-core --include-deps --force
```


---

### Kubernetes

Add bitnami helm repo:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

Install postgresql:
```bash
helm upgrade -i postgresql bitnami/postgresql \
  --set global.postgresql.auth.postgresPassword=backstage \
  --set global.postgresql.auth.username=backstage \
  --set global.postgresql.auth.password=backstage \
  --set global.postgresql.auth.database=backstage \
  --namespace backstage \
  --create-namespace
```

Apply the Kubernetes manifests:
```bash
kubectl apply -f charts/backstage-k8s.yaml
```

### Service Account

Create a service account with cluster-admin role:
```bash
kubectl -n backstage create serviceaccount backstage-sa
```

Create a secret for the service account:
```yaml
kubectl apply -f - << EOF
apiVersion: v1
kind: Secret
metadata:
  name: backstage-sa-token
  namespace: backstage
  annotations:
    kubernetes.io/service-account.name: backstage-sa
type: kubernetes.io/service-account-token
EOF
```

Create a ClusterRoleBinding for the service account:
```yaml
kubectl apply -f - << EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: backstage-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: backstage-sa
    namespace: backstage
EOF
```

Get the token for the service account:
```bash
kubectl -n backstage get secret backstage-sa-token -o jsonpath="{.data.token}" | base64 -d; echo
```
