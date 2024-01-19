## 模板
关键字：变量模板
```c++
template <typename T>//也可装载非类型参数<size_t N>
const T pi = static_cast<T>(3.1415926);
```
模板不是任何的程序实体（无实例化时）

使用时：模板变量实例化（显式）
```c++
pi<double>//尖括号内为模板参数T
```
### 立即函数
consteval修饰函数（表明为立即函数，编译时运行）

constexpr修饰变量（表明为编译时常量）

### 模板特化
以斐波那契数列为例：
```c++
using cstring = const char*;
template <>
cstring pi<cstring> = "3.1415926";

template <size_t N>//通用模板
constexpr size_t fibo_v = fibo_v<N-1> + fibo_v<N-2>;

template <>//特化模板
constexpr size_t fibo_v<1> = 1;
template <>//特化模板
constexpr size_t fibo_v<2> = 1;
```
必须有通用模板才能有特化模板，一个通用模板可以有多个特化模板（在递归示例中特化模板作出口）

模板是可以递归的；模板元编程牺牲编译时间换取更少的运行时间
### 函数模板/泛型函数
函数模板隐式实例化（像调用函数一样使用模板函数）
```c++
template <typename T>
auto abs(T v){
    return v < 0 ? -v : v;
}

int main(){
    std::cout << abs(-2) << std::endl;//将-2的类型int给到模板类型T

    return 0;
}
```
实例化模板函数时不创建函数源码，而是直接生成函数代码<br>即源代码没有变化，生成代码有变化，对比#define宏展开直接改变源代码

在上面的代码中，只有T是数值类型才有意义，如T传入结构体，小于无法进行比较（可重载小于号）
```c++
template <typename T, T zero = T(0)>
auto abs(T v){
    return v < 0 ? -v : v;
}

int main(){
    std::cout << abs<int, 0>(-2) << std::endl;//显式给出参赛

    return 0;
}
```
这里通过给出0的定义，对于类仍然类型安全（相当于调用构造函数；需要重载小于号）

#### 偏特化
```c++
template <typename T>
auto abs<T*>(T* v){//只有T是特化的，而传入了T*,并使用了显式实例化
    return v;
}
```
#### 模板缩写
```c++
template <typename T>
auto add(T a,T b){
    return a + b;
}

auto add2(auto a, auto b){
    return a + b;
}

```
两者等价（但auto并不保证类型一致），除非使用decltype保证
```c++
auto add2(auto a, decltype(a) b){
    return a + b;
}
```
建议函数使用template而$\lambda$使用auto
#### 变长参数模板
在C风格的代码中一般先传入一个长度值
```c
auto sum(int count, T v0...){}
```
C++使用折叠表达式和变长模板
```c++
template <typename ...types>
auto sum(types ...args){//...在左边称为左折叠
    return (0 + .. + args);
}
```
若要知道参数数量，无法使用循环，而是通过sizeof...
```c++
sizeof...(args)//sizeof...是表达式，并不是sizeof运算符
```