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
2. Создадим ReplicaSet и передадим переменные с помощью ConfigMap
3. Включим Ingress контроллер "minikube addons enable ingress"
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209151940.png)
4. Сгенерируем сертификат
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt -subj "/C=RU/ST=SPB/L=Saint-Petersburg/O=ITMO-Organization/OU=FICT/CN=itdt-application.daniildoroshenko.com"
```
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209152112.png)

5. Импортируем сертификат
```bash
kubectl create secret tls tls-sertificat --cert=certificate.crt --key=private.key
```

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209153425.png)
  
6. Пропишем в /etc/hosts доменное имя и IP


![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209153351.png)
7. Проверим работу приложения
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209163644.png)
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab3/screenshot/Pasted%20image%2020241209163705.png)
