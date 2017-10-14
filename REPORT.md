## Laboratory work VI

Данная лабораторная работа посвещена изучению фреймворков для тестирования на примере **Catch**

```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Устанавливаем значение переменных окружения
```ShellSession
$ export GITHUB_USERNAME=h1kk4
```
Клонируем репозиторий
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab05 lab06
$ cd lab06
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06
```
Загружаем необходимые файлы
```ShellSession
$ mkdir tests 
#создаем папку tests
$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp 
# скачиваем "catch.hpp" в директроию /tests
$ cat > tests/main.cpp <<EOF #редактируем файла main.cpp
#define CATCH_CONFIG_MAIN #при компиляции будет преобразована в функцию main
#include "catch.hpp" 
EOF
```
Создаем тесты
```ShellSession
$ sed -i '' '/option(BUILD_EXAMPLES "Build examples" OFF)/a\ #выделение определенной строки
option(BUILD_TESTS "Build tests" OFF)   #вставка следующей строки после выделенной
' CMakeLists.txt # указываем файл, куда будем вставлять 
$ cat >> CMakeLists.txt <<EOF #редаектируем CMakeList

if(BUILD_TESTS)
	enable_testing()
	file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
	add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
	target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
	#добавляем имя теста и указываем команду, необходимую для его запуска 
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes") 
endif()
EOF
```
Написание теста test1.cpp
```ShellSession
$ cat >> tests/test1.cpp <<EOF
#include "catch.hpp"
#include <print.hpp>

TEST_CASE("output values should match input values", "[file]") {
  std::string text = "hello";
  std::ofstream out("file.txt");
  
  print(text, out);
  out.close();
  
  std::string result;
  std::ifstream in("file.txt");
  in >> result;
  
  REQUIRE(result == text);
}
EOF
```
Запуск тестов с помощью cmake
```ShellSession
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
$ cmake --build _build
$ cmake --build _build --target test  
#после сборки запускает test, которая представляет собой запуск исполняемых файлов,одним из которых является исполняемый файл *check*

```
Редкатирование файлов с помощью sed
```ShellSession
$ sed -i '' 's/lab05/lab06/g' README.md   #заменяем lab05 на lab06 в README.md
$ sed -i '' 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml #заменяем строки в .travis.yml
$ sed -i '' '/cmake --build _build --target install/a\ #добавляем следующие строки
- cmake --build _build --target test
' .travis.yml
```

```ShellSession
$ travis lint #проверка .travis.yml
```
Пушим изменения
```ShellSession
$ git add .
$ git commit -m"added tests"
$ git push origin master
```
Авторизация и подключение travis к репозиторию lab06
```ShellSession
$ travis login --auto
$ travis enable
```
Делаем скриншот и сохраняем в artifacts
```ShellSession
$ mkdir artifacts
$ screencapture -T 20 artifacts/screenshot.jpg
<Command>-T
$ open https://github.com/${GITHUB_USERNAME}/lab06
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=06
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Boost.Tests](http://www.boost.org/doc/libs/1_63_0/libs/test/doc/html/)
- [Google Test](https://github.com/google/googletest)

```
Copyright (c) 2017 Братья Вершинины
```
