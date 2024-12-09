University: ITMO University  
Faculty: FICT  
Course: Introduction to distributed technologies  
Year: 2024/2025  
Group: K4111c  
Author: Doroshenko Daniil Vadimovich  
Lab: Lab3  
Date of create: 9.12.2024  
Date of finished: 12.11.2024

Ход работы:  
1. Создадим ConfigMap для передачи переменных.  
ConfigMap — это объект, который хранит значения переменных и обычно используется для передачи конфигурационных данных в поды при этом типы данных могут разные и строки, и бинарные файлы.
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: configmap-for-react
data:
  REACT_APP_USERNAME: "Doroshenko Daniil"
  REACT_APP_COMPANY_NAME: "ITMO"
```
3. Создадим ReplicaSet и передадим переменные с помощью ConfigMap  
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: itdt-frontend
  labels:
    app: itdt
spec:
  replicas: 2
  selector:
    matchLabels:
      app: itdt-frontend
  template:
    metadata:
      labels:
        app: itdt-frontend
    spec:
      containers:
      - name: itdt-container
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        envFrom:
        - configMapRef:
            name: configmap-for-react
```
5. Включим Ingress контроллер "minikube addons enable ingress"
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209151940.png)
6. Сгенерируем сертификат

SSL-сертификат — это файл, который устанавливается на сервере с сайтом. Он содержит цифровую подпись издателя, открытый ключ и имя владельца сертификата, название центра сертификации.  SSL-сертификаты нужны для обеспечения безопасности пользовательских данных, подтверждения права собственности на веб-сайт, предотвращения создания поддельных версий сайта.  Если веб-сайт просит пользователей войти в систему или ввести личные данные, SSL поможет сохранить конфиденциальность сессии. Он также гарантирует, что веб-сайт является подлинным.  
Самое главное — сертификат нужен, чтобы веб-сайт мог работать через шифрование HTTPS. Если сертификат отсутствует, то обмен данными осуществляется через HTTP.
HTTPS создает зашифрованное соединение между браузером пользователя и веб-сервером, с которым он общается. SSL-сертификаты необходимы для установки этого зашифрованного соединения.

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt -subj "/C=RU/ST=SPB/L=Saint-Petersburg/O=ITMO-Organization/OU=FICT/CN=itdt-application.daniildoroshenko.com"
```
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209152112.png)

5. Импортируем сертификат


Команда kubectl create secret tls используется для создания Kubernetes-секрета типа tls, который хранит TLS-сертификат и соответствующий ему приватный ключ.  
secret tls - Указывает тип создаваемого секрета. tls предназначен специально для хранения TLS-сертификатов и приватных ключей, которые используются для настройки HTTPS в Kubernetes.  
tls-sertificat - Имя создаваемого секрета. Это имя, на которое будем ссылаться в манифестах Ingress.  
--cert=certificate.crt - Указывает путь к файлу, содержащему публичный сертификат. Этот файл должен быть в формате PEM, который используется для SSL/TLS.  
--key=private.key - Указывает путь к файлу, содержащему приватный ключ. Этот ключ должен соответствовать указанному сертификату.  

```bash
kubectl create secret tls tls-sertificat --cert=certificate.crt --key=private.key
```

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209153425.png)
  
6. Пропишем в /etc/hosts доменное имя и IP


![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209153351.png)  
7. Зпустим сервис (ClusterIP)  
ClusterIP — это тип сервиса, который создаёт виртуальный IP-адрес внутри кластера Kubernetes, на который могут направляться запросы из других подов в этом же кластере. То есть, сервис доступен только внутри самого кластера и не имеет публичного доступа извне. Это основной способ взаимодействия между компонентами внутри кластера, когда внешний доступ не требуется.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: itdt-frontend
  ports:
  - port: 3000
    targetPort: 3000
```
8. Запустим Ingress контроллер  
Ingress контроллер — это компонент в Kubernetes, который управляет входящим трафиком и направляет его к нужным сервисам внутри кластера.  
Когда создается Ingress, необходимо задать правила маршрутизации, например, чтобы запросы на определённый домен или путь направлялись на конкретный сервис. Ingress контроллер — это по сути веб-сервер, который смотрит заголовки запросов и маршрутизирует трафик на разные домены. Он принимает запросы снаружи, а затем передаёт их в Kubernetes на соответствующие сервисы, внутри кластера.
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: itdt-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  tls:
  - hosts:
      - itdt-application.daniildoroshenko.com
    secretName: tls-sertificat
  rules:
  - host: itdt-application.daniildoroshenko.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 3000
```
9. Проверим работу приложения
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209163644.png)
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209163705.png)
10 Создадим схему данной лабораторной работы

