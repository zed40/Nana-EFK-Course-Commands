## Set up elastic stack in kubernetes cluster

###### Deploy and access Dasboard

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml

    kubectl proxy 

    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.


###### create docker-registry secret for dockerHub

    DOCKER_REGISTRY_SERVER=docker.io
    DOCKER_USER=your dockerID, same as for `docker login`
    DOCKER_EMAIL=your dockerhub email, same as for `docker login`
    DOCKER_PASSWORD=your dockerhub pwd, same as for `docker login`

    kubectl create secret docker-registry myregistrysecret \
    --docker-server=$DOCKER_REGISTRY_SERVER \
    --docker-username=$DOCKER_USER \
    --docker-password=$DOCKER_PASSWORD \
    --docker-email=$DOCKER_EMAIL

### Install Elastic Stack (EFK) Elastic, FluentD, Kibana

##### install elastic search chart 

    helm repo add elastic https://Helm.elastic.co
    helm install elasticsearch elastic/elasticsearch -f values-linode.yaml

##### install Kibana chart

    helm install kibana elastic/kibana

##### access Kibana locally

    kubectl port-forward deployment/kibana-kibana 5601

    access: localhost:5601

##### install ingress controller from k8s

    helm repo add stable https://kubernetes-charts.storage.googleapis.com/
    helm install nginx-ingress stable/nginx-ingress --set controller.publishService.enabled=true

##### install Fluentd

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install my-release bitnami/fluentd


### Other useful commands

##### restart Fluentd deamonSet

    kubectl rollout restart daemonset/fluentd

##### restart elastic search statefulSet

    kubectl rollout restart statefulset/elasticsearch-master



