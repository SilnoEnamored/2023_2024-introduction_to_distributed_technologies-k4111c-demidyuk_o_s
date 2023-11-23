University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)    
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)     
Year: 2023  
Group: K4111c  
Author: Demidyuk Oleg Sergeevich  
Lab: Lab3  
Date of create: 23.11.2023 
Date of finished: 30.11.2023

### Цель работы:

Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

### Ход работы:

1. Создан шаблон configMap configmap.yaml с необходимыми переменными.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: react-cm
data:
  react_app_username: "silno"
  react_app_company_name: "itmo"
```

2. ConfigMap создан в кластере Minikube.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/1.jpg)

3. Создан шаблон replicaSet, использующий значения из созданной configMap.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: react-rs
spec:
  replicas: 2
  selector:
    matchExpressions:
      - key: app
        operator: In
        values:
          - frontend
  template:
    metadata:
      labels:
        creation_method: replicaSet
        app: frontend
    spec:
      containers:
        - image: ifilyaninitmo/itdt-contained-frontend:master
          name: react
          env:
            - name: REACT_APP_USERNAME
              valueFrom:
                configMapKeyRef:
                  name: react-cm
                  key: react_app_username
            - name: REACT_APP_COMPANY_NAME
              valueFrom:
                configMapKeyRef:
                  name: react-cm
                  key: react_app_company_name
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
```

4. ReplicaSet развернут в кластере Minikube.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/2.jpg)

5. Включен minikube addons enable ingress и сгенерирован TLS сертификат.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/3.jpg)

```yaml
$ openssl req -new -newkey rsa:4096 -x509 -sha256 -days 365 -nodes -out minikube.crt -keyout minikube.key
...
Country Name (2 letter code) [XX]:RU
State or Province Name (full name) []:SPB
Locality Name (eg, city) [Default City]:SPB
Organization Name (eg, company) [Default Company Ltd]:ITMO
Organizational Unit Name (eg, section) []:LOMO9
Common Name (eg, your name or your server's hostname) []:SILNO
```

6. Сертификат импортирован в Minikube.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/5.jpg)

7. Создан шаблон ingress с использованием tls сертификата.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-services
spec:
  tls:
    - hosts:
        - kube.example.com
      secretName: ingress-tls
  rules:
    - host: kube.example.com
      http:
        paths:
          - pathType: Prefix
            path: /react
            backend:
              service:
                name: react-np
                port:
                  number: 80
```

8. Ingress развернут в кластере Minikube.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/5.jpg)

9. В hosts прописан FQDN и IP адрес созанного ingress.

```yaml
$ sudo nano /etc/hosts
...
192.168.49.2 kube.example.com
```

10. Осуществленна проверка наличия сертификата веб приложения.

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/7.jpg)

11. Схема

![Image text](https://github.com/SilnoEnamored/2023_2024-introduction_to_distributed_technologies-k4111c-demidyuk_o_s/raw/main/lab3/screenshots/8.jpg)

### Вывод: В результате выполнения лаборатнорной работы в ранее созданном кластере Minikube был поднят ингресс с использованием фейкового tls сертификата. Рассмотрены методы хранения конфигураций при помощи configMap, а также секретов minikube.
