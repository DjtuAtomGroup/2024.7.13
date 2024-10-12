## 资源
### 库
* [C++标准库官方](https://en.cppreference.com/w/)
* [Boost官方](https://www.boost.org/)
* [gRPC](https://grpc.io/)
* [FTXUI](https://arthursonzogni.github.io/FTXUI/)
### 其它
* [Awesome](https://github.com/sindresorhus/awesome)
* [Awesome Cpp](https://github.com/fffaraz/awesome-cpp)
* [C++菜鸟教程](https://www.runoob.com/cplusplus/cpp-tutorial.html)
* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
## 库与框架
### [标准库](https://en.cppreference.com/w/)
* libstdc++: GUN下的C++标准库实现，g++默认使用使用
* libc++: LLVM下的C++标准库实现，clang++更多使用
#### 功能
##### 字符串与数字互转
字符串转数字：std::stoi([string])
数字转字符串：std::to_string([int])
##### 函数绑定
std::bind(&[函数], [参数])
std::bind(&[函数], [this], [参数])
### FTXUI
## 实操(设计模式)
### 前向声明
1. a,b两个类，想要实现a调用b的公有成员函数，b调用a的公有成员函数，且a,b分别有两个不同的头文件和两个不同的源文件
采用前向声明 + 类内部实例化，b头文件和a头文件分别前向声明对方并且在类内部实例化对方，最后源文件中互相包含对方头文件
## 语言原理(编译原理)
