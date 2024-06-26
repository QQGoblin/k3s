# 说明

生成编译镜像
```shell
docker build --build-arg GOLANG=registry.lqingcloud.cn/library/golang:1.21.9-alpine3.18 \
-t registry.lqingcloud.cn/rancher/k3s-build:v1.28.10 \
-f ./custom/Dockerfile ./
```

编译 k3s
```shell
docker run --network=host \
-v $PWD:/src/k3s \
-v $GOPATH/pkg:/go/pkg \
registry.lqingcloud.cn/rancher/k3s-build:v1.28.10
```