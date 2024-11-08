# Домашнее задание к занятию "6.4. Docker. Часть 2" - Петр Фумелли`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.

Желаем успехов в выполнении домашнего задания!

### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Docker Compose – это инструмент, который позволяет управлять многоконтейнерными приложениями с помощью простых команд. Он позволяет запускать, останавливать и настраивать несколько контейнеров одновременно, используя один YAML-файл для описания всей конфигурации. Функицонал:
1. Упрощение управления несколькими контейнерами
2. Автоматизация и повторяемость окружений
3. Лёгкость в настройке сложных DevOps-процессов
4. Экономия времени на разработке и тестировании
5. Простота развёртывания

В любой момент, просто выполнив docker-compose up, получишь готовую рабочую инфраструктуру.
С Docker Compose автоматизируешь и ускоряешь работу с инфраструктурой, что даёт больше времени на работу с кодом, тестирование и оптимизацию, а не на рутину.






---

### Задание 2

```yaml
version: '3.8'

services:

volumes:

networks:
   fumelli-pa-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
---

### Задание 3

```yaml
vversion: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: fumelli-pa-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus-data:/prometheus
    networks:
      - fumelli-pa-my-netology-hw
    restart: always

volumes:
  prometheus-data:

networks:
   fumelli-pa-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```

### Задание 4

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus:v2.47.2
    container_name: fumelli-pa-netology-prometheus
    command: --web.enable-lifecycle --config.file=/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    networks:
      - fumelli-pa-my-netology-hw
    restart: always

  pushgateway:
    image: prom/pushgateway:latest
    container_name: fumelli-pa-netology-pushgateway
    ports:
      - 9091:9091
    networks:
      - fumelli-pa-my-netology-hw
    depends_on:
      - prometheus
    restart: unless-stopped

volumes:
  prometheus-data:

networks:
   fumelli-pa-my-netology-hw:
    driver: bridge
    ipam:
      config:
        - subnet: 10.5.0.0/16
          gateway: 10.5.0.1
```
