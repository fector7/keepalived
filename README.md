# Домашнее задание к занятию 1 «Disaster recovery и Keepalived»

### Задание 1
- Дана [схема](1/hsrp_advanced.pkt) для Cisco Packet Tracer, рассматриваемая в лекции.
- На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
- Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).
- Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
- На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

------

![router1](https://github.com/fector7/keepalived/blob/main/img/r1.PNG)
![router2](https://github.com/fector7/keepalived/blob/main/img/r2.PNG)
[схема](https://github.com/fector7/keepalived/blob/main/conf/hsrp_advanced(fedor).pkt)

### Задание 2
- Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного [файла](1/keepalived-simple.conf).
- Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах
- Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.
- Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script
- На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html

------

![master](https://github.com/fector7/keepalived/blob/main/img/keep1.PNG)
![backup](https://github.com/fector7/keepalived/blob/main/img/keep2.PNG)
![log](https://github.com/fector7/keepalived/blob/main/img/keep3.PNG)

```bash
#!/bin/bash

PORT=80
FILE="/var/www/html/index.html"

# Проверка порта
nc -z 127.0.0.1 $PORT
if [ $? -ne 0 ]; then
    exit 1
fi

# Проверка файла
if [ ! -f "$FILE" ]; then
    exit 1
fi

exit 0
```

[master_conf](https://github.com/fector7/keepalived/blob/main/conf/keepalived_master.conf)
[backup_conf](https://github.com/fector7/keepalived/blob/main/conf/keepalived_backup.conf)



 