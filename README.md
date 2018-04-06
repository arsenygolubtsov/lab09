[![Build Status](https://travis-ci.org/arsenygolubtsov/lab05.svg?branch=master)](https://travis-ci.org/arsenygolubtsov/lab05)
Laboratory work V
Данная лабораторная работа посвещена изучению систем непрерывной интеграции на примере сервиса Travis CI

$ open https://travis-ci.org
Tasks
 1. Авторизоваться на сервисе Travis CI с использованием GitHub аккаунта
 2. Создать публичный репозиторий с названием lab05 на сервисе GitHub
 3. Ознакомиться со ссылками учебного материала
 4. Включить интеграцию сервиса Travis CI с созданным репозиторием
 5. Получить токен для Travis CLI с правами repo и user
 6. Получить фрагмент вставки значка сервиса Travis CI в формате Markdown
 7. Установить Travis CLI
 8. Выполнить инструкцию учебного материала
 9. Составить отчет и отправить ссылку личным сообщением в Slack
Tutorial
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_TOKEN=<полученный_токен>
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ rvm autolibs disable
$ rvm install ruby-2.4.2
$ rvm use 2.4.2 --default
$ gem install travis
$ git clone https://github.com/${GITHUB_USERNAME}/lab04 projects/lab05
$ cd projects/lab05
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab05
$ cat > .travis.yml <<EOF //создание файла, в котором прописаны язык программирования и параметры компиляции для сборки проекта
language: cpp
EOF
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
$ travis login --github-token ${GITHUB_TOKEN} //авторизация на travis через аккаунт github
$ travis lint // travis lint - это инструмент, который проверяет файл travis.yml на возможные проблемы
$ ex -sc '1i|<фрагмент_вставки_значка>' -cx README.md
$ git add .travis.yml
$ git add README.md
$ git commit -m"added CI"
$ git push origin master
$ travis lint
$ travis accounts //указывает используемый аккаунт на travis
$ travis sync //синхронизация репозиториев на github с travis
$ travis repos //выводит данные о репозиториях, доступных на travis
$ travis enable //включает указанный проект в число отслеживаемых travis
$ travis whatsup //выводит список последних сборок
$ travis branches //отображает последние версии сборок для каждой ветки
$ travis history //выводит историю сборок
$ travis show //отображение сборки или задания
Report
$ popd
$ export LAB_NUMBER=05
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
Links
Travis Client
AppVeyour
GitLab CI
Copyright (c) 2017 Братья Вершинины
