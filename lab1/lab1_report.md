University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)    
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)     
Year: 2023  
Group: K4111c  
Author: Demidyuk Oleg Sergeevich  
Lab: Lab1  
Date of create: 20.11.2023 
Date of finished: 23.11.2023

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
4. Под поднят в Minikube кластере.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/3.jpg)  

5. Создан сервис для доступа к поду.
   
```yaml
apiVersion: v1
kind: Service
metadata:
  name: vault
  namespace: vault
  labels:
    run: vault
spec:
  ports:
  - port: 8200
    targetPort: 8200
    protocol: TCP
  selector:
    app: vault
```
![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/4.jpg)

6. Получен токен для входа в Vault с помощью просмотра логов контейнера.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/6.jpg)

7. Произведен проброс портов c локальной машины в контейнер через сервис.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/5.jpg)

8. Произведен вход в Vault.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/7.jpg)

9. Схема.
    
![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab1/screenshots/8.jpg)

10. Ответы на вопросы:
1. Мы развернули приложение для хранения секретов в кубере. Для этого развернут один деплоймент с двумя подами и 1 сервис для подключения к нему.
2. Токен находится в логах контейнера

### Вывод:
В результате выполнения лаборатнорной работы был создан тестовый kubernetes кластер с помощью minikube, в котором был развернуты один под и сервис, после чего к Pod был получен удаленный доступ с помощью проброса портов. Также в ходе работы были изучены принципы формирования соединений при создании пода, сервиса и использовании проброса портов.
