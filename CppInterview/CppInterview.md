# 1. C++面向对象三大特性:继承\封装\多态

**(1) 封装性：** 将客观事物抽象成类，把复杂的细节封装在内部，仅提供简单的接口即可,封装可以隐藏实现细节，使得代码模块化,保护或者防止数据被无意破坏。每个类自身的数据和方法实现权限控制，只让可信的类或者对象操作,对不可信的类进行信息隐藏；

**(2) 继承性:** 继承可以使得子类具有父类的各种属性和方法,无需重新编写；

**(3) 多态性：** 多态是指不同对象接收相同消息时产生不同的动作,通过基类的指针或者引用,在运行时动态调用实际绑定对象函数的行为。在基类的函数前加上virtual关键字，在派生类中重写该函数，运行时将会根据对象的实际类型来调用相应的函数。



# 2.类的访问与继承权限

- **类的访问:**

**(1) public:** 用该关键字修饰的成员表示<u>公有成员</u>，该成员不仅可以在类内可以被访问，在类外也是可以被访问的,是类对外提供的可访问接口；

**(2) private:** 用该关键字修饰的成员表示<u>私有成员</u>，该成员仅在类内可以被访问,在类体外是隐藏状态；

**(3) protected:** 用该关键字修饰的成员表示<u>保护成员</u>，保护成员在类体外同样是隐藏状态，但是对于该类的<u>派生类</u>来说,相当于<u>公有成员</u>,在派生类中可以被访问。

- **类的继承:**

**(1) 继承方式public:** 基类成员在派生类中的访问权限保持不变；

**(2) 继承方式private:** 基类所有成员在派生类中的访问权限都会变为私有(private)权限；

**(3) 继承方式protected:** 基类的公有成员和保护成员在派生类中的访问权限都会变为保护权限，私有成员在派生类中的访问权限仍然是私有权限。

- **构造和析构:**

构造函数和析构函数无法被继承，在整个层次中的所有的构造函数和析构函数都必须被调用。子类的构造函数会显示的调用父类的构造函数或隐式的调用父类的默认的构造函数进行父类部分的初始化。


# 3. C++程序的内存分区 

- **分区:**

**(1) 栈区(stack):** 由编译器自动分配和释放，存放函数的形参，局部变量等；

**(2) 堆区(heap):** 一般由程序员释放(动态内存申请与释放)，如果程序员没有进行释放，程序结束由OS统一回收；

