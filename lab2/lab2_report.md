University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)    
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)     
Year: 2023  
Group: K4111c  
Author: Demidyuk Oleg Sergeevich  
Lab: Lab2  
Date of create: 23.11.2023 
Date of finished: 23.11.2023

### Цель работы:

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

### Ход работы:

1. Создан манифест dep.yaml с двумя репликами предложенного контейнера.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-d
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
  template:
    metadata:
      labels:
        app: frontend
    spec:
      containers:
      - name: react
        image: ifilyaninitmo/itdt-contained-frontend:master
        env:
        - name: REACT_APP_USERNAME
          value: SILNO
        - name: REACT_APP_COMPANY_NAME
          value: itmo
        ports:
        - name: http
          containerPort: 3000
          protocol: TCP
```

2. dep.yaml развернут в Minikube кластере.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab2/screenshots/1.jpg)

3. Создан манифест serv.yaml сервиса для доступа к подам.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: react-np
spec:
  type: NodePort
  ports:
    - name: http
      protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 30123
  selector:
    app: frontend
```
4. Сервис поднят в Minikube кластере.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab2/screenshots/2.jpg)

5. С помощью проброса портов получен доступ к контейнерам через веб-браузер и роведена проверка, что переменные переданные контейнерам действительно передались.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab2/screenshots/3.jpg)

6. Проверены логи контейнеров.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab2/screenshots/4.jpg)

7. Схема.
    
![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab2/screenshots/5.jpg)

### Вывод:
В результате выаолнения лаборатнорной работы в ранее созданном кластере Minikube было развернуто два пода с помощью deployment, после чего к ним был предоставлен доступ через сервис. Также в ходе работы были изучены принципы работы Depolyment и ReplicaSet, а также принципы распределения трафика между контейнерами.
