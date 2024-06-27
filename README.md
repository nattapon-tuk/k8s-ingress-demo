Chapter 14. Ingress

<!-- 
Ingress Controller::

ingress resource definition manifest needs to include an ingress class annotation with the name of the desired controller kubernetes.io/
ingress.class: "nginx" (for an nginx ingress controller).

Starting the Ingress Controller in Minikube is extremely simple. Minikube ships with the Nginx Ingress Controller set up as an addon, disabled by default. It can be easily enabled by running the following command:
$ minikube addons enable ingress 

-->

- minikube addons list
- minikube addons enable ingress (by default, nginx ingress addons)
- minikube tunnel
- kubectl create -f ingress-demo.yaml
  - kubectl apply -f ingress-demo.yaml
(- minikube service  blue-web green-web -n default )
- kubectl get ingress
-  kubectl describe ingress ingress-demo
   -  blue.example.local/ >> blue-web:80 (10.244.0.9:80)
   -  green.example.local/ >> green-web:80 (10.244.0.8:80)

- minikube service list
- minikube service  ingress-nginx-controller -n ingress-nginx
  - http://green.example.local:60433/
  - http://blue.example.local:60433/
  - https://green.example.local:60434/
  - https://blue.example.local:60434/
- alternatively, minikube tunnel
  - http://green.example.local/ or https://green.example.local/
  - http://blue.example.local/ or https://blue.example.local/

Try setting up blue green deployment
- kubectl replace --force -f ingress-demo.yaml

To try setting up ingress rule for Argo CD server for accessing publicly
- kubectl -n argocd expose deployment argocd-server --type=NodePort
- kubectl create service nodeport argocd-server-pub --tcp=80:80 -n argocd --dry-run=client -o yaml > argocd-service.yaml

- kubectl create secret tls temptest-tls --key ${KEY_FILE} --cert ${CERT_FILE}