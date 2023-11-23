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

2.
