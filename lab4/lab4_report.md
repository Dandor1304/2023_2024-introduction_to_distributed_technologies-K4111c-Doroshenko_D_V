University: ITMO University
Faculty: FICT
Course: Introduction to distributed technologies
Year: 2024/2025
Group: K4111c
Author: Doroshenko Daniil Vadimovich
Lab: Lab4
Date of create: 11.12.2024
Date of finished: 12.11.2024

**CNI (Container Network Interface)** — это стандартный интерфейс, разработанный для упрощения сетевого взаимодействия контейнеров в Kubernetes (и других системах). CNI позволяет подключать сетевые плагины к контейнерным платформам, чтобы обеспечить контейнерам сетевые возможности, такие как подключение к сетям, управление IP-адресами и маршрутизацией.

**IPAM Plugin - IP Address Management Plugin** — это часть архитектуры CNI, которая отвечает за управление IP-адресами для сетевых интерфейсов контейнеров. Функция IPAM позволяет автоматизировать выделение, резервирование и управление IP-адресами для Podов в Kubernetes.

- **Установить CNI=calico и Multi-Node Cluster**
```shell
minikube start --network-plugin=cni --cni=calico --nodes 2 -p multinode-demo
```

- **Проверим конфигурацию кластера**

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241211220822.png)

- **Указать label для ноды**
```shell
kubectl label nodes multinode-demo rack=1
kubectl label nodes multinode-demo-m02 rack=2
kubectl get no --show-labels
```
![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241211223149.png)

- **Создадим пулы ip адресов  и установим утилиту calicoctl**
```shell
curl -L https://github.com/projectcalico/calico/releases/download/v3.29.1/calicoctl-linux-amd64 -o calicoctl && chmod +x calicoctl && mv /usr/bin/calicoctl

calicoctl create -f ippool1
calicoctl create -f ippool2
```

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241211230942.png)
- **Создать сервис NodePort**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodeport-service
spec:
  type: NodePort
  selector:
    app: itdt-frontend
  ports:
    - port: 3000
      targetPort: 3000
```
- **Создадим и запустим Deployment**
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: itdt
  labels:
    app: itdt-frontend
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
      - name: itdt-frontend
        image: ifilyaninitmo/itdt-contained-frontend:master
        ports:
        - containerPort: 3000
        env:
        - name: REACT_APP_USERNAME
          value: "Daniil Doroshenko"
        - name: REACT_APP_COMPANY_NAME
          value: ITMO

```
- **Проверить работоспособность приложения**

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241212001507.png)

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241212001533.png)

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241212001813.png)
- **Пинг соседнего пода**  

![](https://github.com/Dandor1304/2024_2025-introduction_to_distributed_technologies-K4111c-Doroshenko_D_V/blob/main/lab4/images/Pasted%20image%2020241212003258.png)
- **Создадим схему**
