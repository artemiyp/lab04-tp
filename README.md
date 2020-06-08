## Homework III

<a href="https://yandex.ru/efir/?stream_id=vjKAlxJ0UQrs"><img src="https://raw.githubusercontent.com/tp-labs/lab03/master/preview.png" width="640"/></a>

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.
```sh
$ cat formatter.cpp <<EOF
#include "formatter.h"

std::string formatter(const std::string& message)
{
    std::string res;
    res += "-------------------------\n";
    res += message + "\n";
    res += "-------------------------\n";
    return res;
}
EOF
$ git add CMakeLists.txt 
$ git commit -m "added CMakeLists.txt"
[master 15829ed] added CMakeLists.txt
 1 file changed, 7 insertions(+)
 create mode 100644 formatter_lib/CMakeLists.txt
$ git push origin master
Enumerating objects: 92, done.
Counting objects: 100% (92/92), done.
Delta compression using up to 4 threads
Compressing objects: 100% (89/89), done.
Writing objects: 100% (92/92), 1.02 MiB | 13.57 MiB/s, done.
Total 92 (delta 42), reused 0 (delta 0)
remote: Resolving deltas: 100% (42/42), done.
To https://github.com/artemiyp/lab03-tp
 * [new branch]      master -master
```

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
```sh
$ cat CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(formatter_exlib)
>
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
>
include_directories(../formatter_lib)
add_subdirectory(../formatter_lib formatter)
add_library(formatter_ex STATIC formatter_ex.cpp formatter_ex.h)
>
target_link_libraries(formatter_ex formatter)
>EOF
$ git add CMakeLists.txt 
$ git commit -m "added CmakeLists.txt for *ex_lib"
[master 4f60aaa] added CmakeLists.txt for *ex_lib
1 files changed, 11 insertions(+)
create mode 100644 formatter_ex_lib/CMakeLists.txt
$ git push origin master
Enumerating objects: 92, done.
Counting objects: 100% (92/92), done.
Delta compression using up to 4 threads
Compressing objects: 100% (89/89), done.
Writing objects: 100% (92/92), 1.02 MiB | 13.57 MiB/s, done.
Total 92 (delta 42), reused 0 (delta 0)
remote: Resolving deltas: 100% (42/42), done.
To https://github.com/artemiyp/lab03-tp
 * [new branch]      master -master
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.
```sh
$ cat CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(hello_world)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(hello_world hello_world.cpp)
include_directories(../formatter_ex_lib)
add_subdirectory(../formatter_ex_lib formatter_ex)
target_link_libraries(hello_world formatter_ex)

EOF
$ git add CMakeLists.txt
$ git commit -m "add CMakeLists for hello"
[master 445453c] add CMakeLists for hello
1 file changed, 11 insertions(+)
create mode 100644 hello_world_application/CMakeLists.txt
$ git push origin master
Enumerating objects: 10, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 805 bytes | 805.00 KiB/s, done.
Total 6 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 1 local object.
To https://github.com/artemiyp/lab03-tp
   fd1401b..6af527b  master -master
```
```sh
$ cat CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(solver)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(solver equation.cpp)
include_directories(../formatter-ex-lib)
add_subdirectory(../formatter_ex_lib formatter_ex)
add_library(solver_lib STATIC ../solver_lib/solver.cpp ../solver_lib/solver.h)
target_link_libraries(solver solver_lib formatter_ex)
EOF
$ git add CMakeLists.txt 
$ git commit -m "add CMakeLists for solver"
$ git push origin master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 580 bytes | 580.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/artemiyp/lab03-tp
   6af527b..500fbb2  master -master
```

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2020 The ISC Authors
```