## Laboratory work III

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** и с лиценцией **MIT**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Устанавливаем значение переменных окружения GITHUB_USERNAME и GITHUB_EMAIL
```ShellSession
$ export GITHUB_USERNAME=h1kk4
$ export GITHUB_EMAIL=ipodmacair@gmail.com
$ alias edit=nano
```
Создаем рабочее пространство
```ShellSession
$ mkdir lab03 && cd lab03  #создание директории и переход в неё
$ git init	#инициализация локального репозитория
$ git config --global user.name ${GITHUB_USERNAME}	#первоначальная настройка данных пользователя
$ git config --global user.email ${GITHUB_EMAIL}
$ git config -e --global
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03 #добавление удаленного репозитория
$ git pull origin master	 #получение данных
$ touch README.md	 #создание файла README
$ git status 	#получение состояния файлов 
$ git add README.md	#добавление файла 
$ git commit -m"added README.md"	#делаем коммит
$ git push origin master 	#делаем пуш в репозиторий
```

Добавить на сервисе **GitHub** в репозитории **lab03** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
```
Получение изменений
```ShellSession
$ git pull origin master	 #получение данных из репозитория
$ git log			#получение истории коммитов
```
Создаем директории для проекта
```ShellSession
$ mkdir source	#создание директории для файлов исходного кода
$ mkdir include	#создание директории для заголовочных файлов
$ mkdir examples#создание директории для примеров
```

Заполненем файлы 
```ShellSession
$ cat > sources/print.cpp <<EOF		
#include <print.hpp>

void print(const std::string& text, std::ostream& out) {
  out << text;
}

void print(const std::string& text, std::ofstream& out) {
  out << text;
}
EOF                  
```
Заполняем заголовочны файл
```ShellSession
$ cat > include/print.hpp <<EOF
#include <string>
#include <fstream>
#include <iostream>

void print(const std::string& text, std::ostream& out = std::cout);
void print(const std::string& text, std::ofstream& out);
EOF              
```
Пишем пример кода 1
```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv) {
  print("hello");
}
EOF               
```
Пишем пример кода 2
```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <fstream>
#include <print.hpp>

int main(int argc, char** argv) {
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF              
```
Редактируем README
```ShellSession
$ edit README.md		
```
Пушим изменения
```ShellSession
$ git status	#Получение состояния файлов	
$ git add .	#Добавление файла
$ git commit -m"added sources" #коммит
$ git push origin master	#пуш данных в репозиторий
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2017 Братья Вершинины
```
