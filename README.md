# palo-deploy-k8s
使用K8S部署Apache Doris (incubating)（原百度palo）

## 快速部署PALO集群

```bash
# 创建namespace
kubectl apply -f palo-ns.yaml

# 部署FE
kubectl apply -f palo-fe.yaml

# 部署BE
kubectl apply -f palo-be.yaml
```

## 配置PALO集群

通过mysql客户端连接FE控制台：

```bash
# 配置BE
mysql -h <NODE_IP> -P 29030 -uroot
```

在FE控制台中配置palo集群：

```sql
# 查看FE
SHOW PROC '/frontends';

# 查看BE
ALTER SYSTEM ADD BACKEND "<CLUSTER_IP>:9050";
SHOW PROC '/backends';
```

通过浏览器访问Palo Web页面：http://<NODE_IP>:28030/

## PALO镜像

有三种获取方式：

1. 下载镜像，载入本地

```bash
docker load < image-palo-fe_<tag>.tar.gz
docker load < image-palo-be_<tag>.tar.gz
```

2. 下载编译好的压缩包，自己build镜像

```bash
# FE
cd palo-deploy-k8s/docker/fe/
wget fe_v0.9.21.tar.gz
tar zxf fe_v0.9.21.tar.gz
wget jdk-8u212-linux-x64.tar.gz
docker build -t palo-fe:<tag> .

# BE
cd palo-deploy-k8s/docker/be/
wget be_v0.9.21.tar.gz
tar zxf be_v0.9.21.tar.gz
docker build -t palo-be:<tag> .
```

3. 自己编译PALO、打镜像，编译过程略。
