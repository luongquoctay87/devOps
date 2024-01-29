# Install ArgoCD on macOS

### Requirements
- Installed kubectl command-line tool.
- Have a `kubeconfig` file (default location is ~/.kube/config).
- CoreDNS. Can be enabled for microk8s by microk8s enable dns && microk8s stop && microk8s start

### 1. Install Argo CD on macOS

```bash
$ kubectl apply -n default -f argocd-install.yaml
or
$ kubectl apply -n default -f argocd-core-install.yaml
```

### 2. Download Argo CD CLI
```bash
$ brew install argocd
```

### 3. Access The Argo CD API Server¶
By default, the Argo CD API server is not exposed with an external IP. 
To access the API server, choose one of the following techniques to expose the Argo CD API server:

- Service Type Load Balancer
Change the argocd-server service type to LoadBalancer:
    ```bash
    $ kubectl patch svc argocd-server -n default -p '{"spec": {"type": "LoadBalancer"}}'
    ```

- Port Forwarding:

    Kubectl port-forwarding can also be used to connect to the API server without exposing the service.
    ```bash
    $ kubectl port-forward svc/argocd-server -n default 8283:443
    ```

- The API server can then be accessed using https://localhost:8283

### 4. Login Using The CLI
 The initial password for the admin account is auto-generated and stored as clear text in the field password in a secret named argocd-initial-admin-secret in your Argo CD installation namespace. 
 You can simply retrieve this password using the argocd CLI:
```bash
$ argocd admin initial-password -n default
```

Using the username admin and the password from above, login to Argo CD's IP or hostname:
```bash
$ argocd login <ARGOCD_SERVER>
```

Change the password using the command:
```bash
$ argocd account update-password
```


### 5. Register A Cluster To Deploy Apps To
- Creating Apps Via CLI

  First we need to set the current namespace to argocd running the following command:
  ```bash
  $ kubectl config set-context --current --namespace=default
  ```

  Create the example guestbook application with the following command:
  ```bash
  $ argocd app create auth --repo https://github.com/luongquoctay87/authentication.git --path auth --dest-server https://kubernetes.default.svc --dest-namespace default
  ```
  
- Creating Apps Via UI:
  
  Follow step by step in [6. Create An Application From A Git Repository](https://argo-cd.readthedocs.io/en/stable/getting_started/)

---
Reference source:
  - [Argo CD - Declarative GitOps CD for Kubernetes](https://argo-cd.readthedocs.io/en/stable/getting_started/)
