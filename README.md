## Laboratory work IX

Данная лабораторная работа посвещена изучению процесса создания пакета на примере **Github Release**

```ShellSession
$ open https://help.github.com/articles/creating-releases/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab09** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Получить токен для доступа к репозиториям сервиса **GitHub**
- [X] 4. Сгенерировать GPG ключ и добавить его к аккаунту сервиса **GitHub**
- [X] 5. Выполнить инструкцию учебного материала
- [X] 6. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Задаем переменные окружения
```ShellSession
$ export GITHUB_TOKEN=****************************************
$ export GITHUB_USERNAME=h1kk4
$ alias gsed=sed # for *-nix system
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ go get github.com/aktau/github-release #Установка github-release
```
Копирование репозитория 8 лабораторной
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab08 projects/lab09
$ cd projects/lab09
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab09
```
Замена lab08 на lab09
```ShellSession
$ gsed -i 's/lab08/lab09/g' README.md
```
Сборка проекта
```ShellSession
$ cmake -H. -B_build -DCPACK_GENERATOR="TGZ"
$ cmake --build _build --target package
```
Активируем этот проект в Travis
```ShellSession
$ travis login --auto
$ travis enable
```
Добавляем метки
```ShellSession
$ git tag -s v0.1.0.0
$ git tag -v v0.1.0.0
$ git push origin master --tags
```
Добавление релиза
```ShellSession
$ github-release --version #версия github-release 
github-release v0.7.2
$ github-release info -u ${GITHUB_USERNAME} -r lab09  #информация о релизах
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/h1kk4/lab09/commits/d81d3adb11c7610bbb45d540d6e2994d289819fd)
releases:
- v0.1.0.0, name: '', description: '', id: 8302744, tagged: 29/10/2017 at 16:48, published: 29/10/2017 at 18:21, draft: ✗, prerelease: ✗
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "libprint" \
    --description "my first release"
```
Загрузка релиза
```ShellSession
$ export PACKAGE_OS=`uname -s` PACKAGE_ARCH=`uname -m` #записываем в переменные окружения данные об ОС
$ export PACKAGE_FILENAME=print-${PACKAGE_OS}-${PACKAGE_ARCH}.tar.gz  #записываем в переменные окружения имя пакета
$ github-release upload \  #загрузка релиза
    --user ${GITHUB_USERNAME} \
    --repo lab09 \
    --tag v0.1.0.0 \
    --name "${PACKAGE_FILENAME}" \
    --file _build/*.tar.gz
```
Загрузка архива
```ShellSession
$ github-release info -u ${GITHUB_USERNAME} -r lab09
tags:
- v0.1.0.0 (commit: https://api.github.com/repos/h1kk4/lab09/commits/d81d3adb11c7610bbb45d540d6e2994d289819fd)
releases:
- v0.1.0.0, name: 'libprint', description: 'my first release', id: 8302909, tagged: 29/10/2017 at 16:48, published: 29/10/2017 at 19:00, draft: ✗, prerelease: ✗
  - artifact: print-Darwin-x86_64.tar.gz, downloads: 0, state: uploaded, type: application/octet-stream, size: 4.7 kB, id: 5195048
$ wget https://github.com/${GITHUB_USERNAME}/lab09/releases/download/v0.1.0.0/${PACKAGE_FILENAME}
$ tar -ztf ${PACKAGE_FILENAME} #распаковываем файл
print-0.1.0.0-Darwin/cmake/
print-0.1.0.0-Darwin/cmake/print-config-noconfig.cmake
print-0.1.0.0-Darwin/cmake/print-config.cmake
print-0.1.0.0-Darwin/include/
print-0.1.0.0-Darwin/include/print.hpp
print-0.1.0.0-Darwin/lib/
print-0.1.0.0-Darwin/lib/libprint.a
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=09
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Create Release](https://help.github.com/articles/creating-releases/)
- [Get GitHub Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
- [Signing Commits](https://help.github.com/articles/signing-commits-with-gpg/)
- [Go Setup](http://www.golangbootcamp.com/book/get_setup)
- [github-release](https://github.com/aktau/github-release)

```
Copyright (c) 2017 Братья Вершинины
```
