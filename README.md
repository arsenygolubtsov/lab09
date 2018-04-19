[![Build Status](https://travis-ci.org/arsenygolubtsov/lab07.svg?branch=master)](https://travis-ci.org/arsenygolubtsov/lab07)

## Laboratory work VII

Данная лабораторная работа посвещена изучению систем документирования исходного кода на примере **Doxygen**

```ShellSession
$ open https://www.stack.nl/~dimitri/doxygen/manual/index.html
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab07** на сервисе **GitHub**
- [x] 2. Выполнить инструкцию учебного материала
- [x] 3. Ознакомиться со ссылками учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ export TEACHER_EMAIL=<rusdevops@gmail.com|justcppdev@gmail.com>
$ alias edit=<nano|vi|vim|subl>
$ alias gsed=sed # for *-nix system
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```

```ShellSession
$ wget https://redirector.gvt1.com/edgedl/go/go1.9.2.linux-amd64.tar.gz
$ tar -C . -xzf go1.9.2.linux-amd64.tar.gz
$ rm -rf go1.9.2.linux-amd64.tar.gz
$ echo "export GOROOT=`pwd`/go" >> scripts/activate
$ echo "export GOPATH=`pwd`/go_modules" >> scripts/activate
$ echo "export PATH=\${PATH}:\${GOROOT}/bin" >> scripts/activate
$ echo "export PATH=\${PATH}:\${GOPATH}/bin" >> scripts/activate
$ source scripts/activate
$ go get github.com/prasmussen/gdrive
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab06 projects/lab07
$ cd projects/lab07
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab07
```

```ShellSession
$ mkdir docs
$ doxygen -g docs/doxygen.conf
$ cat docs/doxygen.conf | less
```

```ShellSession
$ gsed -i 's/\(PROJECT_NAME.*=\).*$/\1 print/g' docs/doxygen.conf
$ gsed -i 's/\(EXAMPLE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ gsed -i 's/\(INCLUDE_PATH.*=\).*$/\1 examples/g' docs/doxygen.conf
$ gsed -i 's/\(EXTRACT_ALL.*=\).*$/\1 YES/g' docs/doxygen.conf
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

```ShellSession
$ git add .
$ git commit -m"added doxygen.conf"
$ git push origin master
```

```ShellSession
$ travis login --auto
$ travis enable
```

```ShellSession
$ doxygen docs/doxygen.conf
$ ls | grep "[^docs]" | xargs rm -rf
$ mv docs/html/* . && rm -rf docs
$ git checkout -b gh-pages
$ git add .
$ git commit -m"added documentation"
$ git push origin gh-pages
$ git checkout master
```

```ShellSession
$ mkdir artifacts && cd artifacts
$ sleep 20s && gnome-screenshot --file artifacts/screenshot.png
# for macOS: $ screencapture -T 20 artifacts/screenshot.png
# open https://${GITHUB_USERNAME}.github.io/lab07/print_8hpp.html
$ gdrive upload screenshot.png
$ SCREENSHOT_ID=`gdrive list | grep screenshot | awk '{ print $1; }'`
$ gdrive share ${SCREENSHOT_ID} --role reader --type user --email ${TEACHER_EMAIL}
$ echo https://drive.google.com/open?id=${SCREENSHOT_ID}
```

## Report

```ShellSession
$ popd
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
