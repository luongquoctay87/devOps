# [Helm Chart and Ingress Resource](https://stackoverflow.com/questions/72229854/helm-charts-and-ingress-resources)

helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace code-base --create-namespace

kubectl -n code-base get pod -o yaml



  ---
  Reference: 
  - https://kubernetes.github.io/ingress-nginx/deploy/

  - https://docs.nginx.com/nginx-ingress-controller/installation/installing-nic/installation-with-helm/