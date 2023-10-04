ДЗ 26. Развертывание веб приложения

Цели домашнего задания

Получить практические навыки в настройке инфраструктуры с помощью манифестов и конфигураций. Отточить навыки использования ansible/vagrant/docker.

Описание домашнего задания. Стенд: nginx + php-fpm (wordpress) + python (django) + js(node.js) с деплоем через docker-compose.



Перед тем, как запускать предложенный Vagrantfile, нужно создать структуру файлов для деплоя в папке project:

```

├── project
│   ├── docker-compose.yml
│   ├── nginx-conf
│   │   └── nginx.conf
│   ├── node
│   │   └── test.js
│   ├── prov.yml
│   ├── python
│   │   ├── Dockerfile
│   │   ├── manage.py
│   │   ├── mysite
│   │   │   ├── settings.py
│   │   │   ├── urls.py
│   │   │   └── wsgi.py
│   │   └── requirements.txt

```

docker-compose.yml описывает, какие контейнеры нам потребуется создать и их конфигурацию
nginx.conf описывает конфигурацию nginx
test.js представляет сосбой скрипт, задача которого выводить надпись:  Hello from node js server на js сервере
prov.yml - разворачивает через Ansible хост DynamicWeb, устанавливает на него docker и docker-compose, копирует файлы с директории project в директорию /home/vagrant
Dockerfile - файл докера
manage.py - исходный код Django приложения
settings.py, urls.py, wsgi.py - настройки Django приложения
requirements.txt - требования к Django

После этого запускаем создание и деплой ВМ DynamicWeb командой vagrant up:

```
neva@Uneva:~/Otus_Kaneva_dz26/project$ vagrant status
Current machine states:

DynamicWeb                running (virtualbox)
```

Далее отрабатывает Ансибл через параметры, прописанные в prov.yml и запускает контейнеры с параметрам из docker-compose: 


```
neva@Uneva:~/Otus_Kaneva_dz26/project$ vagrant provision
==> DynamicWeb: Running provisioner: ansible...
Vagrant gathered an unknown Ansible version:


and falls back on the compatibility mode '1.8'.

Alternatively, the compatibility mode can be specified in your Vagrantfile:
https://www.vagrantup.com/docs/provisioning/ansible_common.html#compatibility_mode

    DynamicWeb: Running ansible-playbook...

PLAY [Base set up] *************************************************************

TASK [Install docker packages] *************************************************
ok: [DynamicWeb] => (item=apt-transport-https)
ok: [DynamicWeb] => (item=ca-certificates)
ok: [DynamicWeb] => (item=curl)
ok: [DynamicWeb] => (item=software-properties-common)

TASK [Add Docker s official GPG key] *******************************************
ok: [DynamicWeb]

TASK [Verify that we have the key with the fingerprint] ************************
ok: [DynamicWeb]

TASK [Set up the stable repository] ********************************************
ok: [DynamicWeb]

TASK [Update apt packages] *****************************************************
ok: [DynamicWeb]

TASK [Install docker] **********************************************************
ok: [DynamicWeb]

TASK [Add remote "vagrant" user to "docker" group] *****************************
[WARNING]: 'append' is set, but no 'groups' are specified. Use 'groups' for
appending new groups.This will change to an error in Ansible 2.14.
ok: [DynamicWeb]

TASK [Install docker-compose] **************************************************
ok: [DynamicWeb]

TASK [Copy project] ************************************************************
changed: [DynamicWeb]

TASK [reset ssh connection] ****************************************************

TASK [Run container] ***********************************************************
changed: [DynamicWeb]

PLAY RECAP *********************************************************************
DynamicWeb                 : ok=10   changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

Для проверки результата заходим через браузер на адреса:

```
http:/localhost:8081
http:/localhost:8082
http:/localhost:8083
```

Результаы представлены на скриншотах.


