# postgres-grafana

# 1. Install Kube-Prometheus-Stack on cluster:  

Reference Documentation is as below:  

https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

This will installs the kube-prometheus stack, a collection of Kubernetes manifests, Grafana dashboards, and Prometheus rules combined with documentation and scripts to provide easy to operate end-to-end Kubernetes cluster monitoring with Prometheus using the Prometheus Operator.

Below Steps needs to be followed:

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
    kubectl create ns metrics
    kubectl config set-context --current --namespace=metrics

    helm upgrade --install grafana prometheus-community/kube-prometheus-stack

# 2. Install Postgres database on cluster:  

    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install postgresql-dev bitnami/postgresql
    kubectl get pods 
    kubectl exec -it pod/postgresql-dev-0 sh

    kubectl get secret/postgresql-dev -oyaml

    echo OTYwV0FRemZHZA== | base64 -d
    POSTGRES_PASSWORD="960WAQzfGd"
    PGPASSWORD="$POSTGRES_PASSWORD" psql --host 127.0.0.1 -U postgres -d postgres -p 5432

    CREATE DATABASE testdb;

    CREATE TABLE hello_world 
    (
    region      text,
    country     text,
    year        int,
    production  int,
    consumption int
    );

    INSERT INTO hello_world (region, country, year, production, consumption) VALUES ('America', 'USA', 1998, 2014, 12897);



# 3. Install Prometheus Postgres Exporter  

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update

    helm upgrade --install postgres-exporter prometheus-community/prometheus-postgres-exporter -f postgress-exporter-values.yaml

# 4. Install Prometheus Mysql Exporter Dashboard  

    https://grafana.com/grafana/dashboards/12485-postgresql-exporter/

    Add the below id as Grafana Dashboards and monitor Postgres DB 
    
        12485