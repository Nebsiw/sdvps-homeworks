
### Задание 1

Установите Docker Compose и опишите, для чего он нужен и как может улучшить вашу жизнь.

---
##  Ответ задание 1


---

### Задание 7 

**Выполните действия.**
1. Выполните запрос в Pushgateway для помещения метрики <ваши фамилия и инициалы> со значением 5 в Prometheus: ```echo "<ваши фамилия и инициалы> 5" | curl --data-binary @- http://localhost:<внешний порт выбранный вами в задании 4>/metrics/job/netology```.
2. Залогиньтесь в Grafana с помощью логина и пароля из предыдущего задания.
3. Cоздайте Data Source Prometheus (Home -> Connections -> Data sources -> Add data source -> Prometheus -> указать "Prometheus server URL = http://prometheus:9090" -> Save & Test).
4. Создайте график на основе добавленной в пункте 5 метрики (Build a dashboard -> Add visualization -> Prometheus -> Select metric -> Metric explorer -> <ваши фамилия и инициалы -> Apply.

В качестве решения приложите:

* docker-compose.yml **целиком**;
* скриншот команды docker ps после запуске docker-compose.yml;
* скриншот графика, постоенного на основе вашей метрики.

---
##  Ответ задание 7
**Выполнение**
1. [docler-compose.yml](https://github.com/Nebsiw/sdvps-homeworks/blob/main/docker-6.4-new/compose.yml)
2. Отображение выводы команды docker ps ![docker ps](https://github.com/Nebsiw/sdvps-homeworks/blob/main/docker-6.4-new/images/Screenshot%20from%202024-06-27%2019-46-46.png)
3. Отображение графика в Grafana, для создания метрики использовался не большой скрипт ![grafana](https://github.com/Nebsiw/sdvps-homeworks/blob/main/docker-6.4-new/images/Screenshot%20from%202024-06-27%2020-16-35.png)
