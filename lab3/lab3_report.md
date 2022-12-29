University: [ITMO University](https://itmo.ru/ru/)    
Faculty: [FICT](https://fict.itmo.ru)    
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)    
Year: 2022/2023    
Group: K4112c   
Author: Lisitsa Nikita Vyacheslavovich  
Lab: Lab3   
Date of create:   
Date of finished: 
___
## Запуск

Создаем и деплоим все необходимые компоненты:
```bash
kubectl apply -f lab3-configmap.yaml
kubectl apply -f lab3-replicaset.yaml
kubectl apply -f lab3-service.yaml
```
Создаем сертификат TLS и secrets:
```bash
openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout tls.key -out tls.crt -subj "/CN=nikspel.com" -days 20
kubectl create secret tls lab3-tls --cert=tls.crt --key=tls.key
```
Добавляем созданный ingress:
```bash
kubectl apply -f lab3-ingress.yaml
```
Проверяем созданные поды:
![pods](./result/pods.png)

Добавляем ip-адрес приложения и имя хоста в дирикторию C:\Windows\System32\drivers\etc
![result](./result/hosts.png)

Конфигурирем minikube для работы с ingress:
```bash
minikube addons enable ingress
minikube addons enable ingress-dns
```

После чего создаем тунель следующей командой:
```bash
minikube tunnel
```

Перейдя по адресу nikspel.com можно увидеть окно с нашим приложением:
![result](./result/result.png)

## Схема организации контейеров и сервисов нарисованная вами в draw.io или Visio.
![shema](./result/shema.png)
___
## Вывод
В рамках данной лабораторной работы, мной был создан configMap содержащий две переменные, replicaSet с 2 репликами контейнера, service через который осуществляется доступ к подам. После чего, мной по средствам утилиты openssl был сгенерирован TLS сертификат и добавлен в ingress, для возможности доступа к приложению по протоколу https.