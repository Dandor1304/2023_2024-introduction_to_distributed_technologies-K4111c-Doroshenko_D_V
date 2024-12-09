University: ITMO University  
Faculty: FICT  
Course: Introduction to distributed technologies  
Year: 2024/2025  
Group: K4111c  
Author: Doroshenko Daniil Vadimovich  
Lab: Lab1  
Date of create: 9.11.2024  
Date of finished: 12.11.2024  


Ход работы:
1. [Установим docker на рабочий компьютер ](https://docs.docker.com/engine/install/ubuntu/)


   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241108212902.png)

2. [Установим minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download#Service)
   
   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241108214355.png)

   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241108221310.png)

3. [Напишем манифест для пода и запустим](https://github.com/Dandor1304/k8s-labs/blob/develop/lab1/lab1/vault-pod.yaml)
   
   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241109012828.png)
 
4. [Напишем манифест для сервиса и запустим](https://github.com/Dandor1304/k8s-labs/blob/mainlab1/nodeport-service.yaml) 
   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241109010157.png)
Nodeport - сервис, который позволяет открыть порт на всех нодах для доступа к подам извне. При создании сервиса задается 3 вида портов. nodePort - какой порт откроет k8s на ноде для доступа до подов. Как я понял хорошей практикой является не задавать его вручную, k8s сам его задаст (данный порт задается в промежутке 30000-32767). Targetport - порт на котором работает приложение. Port - порт который используется сервисом.
После создания сервиса, чтобы попасть на приложение нужно ввести ip ноды и порт, который выбрал k8s (nodeport).
Лабораторная выполнялась на виртуальной машине(без gui), чтобы получить страницу на windows можно пробросить порты следующим образом (ssh -L 50000:192.168.49.2:30641 login@192.168.1.221). Создадим ssh туннель через виртуальную машину до ноды кластера.
   ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241109012725.png)
5. Для входа посмотрим логи пода с помощью команды `kubectl logs vault` 
    ![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241109011902.png)
![](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Pasted%20image%2020241109012424.png)
![Архитектура](https://github.com/Dandor1304/k8s-labs/blob/main/lab1/Диаграмма%20без%20названия.drawio%20(15).png)
