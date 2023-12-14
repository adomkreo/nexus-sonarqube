# nexus
1- Install helm: sudo snap install helm -- classic
2- Create a namespace (sonarqube)
3- helm repo add sonatype https://sonatype.github.io/helm3-charts/
4- helm repo update
5- helm install nexus sonatype/nexus-repository-manager -n sonarqube
6- create ingress (after creating A) record


apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nexus-ingress 
spec:
  ingressClassName: nginx
  rules:
  - host: nexus.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nexus-rm-nexus-repository-manager
            port:
              number: 8081
  #############################################################
  ###############################################################
  # sonarqube 9.9 LTS CHART
1- helm repo add sonatype https://sonatype.github.io/helm3-charts/ 
2- helm repo update
3- helm upgrade --install -n sonarqube --version ~8 sonarqube sonarqube/sonarqube
4- create ingress (after creating A record)

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sonarqube-ingress 
spec:
  ingressClassName: nginx
  rules:
  - host: sonarqube.smartuniversaldevops.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: sonarqube-sonarqube
            port:
              number: 9000
  ###################################################################################

