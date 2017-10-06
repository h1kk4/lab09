[![Build Status](https://travis-ci.org/h1kk4/lab05.svg?branch=master)](https://travis-ci.org/h1kk4/lab05)
## Laboratory work V

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [X] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [X] 2. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [X] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [X] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [X] 7. Установить [**Travis CLI**](https://github.com/travis-ci/travis.rb#installation)
- [X] 8. Выполнить инструкцию учебного материала
- [X] 9. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Устанавливаем значение переменных окружения
```ShellSession
$ export GITHUB_USERNAME=h1kk4
$ export GITHUB_TOKEN=xxxxxxxxxxxxxxxxxxxxx
```
Клонируем репозиторий
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 lab05
$ cd lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
```
Записываем язык программы
```ShellSession
$ cat > .travis.yml <<EOF
language: cpp
EOF
```
Записываем скрипт
```ShellSession
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```
Записываем аддоны
```ShellSession
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
Записываем наш токек
```ShellSession
$ travis login --github-token ${GITHUB_TOKEN}
```
Проверяем работоспособность
```ShellSession
$ travis lint
```
Вставка значка в файл отчета
```ShellSession
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
```
Делаем коммит
```ShellSession
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```
Используем комманды тревиса
```ShellSession
$ travis lint 
Warnings for .travis.yml:
[x] value for addons section is empty, dropping
[x] in addons section: unexpected key apt, dropping

$ travis accounts #выводим все привязанные GitHub аккаунты
h1kk4 (H1kk4): subscribed, 7 repositories

$ travis sync #синхронизация
synchronizing: . done

$ travis repos #выводим репозитории и их статусы(связаны ли они с Travis)
h1kk4/Metods (active: no, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/TelegramBot (active: no, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/bmstu_labs (active: no, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/lab03 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/lab04 (active: no, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/lab05 (active: yes, admin: yes, push: yes, pull: yes)
Description: ???

h1kk4/piu-piu-SH (active: no, admin: yes, push: yes, pull: yes)
Description: This is an Old School horisontal scroller 'Shoot Them All' game on bash. With co-op mode. You have to defeat 100 aliens to fight with Boss. To play in co-op mode first, start the server, then start the client. Terminals on both hosts have to be with equal dimensions.

$ travis enable #включаем travis в текущей директории
h1kk4/lab05: enabled :)

$ travis whatsup #выодоим список выполненых команд
h1kk4/lab05 passed: #5

$ travis branches #выводим список сделанных шагов на ветке master 
master:  #5    passed     Update README.md

$ travis history #выводим всю историю изменений и их состояние 
#5 passed:       master Update README.md
#4 passed:       master Update README.md
#3 passed:       master Update README.md
#2 passed:       master Update README.md
#1 passed:       master added CI

$ travis show  #выводим всю информацию
Job #5.1:  Update README.md
State:         passed
Type:          push
Branch:        master
Compare URL:   https://github.com/h1kk4/lab05/compare/2632fa03838c...f55a75c4f027
Duration:      30 sec
Started:       2017-10-06 20:58:28
Finished:      2017-10-06 20:58:58
Allow Failure: false
Config:        os: linux
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Travis Client](https://github.com/travis-ci/travis.rb)
- [AppVeyour](https://www.appveyor.com/)
- [GitLab CI](https://about.gitlab.com/gitlab-ci/)

```
Copyright (c) 2017 Братья Вершинины
```
