## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [Х] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [Х] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [Х] 3. Ознакомиться со ссылками учебного материала
- [Х] 4. Выполнить инструкцию учебного материала
- [Х] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

Подготовительные действия - инициализируем важные переменные (логин на github, почту и токен на repo)
```ShellSession
#Присваиваем переменным ниже соответствующие значения (логин на github, почту и токен на repo)
$ export GITHUB_USERNAME=NickTikhomirov
$ export GITHUB_EMAIL=nicktikhomirov02@gmail.com
$ export GITHUB_TOKEN=***************************
#Привязываем команде edit вызов nano
$ alias edit=nano
```

Подготовка рабочего места
```ShellSession
#Входим в директорию workspace, созданную в предыдущей лабораторной работе
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```ShellSession
#Создаём директорию для файлов конфигурации
$ mkdir ~/.config
#Создали файл hub и записали в него текст ниже
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```ShellSession
#Создаём директорию, посвящённую этой лабораторной работе и заходим в неё
$ mkdir projects/lab02 && cd projects/lab02
#Создаём пустой репозиторий
$ git init
Initialized empty Git repository in /home/Никита/NickTikhomirov/workspace/projects/lab02/.git/

#Установка переменных файла конфигурации
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
#Вывод конфигураций
$ git config -e --global
#Добавление ссылки на репозиторий на Github
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
#Получаем изменения с Github
$ git pull origin master
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/NickTikhomirov/lab02
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
#Создали Readme.md
$ touch README.md
#Посмотрели статус репозитория
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.md

nothing added to commit but untracked files present (use "git add" to track)

#Добавили readme в список на фиксированные изменения
$ git add README.md
#Закоммитили эти изменения
$ git commit -m"added README.md"
[master 22199d7] added README.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md

#Отправили изменения на Github
$ git push origin master
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 286 bytes | 143.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/NickTikhomirov/lab02.git
   05de67c..22199d7  master -> master

```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
.idea/
```

```ShellSession
#Получаем изменения с Github
$ git pull origin master
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/NickTikhomirov/lab02
 * branch            master     -> FETCH_HEAD
   22199d7..0d0a036  master     -> origin/master
Updating 22199d7..0d0a036
Fast-forward
 .gitignore | 4 ++++
 1 file changed, 4 insertions(+)

#Получаем логи коммитов с Github
$ git log
commit 0d0a03605907efe4875b78afb9c06647ed83307f (HEAD -> master, origin/master)
Author: NickTikhomirov <47750222+NickTikhomirov@users.noreply.github.com>
Date:   Mon Mar 18 20:58:40 2019 +0300

    Create .gitignore

commit 22199d7582dd09e30841e664fe7e744401153a9a
Author: NickTikhomirov <nicktikhomirov02@gmail.com>
Date:   Mon Mar 18 18:49:41 2019 +0300

    added README.md

commit 05de67c39eb87dc1ae8d49dddbc385358fabcd23
Author: NickTikhomirov <47750222+NickTikhomirov@users.noreply.github.com>
Date:   Mon Mar 18 18:26:51 2019 +0300

    Initial commit

```

Создание директорий и файла с кодом процедуры вывода на экран
```ShellSession
$ mkdir sources
$ mkdir include
$ mkdir examples
#Создание файла с кодом процедуры вывода на экран
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

#Создание файла с кодом
```ShellSession
#Запись кода в файл
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

#Создание файла с кодом
```ShellSession
#Запись кода в файл
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

#Создание файла с кодом
```ShellSession
#Запись кода в файл
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

Редактирование README.md
```ShellSession
$ edit README.md
```

Просмотр состояния репозитория, отправка на Github 
```ShellSession
#Получение статуса репозитория
$ git status
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        examples/
        include/
        sources/

nothing added to commit but untracked files present (use "git add" to track)

#Сохранение изменений
$ git add .
#Коммит изменений
$ git commit -m"added sources"
 4 files changed, 32 insertions(+)
 create mode 100644 examples/example1.cpp
 create mode 100644 examples/example2.cpp
 create mode 100644 include/print.hpp
 create mode 100644 sources/print.cpp

#Отправка на Github
$ git push origin master
Counting objects: 9, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (9/9), 969 bytes | 193.00 KiB/s, done.
Total 9 (delta 0), reused 0 (delta 0)
To https://github.com/NickTikhomirov/lab02.git
   0d0a036..dfedcab  master -> master

```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
4. Добавьте этот файл в локальную копию репозитория.
5. Закоммитьте изменения с *осмысленным* сообщением.
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
8. Запуште изменения в удалёный репозиторий.
9. Проверьте, что история коммитов доступна в удалёный репозитории.

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
3. **commit**, **push** локальную ветку в удалённый репозиторий.
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
7. **commit**, **push**.
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
12. Удалите локальную ветку `patch1`.

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
7. Сделайте *force push* в ветку `patch2`
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)
