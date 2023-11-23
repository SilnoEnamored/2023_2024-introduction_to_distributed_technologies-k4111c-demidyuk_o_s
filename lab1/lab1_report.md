University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)    
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)     
Year: 2023  
Group: K4111c  
Author: Demidyuk Oleg Sergeevich  
Lab: Lab1  
Date of create: 20.11.2023 
Date of finished: 22.11.2023

### Цель работы:

Ознакомиться с инструментами Minikube и Docker, развернуть свой первый "под".

### Ход работы:

1. Docker и Minikube установлены на компьютер.

```bash
$ docker version
Client: Docker Engine - Community
 Version:           24.0.7
 API version:       1.43
 Go version:        go1.20.10
 Git commit:        afdd53b
 Built:             Thu Oct 26 09:07:41 2023
 OS/Arch:           linux/amd64
 Context:           default
```
```bash
$ minikube version
minikube version: v1.32.0
commit: 8220a6eb95f0a4d75f7f2d7b14cef975f050512d
```

2. Кластер Minikube запущен на Docker контейнере.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/1.jpg)

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/2.jpg)

3. Содзан манифест создания пода first-pod.yaml.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: "vault"
  namespace: default
  labels:
    app: "vault"
spec:
  containers:
    - name: vault
      image: "vault:1.13.3"
      ports:
        - containerPort: 8200
          name: http
```

