[![Build Status](https://travis-ci.org/Goganych/lab05.svg?branch=master)](https://travis-ci.org/Goganych/lab05)

## Laboratory work V

Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса **Travis CI**

```ShellSession
$ open https://travis-ci.org
```

## Tasks

- [x] 1. Авторизоваться на сервисе **Travis CI** с использованием **GitHub** аккаунта
- [x] 2. Создать публичный репозиторий с названием **lab05** на сервисе **GitHub**
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Включить интеграцию сервиса **Travis CI** с созданным репозиторием
- [x] 5. Получить токен для **Travis CLI** с правами **repo** и **user**
- [x] 6. Получить фрагмент вставки значка сервиса **Travis CI** в формате **Markdown**
- [x] 7. Установить [**Travis CLI**](https://github.com/travis-ci/travis.rb#installation)
- [x] 8. Выполнить инструкцию учебного материала
- [x] 9. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Установка значений для сервиса **GitHub** и **Travis CI**
```ShellSession
$ export GITHUB_USERNAME=Goganych # Устанавливаем значение переменной окружения GITHUB_USERNAME
$ export GITHUB_TOKEN=*************************** # Устанавливаем значение переменной окружения GITHUB_TOKEN
```


"Связываемся" с 5ой лабораторной работой
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05 #Клонируем из lab04 в lab5
$ cd projects/lab05 # заходим в директорию lab05
$ git remote remove origin #Отключаемся от lab04
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05 #Подключаемся к lab5
```
Создание файла *.travis.yml* и запись в него информации о языке программирования
```ShellSession
$ cat > .travis.yml <<EOF
language: cpp 
EOF
```
Дополнение файла файла *.travis.yml* и запись в него следующей инфомации:
```ShellSession
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
```
Дополнение файла файла *.travis.yml* и запись в него следующей инфомации:
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
Вход в Travis с помощью токена GITHUB

```ShellSession
$ travis login --github-token ${GITHUB_TOKEN}
#Но у меня возникли проблемы с этой командой и пришлось её немного доработать,
#вот как у меня получилось:
$ruby -S travis login --github-token ${GITHUB_TOKEN}
```
Проверка репозитория на файл *.travis.yml*
```ShellSession
$ travis lint
```
Вставка значка сервиса Travis CI в формате Markdown
```ShellSession
$ ex -sc '1i|[![Build Status](https://travis-ci.org/Goganych/lab05.svg?branch=master)](https://travis-ci.org/Goganych/lab05)' -cx README.md
```
Выгрузка локального репозитория в онлайн репозиторий и применение всех изменений
```ShellSession
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
```
Используем пакет команд Travis
```ShellSession
$ travis lint #Показывает ошибки и предупреждения .travis.yml
$ travis accounts #Показываем список акаунтов, которые мы можем настраивать
$ travis sync #Обновляем информацию о пользователях
$ travis repos #Выводит список репозиториев с информацией об их активности
$ travis enable #Активируем репозиторий в Travis CI
$ travis whatsup #Показываем последние изменения в репозиториях
$ travis branches #Показываем последние изменения в ветках нашего репозитория
$ travis history #Отображем всю историю данного проекта
$ travis show #Отображаем информацию о последней сборке проекта
```

## Report

```ShellSession
$ popd
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
