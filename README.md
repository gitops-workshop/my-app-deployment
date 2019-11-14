# GitOps Ninja

### 1. Install Kustomize

```
brew install kustomize
kustomize version
```

### 2. Fork the example

* Open https://github.com/gitops-workshop/my-app-deployment
* Click “Fork”. 
* Then, open your terminal and run:

```bash
git clone git@github.com:<username>/my-app-deployment.git
```

Note: The above URL should start with "git@" and you'll need to enter your username.

### 3. Create your app environment

```bash
mkdir -p my-app-deployment/my-app && cd my-app-deployment/my-app
```

```bash
cat >./kustomization.yaml <<EOL
bases:
  - ../base
EOL
```

```bash
kustomize build .
```

### 4. Change the name prefix

```
kustomize edit set nameprefix <yourusername>-
git diff
kustomize build
git commit -am "set name prefix"
git push
```

### 5. Open Argo CD and create your app

* Open https://argo-cd-kubecon.apps.argoproj.io/
* Click "Login Via Github"
* Click "New application"

| Field | Value |
|-------|-------|
| Application name: | `<username>` |
| Project: | default |
| Sync policy: | Manual |
| Repository: | `https://github.com/<username>/my-app-deployment` |
| Revision: | HEAD |
| Path: | my-app |
| Cluster: | https://kubernetes.default.svc |
| Namespace: | default |
  
### 6. Sync your app

Click "Sync".

### 7. Upgrade your app

```
kustomize edit set image gitopsworkshop/my-app:v2
git diff
kustomize build
git commit -am "upgrade to version 2"
git push
```

* Detect git changes: "Refresh"
* Preview Differences: "App Diff"
* Deploy New Version: "Sync"

### 8. Proper GitOps rollback

```
git revert $(git rev-parse HEAD)
git push
```
