[![Build Status](https://travis-ci.org/h1kk4/lab07.svg?branch=master)](https://travis-ci.org/h1kk4/lab07)
## Laboratory work VII

Данная лабораторная работа посвещена изучению систем документирования исходного кода на примере **Doxygen**

```ShellSession
$ open https://www.stack.nl/~dimitri/doxygen/manual/index.html
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Задаем переменные окружения
```ShellSession
$ export GITHUB_USERNAME=h1kk4
$ alias edit=nano
$ alias gsed=sed # for *-nix system
```
Копируем репозиторий из прошлой лабы
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 lab07
$ cd lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```
Создаем документацию
```ShellSession
$ mkdir docs
$ doxygen -g docs/doxygen.conf
$ cat docs/doxygen.conf
```
Редактируем conf-файл с помощью sed
```ShellSession
$ gsed -i 's/\(PROJECT_NAME.*=\).*$/\1 print/g' docs/doxygen.conf
$ gsed -i 's/\(EXAMPLE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ gsed -i 's/\(INCLUDE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ gsed -i 's/\(INPUT *=\).*$/\1 README.md include/g' docs/doxygen.conf
$ gsed -i 's/\(USE_MDFILE_AS_MAINPAGE.*=\).*$/\1 README.md/g' docs/doxygen.conf
$ gsed -i 's/\(OUTPUT_DIRECTORY.*=\).*$/\1 docs/g' docs/doxygen.conf
```

```ShellSession
$ gsed -i 's/lab06/lab07/g' README.md
```

```ShellSession
# документируем функции print 
$ edit include/print.hpp
```
Коммитим и пушим изменения в репозиторий
```ShellSession
$ git add .
$ git commit -m"added doxygen.conf"
$ git push origin master
```
Активируем этот проект в Travis
```ShellSession
$ travis login --auto
$ travis enable
```
Работаем с файлами в Doxygen
```ShellSession
$ doxygen docs/doxygen.conf #Удаляем все файлы которые не содержат "docs"
$ ls | grep "[^docs]" | xargs rm -rf #Перемещаем директории docs/html в текущую и удаляем директории docs 
$ mv docs/html/* . && rm -rf docs #Создаем новую ветку и пушим в неё
$ git checkout -b gh-pages
$ git add .
$ git commit -m"added documentation"
$ git push origin gh-pages
$ git checkout master
```

```ShellSession
$ mkdir artifacts && cd artifacts  #создание директории и переход в нее
$ screencapture -T 10 screenshot.jpg # или png #делаем скриншот
<Command>-T
$ open https://${GITHUB_USERNAME}.github.io/lab07/print_8hpp.html
$ gdrive upload screenshot.jpg # или png #загрузка скриншота в Google Drive
$ SCREENSHOT_ID=`gdrive list | grep screenshot | awk '{ print $1; }'`
$ gdrive share ${SCREENSHOT_ID} --role reader --type user --email rusdevops@gmail.com   #Разрешить доступ
$ echo https://drive.google.com/open?id=${SCREENSHOT_ID} #Вывод ссылки на скриншот
    https://drive.google.com/open?id=0B1ftqtWqkDq2R2lxbjc1dWprR2M
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=07
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [HTML](https://ru.wikipedia.org/wiki/HTML)
- [LAΤΕΧ](https://ru.wikipedia.org/wiki/LaTeX)
- [man](https://ru.wikipedia.org/wiki/Man_(%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%B0_Unix))
- [CHM](https://ru.wikipedia.org/wiki/HTMLHelp)
- [PostScript](https://ru.wikipedia.org/wiki/PostScript)

```
Copyright (c) 2017 Братья Вершинины
```
