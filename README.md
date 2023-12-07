#  Дипломная работа по профессии «Системный администратор»

Содержание
==========
* [Задача](#задача)
* [Инфраструктура](#инфраструктура)
    * [Сайт](#сайт)
    * [Логи](#логи)
    * [Мониторинг](#мониторинг)
    * [Сеть](#сеть)
    * [Резервное копирование](#резервное-копирование)

---------
## Задача
Ключевая задача — разработать отказоустойчивую инфраструктуру для сайта, включающую мониторинг, сбор логов и резервное копирование основных данных. Инфраструктура должна размещаться в [Yandex Cloud](https://cloud.yandex.com/).


## Инфраструктура
Для развёртки инфраструктуры используем [Terraform](./terraform), а для установки ПО [Ansible](./ansible).

Terraform:

Проверяем правильность синтаксиса файлов Terraform

![Скриншот-1](./img/sys-diplom1.PNG) 

Просматриваем план создание инфраструктуры

![Скриншот-2](./img/sys-diplom2.PNG)

И запускаем terraform apply

![Скриншот-3](./img/sys-diplom3.PNG)

Дожидаемся окончания создания инфраструктуры и вывода информации output

![Скриншот-4](./img/sys-diplom4.PNG)

Заранее подготовленный [output-ansible-hosts](./terraform/output.tf) сохраняем в отдельный файл hosts для ansible и удаляем лишнее из файла

![Скриншот-5](./img/sys-diplom5.PNG)

Проверяем доступность созданных ВМ с помощью ansible all -m ping

![Скриншот-6](./img/sys-diplom6.PNG)

Переходим к установке и настройке ПО с помощью Ansible

### Сайт
Запускаем playbook - [webservers-playbook.yml](./ansible/webservers-playbook.yml)

По окончанию выполнения playbook проверяем что сервисы nginx, node_exporter и nginx_logexporter работаю

![Скриншот-7](./img/sys-diplom7.PNG)

![Скриншот-8](./img/sys-diplom8.PNG)

Проверяем что наш сайт работает

![Скриншот-9](./img/sys-diplom9.PNG)

![Скриншот-10](./img/sys-diplom10.PNG)

### Логи
Запускаем playbook - [log-playbook.yml](./ansible/log-playbook.yml)

По завершению выполнения playbook проверяем что контейнеры с elasticsearch и kibana работают

![Скриншот-25](./img/sys-diplom25.PNG)

![Скриншот-26](./img/sys-diplom26.PNG)

Уставноку filebeat - [log-filebeat-playbook.yml](./ansible/log-filebeat-playbook.yml)

![Скриншот-24](./img/sys-diplom24.PNG)

Заходим в Kibana для проверки что логи nginx с web серверов поступают

![Скриншот-17](./img/sys-diplom17.PNG)

![Скриншот-18](./img/sys-diplom18.PNG)

Из лога ошибок Nginx видим что кто то пытается зайти на страницу которой нет)))

### Мониторинг
Запускаем playbook - [monitoring-playbook.yml](./ansible/monitoring-playbook.yml)

Проверяем что работа playbook завершилась без ошибок

![Скриншот-11](./img/sys-diplom11.PNG)

<details>
При первом запуске monitoring-playbook.yml выдаёт ошибку импорта дашборда в графану

![Скриншот-12](./img/sys-diplom12.PNG)

При повторном запуске playbook он выполняется полностью без ошибок.
</details>

Заходим в Grafana для проверки работоспособности наших ранее импортированных дашбордов 

NGINX Servers Metrics:

![Скриншот-13](./img/sys-diplom13.PNG)

![Скриншот-14](./img/sys-diplom14.PNG)

Node Exporter Full:

![Скриншот-27](./img/sys-diplom27.PNG)

### Сеть

![Скриншот-20](./img/sys-diplom20.PNG)

![Скриншот-21](./img/sys-diplom21.PNG)

![Скриншот-22](./img/sys-diplom22.PNG)

### Резервное копирование

![Скриншот-19](./img/sys-diplom19.PNG)

