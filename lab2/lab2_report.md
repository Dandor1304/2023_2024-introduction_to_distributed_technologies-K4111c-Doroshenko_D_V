University: ITMO University  
Faculty: FICT  
Course: Introduction to distributed technologies  
Year: 2024/2025  
Group: K4111c  
Author: Doroshenko Daniil Vadimovich  
Lab: Lab2 
Date of create: 9.11.2024  
Date of finished: 12.11.2024  

Ход работы:

1 Создаем [деплоймент](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/itdt-deployments.yaml) с [приложением](https://hub.docker.com/r/ifilyaninitmo/itdt-contained-frontend) в 2 реплииках и заданными переменными.

2 [Создаем сервис nodeport](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/itdt-nodeport.yaml) и пробрасываем порты (как в 1 лабе)
ssh -L 45000:192.168.49.2:31596 dandor@192.168.1.221
![](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/Pasted%20image%2020241111215301.png)

3 Проверяем страницу
![](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/Pasted%20image%2020241111215410.png)

![](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/Pasted%20image%2020241111215442.png)
При обновлении страницы данные пода меняются. Это связано с тем, что сервис распределяет нагрузку между 2 подами и при обновлении мы попадаем на разные поды.

4 Проверяем логи контейнера

![](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/Pasted%20image%2020241111215752.png)

5 Создадим схему организации контейнеров 
![](https://github.com/Dandor1304/k8s-labs/blob/develop/lab2/lab2/Диаграмма%20без%20названия.drawio%20(15).drawio.png)
