## Laboratory work VI

Данная лабораторная работа посвящена изучению фреймворков для тестирования на примере **Catch**

[![Build Status](https://travis-ci.org/Goganych/lab06.svg?branch=master)](https://travis-ci.org/Goganych/lab06)

Открываем сайт https://github.com/philsquared/Catch
```ShellSession
$ open https://github.com/philsquared/Catch
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab06** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Делаем первоначальные настройки, добавляя значения переменным окружения
```ShellSession
$ export GITHUB_USERNAME=Goganych #Устанавливаем значение переменной окружения GITHUB_USERNAME
```
Проводим первоначальные настройки для соединения с репозиторием шестой лабораторной работы
```ShellSession

$ git clone https://github.com/${GITHUB_USERNAME}/lab05 lab06 #Клонирование удаленного репозитория lab05 в локальный каталог 
$ cd lab06 #Меняем директорию на lab06
$ git remote remove origin #Отключаемся от удаленного репозитория пятой лабораторной
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab06 #Подключаемся к удаленному репозиторию шестой лабораторной
```

```ShellSession
$ mkdir tests #Создаем каталог tests
#Устанавливаем библиотеку для модульного тестирования на языке С++ catch.hpp в каталог tests
$ wget https://github.com/philsquared/Catch/releases/download/v1.9.3/catch.hpp -O tests/catch.hpp
$ cat > tests/main.cpp <<EOF #Вносим изменения в main.cpp, подключая к нему catch.hpp
#define CATCH_CONFIG_MAIN
#include "catch.hpp"
EOF
```
Вносим изменения в CMakeLists.txt
```ShellSession
#Добавляем опцию option(BUILD_TESTS "Build tests" OFF) в файл CMakeLists.txt
$ sed -i '' '/option(BUILD_EXAMPLES "Build examples" OFF)/a\
option(BUILD_TESTS "Build tests" OFF)
' CMakeLists.txt
$ cat >> CMakeLists.txt <<EOF #Добавляем настройки в CMakeLists.txt

if(BUILD_TESTS)
	enable_testing()
	file(GLOB \${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
	add_executable(check \${\${PROJECT_NAME}_TEST_SOURCES})
	target_link_libraries(check \${PROJECT_NAME} \${DEPENDS_LIBRARIES})
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes")
endif()
EOF
```
Вносим изменения в test1.cpp
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
Работа с CMake
```ShellSession
#-H. устанавливаем каталог в который сгенерируется файл CMakeLists.txt
#-B_build указывает директорию для собираемых файлов
#-D - заменяет команду set
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install -DBUILD_TESTS=ON
#--build _build создает бинарное дерево проекта
$ cmake --build _build
#--build _build создает бинарное дерево проекта
#--target указывает необходимые для обработки цели
$ cmake --build _build --target test Полная сборка проекта test
```
Работа с Travis
```ShellSession

$ sed -i '' 's/lab05/lab06/g' README.md #Вносим изменения в файле README.md
$ sed -i '' 's/\(DCMAKE_INSTALL_PREFIX=_install\)/\1 -DBUILD_TESTS=ON/' .travis.yml #Вносим изменения в файле .travis.yml
$ sed -i '' '/cmake --build _build --target install/a\ #Вносим изменения в файле .travis.yml
- cmake --build _build --target test
' .travis.yml
```
Отображаем предупреждения или ошибки в файле .travis.yml
```ShellSession
$ travis lint #Отображаем предупреждения или ошибки в файле .travis.yml
#Warnings for .travis.yml:
#[x] value for addons section is empty, dropping
#[x] in addons section: unexpected key apt, dropping
```
Выполняем команды для настройки локального репозитория для дальйшей отправки
в удаленный репозиторий шестой лабораторной работы
```ShellSession

$ git add . #Добавляем все отредактированные файлы в подтвержденные
$ git commit -m"added tests" #Создаем коммит с сообщением
$ git push origin master #Выгружаем локальный репозиторий в удаленный репозиторий шестой лабораторной
```
Работа с Travis
```ShellSession
#Авторизуемся своим GITHUB аккаунтом
$ travis login --auto
#Включаем репозиторий в Travis
$ travis enable
```
Заключительные действия
```ShellSession
$ mkdir artifacts #Создаем каталог artifacts
sleep 20s && gnome-screenshot --file artifacts/screenshot.png #Делаем снимок экрана и помещаем его в каталог artifacts
$ open https://github.com/${GITHUB_USERNAME}/lab06 #Открываем репозиторий шестой лабораторной на GitHub
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
