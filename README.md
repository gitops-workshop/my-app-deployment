# Hands-On

### 1. Install Kustomize

```
brew install kustomize
kustomize version
```

### 2. Fork The Example Repo

* Open https://github.com/gitops-workshop/my-app-deployment
* Click “Fork”. 

### 3. Clone Your Fork

In your terminal:

```bash
export username=... ;# your Github username in lowercase
git clone git@github.com:${username}/my-app-deployment.git
cd my-app-deployment
```

### 4. Build The Base

```
kustomize build base
```

Note: The above URL should start with "git@" and you'll need to enter your username.

### 5. Create An Overlay

```bash
mkdir -p overlays/dev
```

```bash
cat > overlays/dev/kustomization.yaml <<EOL
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
resources:
  - ../../base
EOL
```

```bash
kustomize build overlays/dev
git add . && git commit -am "add dev overlay"
git push
```

### 6. Change The Name Prefix

```
cd overlays/dev
kustomize edit set nameprefix ${username}-
git diff
kustomize build
git commit -am "set name prefix"
git push
```

### 7. Open Argo CD And Create Your App

* Open https://argo-cd-kubecon.apps.argoproj.io/
* Click "Login Via Github"
* Click "New application"

| Field | Value |
|-------|-------|
| Application name: | `${username}` |
| Project: | `default` |
| Sync policy: | `Manual` |
| Repository: | `https://github.com/${username}/my-app-deployment` |
| Revision: | `HEAD` |
| Path: | `overlays/dev` |
| Cluster: | `https://kubernetes.default.svc` |
| Namespace: | `default` |
  
### 8. Sync Your App

* Click "Sync".
* Click "Synchronize" in the Sliding panel.

### 9. Upgrade Your App

```
kustomize edit set image gitopsworkshop/my-app:v2
git diff
kustomize build
```

```
git commit -am "upgrade to version 2"
git push
```

* Detect git changes: "Refresh"
* Preview Differences: "App Diff"
* Deploy New Version: "Sync"

### 10. Troubleshoot Degraded App

1. Open app
2. Find the red heart
3. Clik on the resource and check each tab

### 11. Emergency Rollback

* Click "History And Rollback"
* Click "..." button in the last row
* Click "Rollback"
* Click "Ok" in the modal panel

### 12. GitOps Rollback

```
git revert $(git rev-parse HEAD)
git push
```
