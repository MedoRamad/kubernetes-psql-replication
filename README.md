# Postgresql Replication on Kubernetes
使用Kuberntes建立Postgresql 主從。
# Getting Started
#### 1. 設定Master環境
psql-master/templates/deployment.yaml
```sh
env:
            - name: POSTGRES_USER
              value: 'postgres'
            - name: POSTGRES_PASSWORD
              value: 'postgres'
            - name: POSTGRES_DB
              value: 'postgres_db'
            - name: PG_REP_USER
              value: 'repl'
            - name: PG_REP_PASSWORD
              value: 'repl'
```
#### 2. 設定Slave環境
psql-slave/templates/deployment.yaml
```sh
          env:
            - name: POSTGRES_USER
              value: 'postgres'
            - name: POSTGRES_PASSWORD
              value: 'postgres'
            - name: PG_REP_USER
              value: 'repl'
            - name: PG_REP_PASSWORD
              value: 'repl'
```
# Running
#### 1. 建立存儲區
```sh
kubectl create -f psql-master/master-hostpath.yml
kubectl create -f psql-master/master-pvc.yml
kubectl create -f psql-slave/slave-hostpath.yml
kubectl create -f psql-slave/slave-pvc.yml
```
#### 2. 啟動服務
```sh
cd psql-master
helm install psql-master .
cd psql-slave
helm install psql-slave .
```
