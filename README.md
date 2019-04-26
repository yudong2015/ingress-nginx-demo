
Ingress是一种Kubernetes资源，借助Nginx、Haproxy或云厂商的负载均衡器将Kubernetes集群内的Service暴露到集群外，本文将主要介绍ingress-nginx；

Kubernetes: 1.13.4

主机地址: 192.168.0.5(替换成自己的主机地址)

## Ingress-nginx structure

![structure]()


## Usage 

### 创建Namespace

```
$ kubectl create namespace test
```

### 部署default-backend

```
$ kubectl create -f default-backend.yaml
```

### 创建ConfigMap和ServiceAccount

```
$ kubectl create -f configmap.yaml

$ kubectl create -f serviceaccount.yaml
```

### 部署nginx-ingress-controller

```
$ kubectl create -f ingress-nginx-controller.yaml
```
### 创建ingress-nginx service

把nginx-ingress-controller（负载均衡器）暴露到Kubernetes集群外

```
$ kubectl create -f ingress-service.yaml
```

Test:

```
$ curl 192.168.0.5:80
default backend - 404
```

----

### 部署echo http service服务

```
$ kubectl create -f echo-service.yaml
```

### echo服务ingress配置（IP访问）

```
$ kubectl create -f echo-service-ingress.yaml
```

Test:

```
$ curl http://192.168.0.5/http-svc

Hostname: http-svc-6f459dc547-hng9d

Pod Information:
	node name:	minikube
	pod name:	http-svc-6f459dc547-hng9d
	pod namespace:	testsvc
	pod IP:	172.17.0.16

Server values:
	server_version=nginx: 1.13.3 - lua: 10008

Request Information:
	client_address=172.17.0.15
	method=GET
	real path=/http-svc
	query=
	request_version=1.1
	request_uri=http://192.168.0.5:8080/http-svc

Request Headers:
	accept=*/*
	connection=close
	host=192.168.0.5
	user-agent=curl/7.47.0
	x-forwarded-for=172.17.0.1
	x-forwarded-host=192.168.0.5
	x-forwarded-port=80
	x-forwarded-proto=http
	x-original-uri=/http-svc
	x-real-ip=172.17.0.1
	x-request-id=26a77529c60e672b69191a8e1d07ca32
	x-scheme=http

Request Body:
	-no body in request-

```

### echo服务ingress配置(域名访问)

```
$ kubectl create -f echo-service-domain-ingress.yaml
```

Test:

```
$ curl 192.168.0.5 -H "Host: http-svc.yudong.com"

Hostname: http-svc-6f459dc547-hng9d

Pod Information:
	node name:	minikube
	pod name:	http-svc-6f459dc547-hng9d
	pod namespace:	testsvc
	pod IP:	172.17.0.16

Server values:
	server_version=nginx: 1.13.3 - lua: 10008

Request Information:
	client_address=172.17.0.15
	method=GET
	real path=/http-svc
	query=
	request_version=1.1
	request_uri=http://192.168.0.5:8080/http-svc

Request Headers:
	accept=*/*
	connection=close
	host=192.168.0.5
	user-agent=curl/7.47.0
	x-forwarded-for=172.17.0.1
	x-forwarded-host=192.168.0.5
	x-forwarded-port=80
	x-forwarded-proto=http
	x-original-uri=/http-svc
	x-real-ip=172.17.0.1
	x-request-id=26a77529c60e672b69191a8e1d07ca32
	x-scheme=http

Request Body:
	-no body in request-

```

