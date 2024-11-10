# Домашнее задание к занятию "6.4. Docker. Часть 2" - Петр Фумелли

### Задание 1

Docker Compose – это инструмент, который позволяет управлять многоконтейнерными приложениями с помощью простых команд. Он позволяет запускать, останавливать и настраивать несколько контейнеров одновременно, используя один YAML-файл для описания всей конфигурации. Функицонал:

1. Упрощение управления несколькими контейнерами
2. Автоматизация и повторяемость окружений
3. Лёгкость в настройке сложных DevOps-процессов
4. Экономия времени на разработке и тестировании
5. Простота развёртывания

В любой момент, просто выполнив docker-compose up, получишь готовую рабочую инфраструктуру.
С Docker Compose автоматизируешь и ускоряешь работу с инфраструктурой, что даёт больше времени на работу с кодом, тестирование и оптимизацию, а не на рутину.

### Задание 2

```yaml
version: '3'

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
    image: prom/pushgateway:v1.6.2
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

### Задание 7

<https://github.com/PeterFumelli/docker-part-two/blob/main/img/grafana.png>

<https://github.com/PeterFumelli/docker-part-two/blob/main/img/docker_compose_ps.png>

### Задание 8

<https://github.com/PeterFumelli/docker-part-two/blob/main/img/%20docker_rm.png>

### Задание 9

<https://github.com/PeterFumelli/docker-part-two/blob/main/img/alertmanager.png>