**(3) 全局区\静态区(static:** 用于存放全局变量和静态变量，分为两部分，初始化的放置.data段，没有初始化的放在.bss段，没有初始化的全局变量会初始化为0，静态成员变量在初始化前没有进行内存分配；

**(4) 代码区:** 存放代码。

- **堆栈区别:**

**(1)管理方式:** 堆由用户手动申请手动释放，容易产生内存泄露；栈由系统自由分配自动回收。

**(2)生长方式:** 对自下而上，栈自上而下。

**(3)碎片:** 堆中内存分配和释放会产生内存碎片，栈采用后进先出的机制，不会产生碎片。


# 4. C++编译过程
编译流程分为四个阶段:<u>预处理</u> <u>编译</u> <u>汇编</u> <u>链接</u>

**(1)预处理:** 处理一些#定义的命令或语句(如`#define #include #ifdef`)，生成.i文件；

**(2)编译:** 进行语法分析等，生成.s的汇编文件；

**(3)汇编:** 将对应的汇编指令翻译成机器指令，生成.o目标文件；

**(4)链接:** 动态链接或静态链接，将多个目标文件以及所需要的库链接成最终的生成可执行文件。


# 5. C++泛型编程
- 主要通过模板实现，包括<u>函数模板</u>和<u>类模板</u>。

**(1)函数模板:** 以`template`关键字开始，形参由关键字`class`或`typename`构成如下，

```c++
template<typename T>
int add(const T &v1, const T &v2) {
    return v1 + v2;
}
```

**(2)类模板:** 以`template`关键字开始，在调用时需要为模板形参显示指定形参，

```c++
template<typename Type>
class Queue {
public:
    Queue();  
    Type & front();
    const Type & front() const;
    void push(const Type &);
    void pop();
    bool empty() const;
private:
    // …
};

// 调用
Queue<int> test_int;
Queue<double> test_double;
```

**typename和class区别与联系:**  当表示模板参数时，二者没有区别；当模板函数内部的成员表示一个类型时(标识嵌套从属类型名称)，必须使用`typename`，如果不使用`typename`编译器会把从属嵌套的类型视为一个成员变量，在遇到\*时，会把\*解析成乘法，而不是解引用符号。如下

```c++
template <class T>
class MyTest {
    public:
	class T::val_type func(const T &c);
};

template <class T>
class T::val_type MyTest<T>::func(const T &c) {
	typename T::val_type *ptr;  // 必须使用typename
    class T::val_type t;
	return typename T::val_type();
}
```



# 6. C++编译器自动为类产生的缺省函数

- [ ] 未完成


# 7. const关键字

- [ ] 未完成


# 8. static关键字

- [ ] 未完成

# 9. C++智能指针及其原理

c++的智能指针是一种管理内存动态分配的对象，提供了自动内存管理机制，表面了手动释放内存的繁琐和潜在的内存泄露问题。
智能指针的原理基于RAII原则，即资源获取初始化。智能指针在构造函数中获取资源(动态分配的内存)，并在析构函数中释放资源，从而确保资源的正确释放。
C++11中存在三个智能指针: weak_ptr, unique_ptr, shared_ptr。

1.unique_ptr: 独占所有权的智能指针，确保只有一个指针可以访问管理的对象。当unique_ptr被销毁时，会自动释放所拥有的对象。其不可被赋值，但可以通过std::move()函数进行所有权转移。

```c++
std::unique_ptr<string> pu1(new string ("hello world")); 
std::unique_ptr<string> pu2; 
pu2 = pu1;                                      // #1 非法
std::unique_ptr<string> pu3; 
pu3 = std::unique_ptr<string>(new string ("You"));   // #2 合法
string* ps =  pu1.get();  
```

2.shared_ptr: 共享所有权的智能指针，可以被多个指针同时访问和共享所有的对象，使用引用计数追踪多少个指针指向该对象，当引用计数为0时，即没有任何指针指向对象，资源会被释放。shared_ptr可以被赋值和复制。

shared_ptr 是为了解决 auto_ptr 在对象所有权上的局限性(auto_ptr 是独占的), 在使用引用计数的机制上提供了可以共享所有权的智能指针。

成员函数：

use_count 返回引用计数的个数

unique 返回是否是独占所有权( use_count 为 1)

swap 交换两个 shared_ptr 对象(即交换所拥有的对象)

reset 放弃内部对象的所有权或拥有对象的变更, 会引起原有对象的引用计数的减少

get 返回内部对象(指针), 由于已经重载了()方法, 因此和直接使用对象是一样的.如

```c++
std::shared_ptr<int> sp(new int(1)); 
```

3.weak_ptr: 不控制对象生命周期的智能指针，指向shared_ptr所管理的对象。对对象的内存管理依然是强引用的shared_ptr，weak_ptr只提供对管理是对象的访问手段。

不能通过weak_ptr直接访问对象的方法，比如B对象中有一个方法print()，我们不能这样访问，pa->pb_->print()，因为pb_是一个weak_ptr，应该先把它转化为shared_ptr，如：
```c++
shared_ptr<B> p = pa->pb_.lock();
p->print();
```

weak_ptr 没有重载*和->但可以使用 lock 获得一个可用的 shared_ptr 对象. 注意, weak_ptr 在使用前需要检查合法性.

expired 用于检测所管理的对象是否已经释放, 如果已经释放, 返回 true; 否则返回 false.

lock 用于获取所管理的对象的强引用(shared_ptr). 如果 expired 为 true, 返回一个空的 shared_ptr; 否则返回一个 shared_ptr, 其内部对象指向与 weak_ptr 相同.

use_count 返回与 shared_ptr 共享的对象的引用计数.

reset 将 weak_ptr 置空.

weak_ptr 支持拷贝或赋值, 但不会影响对应的 shared_ptr 内部对象的计数.


# 10. share_ptr和weak_ptr的核心实现
weakptr的作为弱引用指针，其实现依赖于counter的计数器类和share_ptr的赋值和构造，所以先把counter和share_ptr简单实现

1.Counter简单实现

```c++
class Counter {
public:
    Counter() : s(0), w(0){}
    int s;	// share_ptr的引用计数
    int w;	// weak_ptr的引用计数
}
```

counter对象的目地就是用来申请一个块内存来存引用基数，s是share_ptr的引用计数，w是weak_ptr的引用计数，当w为0时，删除Counter对象。

2.share_ptr的简单实现

```c++
template <class T>
class WeakPtr; //为了用weak_ptr的lock()，来生成share_ptr用，需要拷贝构造用

template <class T>
class SharePtr {
public:
    SharePtr(T *p = 0) : _ptr(p) {
        cnt = new Counter();
        if (p) {
            cnt->s = 1;
        } 
        cout << "in construct " << cnt->s << endl;
    }
    ~SharePtr() {
        release();
    }

    SharePtr(SharePtr<T> const &s) {
        cout << "in copy con" << endl;
        _ptr = s._ptr;
        (s.cnt)->s++;
        cout << "copy construct" << (s.cnt)->s << endl;
        cnt = s.cnt;
    }
    //为了用weak_ptr的lock()，来生成share_ptr用，需要拷贝构造用
    SharePtr(WeakPtr<T> const &w) {
        cout << "in w copy con " << endl;
        _ptr = w._ptr;
        (w.cnt)->s++;
        cout << "copy w  construct" << (w.cnt)->s << endl;
        cnt = w.cnt;
    }
    SharePtr<T> &operator=(SharePtr<T> &s) {
        if (this != &s) {
            release();
            (s.cnt)->s++;
            cout << "assign construct " << (s.cnt)->s << endl;
            cnt = s.cnt;
            _ptr = s._ptr;
        }
        return *this;
    }
    T &operator*() {
        return *_ptr;
    }
    T *operator->() {
        return _ptr;
    }
    friend class WeakPtr<T>; //方便weak_ptr与share_ptr设置引用计数和赋值

protected:
    void release() {
        cnt->s--;
        cout << "release " << cnt->s << endl;
        if (cnt->s < 1) {
            delete _ptr;
            if (cnt->w < 1) {
                delete cnt;
                cnt = NULL;
            }
        }
    }

private:
    T *_ptr;
    Counter *cnt;
}
```

share_ptr的给出的函数接口为：构造，拷贝构造，赋值，解引用，通过release来在引用计数为0的时候删除_ptr和cnt的内存。

3.weak_ptr简单实现

```c++
template <class T>
class WeakPtr {
public: //给出默认构造和拷贝构造，其中拷贝构造不能有从原始指针进行构造
    WeakPtr() {
        _ptr = 0;
        cnt = 0;
    }
    WeakPtr(SharePtr<T> &s) : _ptr(s._ptr), cnt(s.cnt) {
        cout << "w con s" << endl;
        cnt->w++;
    }
    WeakPtr(WeakPtr<T> &w) : _ptr(w._ptr), cnt(w.cnt) {
        cnt->w++;
    }
    ~WeakPtr() {
        release();
    }
    WeakPtr<T> &operator=(WeakPtr<T> &w) {
        if (this != &w) {
            release();
            cnt = w.cnt;
            cnt->w++;
            _ptr = w._ptr;
        }
        return *this;
    }
    WeakPtr<T> &operator=(SharePtr<T> &s) {
        cout << "w = s" << endl;
        release();
        cnt = s.cnt;
        cnt->w++;
        _ptr = s._ptr;
        return *this;
    }
    SharePtr<T> lock() {
        return SharePtr<T>(*this);
    }
    bool expired() {
        if (cnt) {
            if (cnt->s > 0) {
                cout << "empty" << cnt->s << endl;
                return false;
            }
        }
        return true;
    }
    friend class SharePtr<T>; // 方便weak_ptr与share_ptr设置引用计数和赋值
    
protected:
    void release() {
        if (cnt) {
            cnt->w--;
            cout << "weakptr release" << cnt->w << endl;
            if (cnt->w < 1 && cnt->s < 1) {
                // delete cnt;
                cnt = NULL;
            }
        }
    }

private:
    T *_ptr;
    Counter *cnt;
}
```

weak_ptr一般通过share_ptr来构造，通过expired函数检查原始指针是否为空，lock来转化为share_ptr。

# 11. C++中动态链接库和静态链接u的区别

1.链接时期

静态链接库：在程序编译时，静态库的内容会被复制到最终的可执行文件中。当你运行程序时，不需要库文件，因为所有的功能都已经包含在可执行文件里了。

动态链接库：程序在编译时并不复制库中的代码，而是在程序运行时加载库文件。这意味着库文件必须在程序运行时可用。

2.文件大小

静态链接库通常会导致较大的可执行文件大小，因为所有使用的库代码都被复制进去了。

动态链接库允许可执行文件小一些，因为代码是在运行时才被加载。

3.内存占用
静态链接库的缺点是如果有多个程序使用相同的库，每个程序都有自己的副本，这将导致内存的浪费。

动态链接库可以由多个正在运行的程序共享，只需在内存中有一个副本即可。

4.分发和更新

静态链接库使得更新库变得复杂，因为每个应用都有自己的副本，所以每个应用都需要重新编译和分发。

使用动态链接库时，只需替换库文件并且确保API兼容性，所有使用该库的应用程序就可以直接利用新版本的库，无须重新编译。

5.跨平台兼容性

静态链接库生成的可执行文件更易于在没有安装相应库的不同系统上运行，因为它们包括了所有需要的代码。

对于动态链接库，需要确保目标系统上存在正确版本的库文件。

6.链接错误和冲突

静态链接库可能会引起版本冲突问题，尤其是当不同的库依赖同一个库但又各自静态链接了不同版本时。

动态链接库可以减少这种冲突，因为同一份库文件被所有依赖它的程序共享。


# 12. 常用关键字:#define, const, typedef, using, inline

1.#define余const区别

(1) 起作用阶段

#define 是在预处理阶段起作用，经define定义的变量全部替换。

const 是在编译链接时候起作用。

(2) 是否安全

#define 不安全，不会做类型检查，只做文本的简单替换。

const 在定义的时候，就是类型加常量名，会做类型检查，

(3) 占用内存与开销

#define 在预编译后，将用到地方都展开，多个地方用到的话，会展开多次，占用代码段内存。

const 变量会在常量区，只有一份。之后在用到的地方，编译器都直接给的是地址，省了内存空间。

(4) 特有功能

#define 特有功能，用来防止文件重复引用。也可以定义函数等。

2.#define与inline区别

(1) 起作用阶段

define 是在预处理阶段

inline 是在编译链接 阶段，inline 是否起作用由编译器决定。

(2) 是否安全

inline 函数有类型检查，相比较 宏 是更安全的

(3) inline的用法

inline 用法随着C++的修订，越来越多。但对于普通常用的修饰函数，有几点注意

inline对于编译器而言只是一个建议，不同编译器关于inline实现机制可能不同，一般建议：将函数规模较小(即函数不是很长，具体没有准确的说法，取决于编译器内部实现)、不是递归、且频繁调用的函数采用inline修饰，否则编译器会忽略inline特性。

3.typedef与using

使用方法

```c++
typedef int points;
using points = int;
```

using 可以使用auto进行类型推导辅助简化复杂类型的声明。

typedef 不能用于创建模板类型别名。


# 13 右值引用

# 14 std::forward用法

# 15 C++中四种cast转换
https://zhuanlan.zhihu.com/p/578828427

c++ 中的四种cast转换是: static_cast, const_cast, dynamic_cast, reinterpret_cast

(1) static_cast(静态类型转换): 

(2) const_cast:

(3) dynamic_cast(动态类型转换): 

(4) reinterpret_cast:
