# palo-deploy-k8s
使用K8S部署Apache Doris (incubating)（原百度palo）

## 快速部署PALO集群

```bash
# 创建namespace
kubectl apply -f palo-ns.yaml

# 部署BE
kubectl apply -f palo-be.yaml
```

## PALO镜像

有三种获取方式：

1. 下载镜像，载入本地

```bash
docker load < image-palo-be_v0.9.21-2.tar.gz
```

2. 下载编译好的压缩包，自己build镜像

```bash
wget be_v0.9.21.tar.gz
tar zxf be_v0.9.21.tar.gz
cd palo-deploy-k8s/docker/be/
docker build -t palo-be:v0.9.21-2 .
```

3. 自己编译PALO、打镜像，编译过程略。