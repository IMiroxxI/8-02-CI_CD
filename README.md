# Домашнее задание к занятию "`Что такое DevOps. СI/СD`" - `Khalatov Alexandr`

### Задание 1

1. Установите себе jenkins по инструкции из лекции или любым другим способом из официальной документации. Использовать Docker в этом задании нежелательно.
![1-1](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-1.png)
2. Установите на машину с jenkins [golang](https://go.dev/doc/install). 
![1-2](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-2.png)
3. Используя свой аккаунт на GitHub, сделайте себе форк репозитория. В этом же репозитории находится дополнительный материал для выполнения ДЗ.
![1-3](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-3.png)
4. Создайте в jenkins Freestyle Project, подключите получившийся репозиторий к нему и произведите запуск тестов и сборку проекта `go test .` и `docker build .`.
![1-4](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-4.png)
![1-4.1](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-4.1.png)
![1-4.2](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/1-4.2.png)

---

### Задание 2

1. Создайте новый проект pipeline.
![2-1](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/2-1.png)
2. Перепишите сборку из задания 1 на declarative в виде кода.
При описании PipeLine у меня возникала ошибка по ветке.
Пришлось в конфигурацию дополнительно дописывать:
```Groovy
stage('Git Checkout') {
            steps {
                git branch: 'main', // Используйте 'main' вместо 'master'
                    url: 'https://github.com/IMiroxxI/sdvps-materials.git'
            }
        }
```

![2-2](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/2-2.png)
![2-3](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/2-3.png)


---

### Задание 3

1. Установите на машину Nexus.

*Скачайте последнюю версию*
```
sudo mkdir -p /opt/nexus
cd /opt/nexus
sudo wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.78.2-04.tar.gz
sudo tar xzf nexus-unix-x86-64-3.78.2-04.tar.gz
sudo rm nexus-unix-x86-64-3.78.2-04.tar.gz
```

*Настройте пользователя*
```
sudo useradd nexus -d /opt/nexus
sudo chown -R nexus:nexus /opt/nexus
```

*Создайте systemd unit*
```
sudo tee /etc/systemd/system/nexus.service <<EOF
[Unit]
Description=Nexus Service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
ExecStart=/opt/nexus/nexus-3.78.2-04/bin/nexus start
ExecStop=/opt/nexus/nexus-3.78.2-04/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target
EOF
```

*Запустите*
```
sudo systemctl daemon-reload
sudo systemctl enable nexus
sudo systemctl start nexus
```
![3-1](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-1.png)
2. Создайте raw-hosted репозиторий.
![3-2](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-2.png)
3. Измените pipeline так, чтобы вместо Docker-образа собирался бинарный go-файл. Команду можно скопировать из Dockerfile.
![3-3](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-3.png)
4. Загрузите файл в репозиторий с помощью jenkins.
![3-4](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-4.png)
![3-4.1](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-4.1.png)
![3-4.2](https://github.com/IMiroxxI/8-02-CI_CD/blob/main/img/3-4.2.png)
