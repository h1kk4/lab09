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
$ travis accounts #выводим все привязанные GitHub аккаунты
$ travis sync #синхронизация
$ travis repos #выводим репозитории и их статусы(связаны ли они с Travis)
$ travis enable #включаем travis в текущей директории
$ travis whatsup #выодоим список выполненых команд
$ travis branches #выводим список сделанных шагов на ветке master 
$ travis history #выводим всю историю изменений и их состояние 
$ travis show  #выводим всю информацию
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
