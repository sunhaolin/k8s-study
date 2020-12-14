## 安装及配置 NFS 服务

## 创建 nfs-provisioner

```
kubectl apply -f serviceaccount.yaml
kubectl apply -f  service-rbac.yaml
kubectl apply -f nfs-provisioner-deploy.yaml
kubectl get pod -l app=nfs-provisioner
```

## 配置 storageclass 存储类

```
kubectl apply -f storageclass.yaml
```

## 部署 mongoDB 资源

```
kubectl apply -f mongo_sta_nodeport.yaml
```

```
kubectl exec -it mongo-0 -- mongo
```

##
