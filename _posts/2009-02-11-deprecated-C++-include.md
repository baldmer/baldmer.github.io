---
layout: post
title: Deprecated C++ include
tags: [C++, OOP]
---

I've found some deprecated code while testing g++ compiler. Take a look at the following clasical program.

```cpp
#include <iostream.h>
int main(){
        cout<<"hello world"<<endl;
        return 1;
}
```

Now, try to compile this code snippet.

```
$ g++ -o hello_world hello_world.cpp 
hello_world.cpp:1:22: error: iostream.h: No such file or directory 
hello_world.cpp: In function ‘int main()’: 
hello_world.cpp:5: error: ‘cout’ was not declared in this scope 
hello_world.cpp:5: error: ‘endl’ was not declared in this scope 
```

As you can see there is an error with the program. This is because `iostream.h` is deprecated, to get running this simple program you should use `iostream` instead `iostream.h` and the std namespace.

```cpp
#include <iostream>

using namespace std;

int main(){

        cout<<"hello world"<<endl;
        return 1;

}
```

Compile again.

```
$ g++ -o hello_world hello_world.cpp 
$ ./hello_world 
hello world 
```

Now, it compiles like a charm.
