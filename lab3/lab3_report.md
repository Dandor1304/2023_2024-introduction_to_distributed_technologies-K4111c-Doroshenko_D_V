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
![[Pasted image 20241209151940.png]]
4. Сгенерируем сертификат
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt -subj "/C=RU/ST=SPB/L=Saint-Petersburg/O=ITMO-Organization/OU=FICT/CN=itdt-application.daniildoroshenko.com"

![[Pasted image 20241209152112.png]]

5. Импортируем сертификат kubectl create secret tls tls-sertificat --cert=certificate.crt --key=private.key
![[Pasted image 20241209153425.png]]

7. Пропишем в /etc/hosts доменное имя и IP
![[Pasted image 20241209153351.png]]
1. Проверим работу приложения
![[Pasted image 20241209163644.png]]
![[Pasted image 20241209163705.png]]
