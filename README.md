# Cmake Projects Template

Generate template projects for C/C++

## Build

How to debug build for example.

```shell
cmake --no-warn-unused-cli -DCMAKE_EXPORT_COMPILE_COMMANDS:BOOL=TRUE -DCMAKE_BUILD_TYPE:STRING=Debug -H/path/to/ -B/path/to/build -G "Unix Makefiles"
cmake --build /path/to/build --parallel 10
```

## Directory tree

```text:
.
├── readme.md
├── .clang-format.yml
├── .gitignore
├── CMakeList.txt
├── projects
│  ├── project1
│  │  ├── main.cpp
│  │  ├── *.cpp
│  │  └── *.hpp
│  ├── project2
│  │  ├── main.cpp
│  │  ├── *.cpp
│  │  └── *.hpp
│  └── ...
├── test
│  ├── test1
│  │  ├── main.cpp
│  │  ├── *.cpp
│  │  └── *.hpp
│  ├── test2
│  │  ├── main.cpp
│  │  ├── *.cpp
│  │  └── *.hpp
│  └── ...
├── src
│  ├── foo.cpp
│  ├── bar.cpp
│  └── ...
├── include
│  ├── baz.hpp
│  ├── qux.hpp
│  └── ...
├── third-party
│  ├── hoge
│  ├── fuga
│  └── ...
├── lib
│  ├── piyo
│  ├── xyzzy
│  └── ...
└── bin
   ├── project1
   ├── project2
   ├── ...
   ├── test1
   ├── test2
   └── ...
```
