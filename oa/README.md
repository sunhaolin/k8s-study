## 安装及配置 NFS 服务（Ubuntu 18.04）

```bash
sudo apt update
sudo apt install nfs-kernel-server
sudo mkdir -p /srv/kubedata
```

```bash
sudo vim /etc/exports
```

添加 **/srv/kubedata 192.168.0.0/24(rw,sync,no_subtree_check,crossmnt,fsid=0,no_root_squash)**

保存文件并且导出分享:`sudo exportfs -ra`

想要查看当前活跃的导出和它们的状态，使用：`sudo exportfs -v`

防火墙配置:

```bash
sudo ufw allow from 192.168.0.0/24 to any port nfs
```

想要验证修改，运行：`sudo ufw status`

其他服务器安装 NFS 客户端
在其他客户端的机器上，安装需要挂载远程 NFS 文件系统的工具.

```bash
sudo apt update
sudo apt install nfs-common
```

在某个客户端测试

```
sudo mkdir -p /testmountnfs
sudo mount -t nfs -o vers=4 192.168.0.153:/srv/kubedata/ /testmountnfs
cd /testmountnfs/
sudo touch test.txt
```

到 nfs 服务器端 **/srv/kubedata** 下查看

## 创建 nfs-provisioner
需要准备docker镜像，**quay.io/external_storage/nfs-client-provisioner:latest**

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

```
db.getMongo().setSlaveOk();
```

## 启动 oa
