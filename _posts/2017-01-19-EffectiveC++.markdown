---
layout: post
author: LPF
title: Effective C++
date: 2017-01-19 12:11:01
updated: 2017-02-23 09:39:22
tags:
- C++
categories:
- Study
- Computer
- PL
- C++
- Effective
---
# 第一章 让自己习惯习惯C++

1. 让自己习惯C++

- C++高效编程守则视状况而变化，取决与你使用C++的哪一部分——C、Object-Oriented C++、Template C++、STL

2. 宁可以编译器替换预处理器

- 对于单纯常量，最好以`const`对象或`enum`替换`#define`
- 对于形似函数的宏，最好改用`inline`函数替换`#define`

3. 尽可能使用const

- 将某些东西声明为`const`可帮助编译器侦测出错误用法
- `const`可被施加于任何作用域的对象、函数参数、函数返回类型、成员函数本体
- 编译器强制实施`bitwise constness`，但编写程序时应该使用`概念上的常量性`
- 当`const`和`non-const`成员函数有着实质等价的实现时，令`non-const`版本调用`const`版本可避免代码重复
    
4. 确定对象被使用前已被初始化

- 为内置对象进行手工初始化，因为标准不保证初始化他们
- 构造函数最好使用成员初值列，而不要在构造函数本体使用赋值操作
- 初值列列出的成员变量，其排列次序应该和他们在class中的声明次序相同
- 为免除`跨编译单元之初始化次序`问题，以`local static`对象替换`non-local static`对象

# 第二章 构造/析构/赋值运算

5. 了解C++ 默默编写并调用哪些函数

- 编译器可以暗自为class创建(public)`default`、`copy`构造函数以及`copy assignment`操作符和析构函数

6. 若不想使用编译器自动生成的函数，就该明确拒绝

- 将相应的成员函数声明为`private`并且不予实现，可以避免编译器自动生成该函数，且无须担心会被外部使用到这些函数
- 为避免编译器自动生成拷贝构造和赋值函数，可以使用类似`Uncopyable`这样的`base class`
    
7. 为多态基类声明virtual析构函数

- `polymorphic base classes`应该声明一个`virtual`析构函数
- 如果class带有任何virtual函数，他就应该拥有一个virtual析构函数
- classes的设计目的如果不是作为`base class`使用，或不是为了具备`polymorphically`，就不该声明virtual析构函数

8. 别让异常逃离析构函数
- 析构函数绝对不要吐出异常。如果一个被系够函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后不传播他们或结束程序
- 如果客户需要对某个操作函数运行期间抛出的异常作出反应，那么class应该提供一个普通函数(而非在析构函数中)执行该操作

9. 绝不在构造和析构过程中调用virtual函数

- 在构造和析构期间不要调用virtual函数，因为这类调用从不下降至`derived class`

10. 令`operator=`返回一个`reference to *this`

- 令赋值操作符返回一个`reference to *this`

11. 在`operator=`中处理`自我赋值` 

- 确保对象自我赋值时 `operator=`有良好行为，其中技术包括比较**来源对象**和**目标对象**的地址、精心周到的语句顺序、以及`copy-and-swap`
- 确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确

    ```c++
    class Bitmap{...};
    class Widget{
        ...
    private:
        Bitmap *pb;//pointer, point to an object create from heap
    };
    
    Widget& Widget::operator=(const Widget& rhs)//unsafe
    {
        delete pb;
        pb = new Bitmap(*rhs.pb);
        return *this;
    }
    
    Widget& Widget::operator=(const Widget& rhs)//identity test
    {
        if (this == &rhs) return *this;//identity test
        
        delete pb;
        pb = new Bitmap(*rhs.pb);
        return *this;
    }
    
    Widget& Widget::operator=(const Widget& rhs)//safe
    {
        Bitmap *pOrig = pb; //reserve the original pb
        pb = new Bitmap(*rhs.pb);   //pb point to copy
        delete pOrig;   //delete original pb
        return *this;
    }
    
    Widget& Widget::operator=(const Widget& rhs)//copy and swap
    {
        Widget temp(rhs);
        swap(temp);
        return *this;
    }
    
    Widget& Widget::operator=(Widget rhs)//another copy-and-swap, pass by value, more effctive than previous one
    {
        swap(rhs);
        return *this;
    }
    ```

12. 复制对象时勿忘其每一个成分

- `Copying`函数应该确保复制**对象内的所有成员变量**和**所有base class成分**
- 不要尝试以某个copying函数实现另一个copying函数，应该将共同机能放在第三个函数中，并且两个copying函数共同调用

# 第三章 资源管理

13. 以对象管理资源

- 为防止资源泄漏，使用`RAII`对象，他们在构造函数中获得资源并在析构函数中释放资源

14. 在资源管理类中小心copying行为

- 复制RAII对象必须一并复制他所管理的资源，所以资源的copying行为决定RAII对象的copying行为
- 普遍而常见的RAII class copying行为是：抑制copying、施行引用技术法

15. 在资源管理类中提供对原始资源的访问

- APIs往往要求访问原始资源，所以每一个RAII class应该提供一个取得其所管理资源的方法
- 对原始资源的访问可能经由显示转换或隐式转换。一般而言显示转换较为安全，而隐式转换对客户比较方便

16. 成对使用new和delete时要采用相同形式

- 如果在new表达式中使用[]，则必须在相应的delete表达式中也使用[]
- 如果在new表达式中没有使用[]，一定不要在相应的delete表达式中使用[]

17. 以独立语句将newed对象置入智能指针

- 以独立语句将newed对象置入智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄漏

# 第四章 设计与声明

18. 让接口更容易被使用，不易被误用

- 好的接口很容易被正确使用，不容易被误用。应该在所有接口中努力达成这些性质
- `促进正确使用`的办法包括接口的一致性，以及与内置类型的行为兼容
- `阻止误用`的办法包括建立新类型、限制类型上的操作、束缚对象值，以及消除客户的资源管理任务

19. 设计class犹如设计type

- class的设计就是type的设计，在定义一个新type之前，先确定已经考虑过本条款覆盖的所有讨论主题
    - 新type对象应该被如何创建及销毁
    - 对象的初始化和赋值应该有什么样的区别
    - 新type对象如果以值传递，意味着什么
    - 什么是type的合法值
    - 新type需要配合某个继承体系吗
    - 新的type需要什么样的转换
    - 什么样的操作符和函数对此type是合理的
    - 什么样的标准函数该被驳回
    - 谁该取用新type的成员
    - 什么是新type的未声明接口
    - 新的type有多么一般化，是否应该成为template
    - 是否真的需要一个新的type

20. 宁以`pass-by-reference-to-const`替换`pass-by-value`

- 尽量以`pass-by-reference-to-const`替换`pass-by-value`。前者通常比较高效，并可避免切割问题
- 对于内置类型、STL的迭代器和函数对象，使用`pass-by-value`比较合适

21. 必须返回对象时，别妄想返回其reference

- 绝不要返回pointer或reference指向一个`local stack`对象，或返回reference指向一个`heap allocated`对象，或返回pointer或reference指向一个`local static`对象而有可能有多个这样的对象。
- 当必须返回一个reference和一个object之间抉择时，选择行为正确的那个

22. 将成员变量声明为private

- 切忌将成员变量声明为private，这可赋予客户访问数据的一致性、可细微划分访问控制、允诺约束条件获得保证，并提供class作者以充分的实现弹性
- protected并不比public更具封装性

23. 宁以non-member、non-friend替换member函数

- 宁可以non-member non-friend函数替换member函数，这样做可以增加封装性、包裹弹性和机能扩充性
- 比较自然的做法是将一个便利函数作为non-member函数并且位于与类所在的同一个namespace内，并将不同种类的便利函数分别置于相同namespace的不同头文件下

24. 若所有参数皆需类型转换，则为此采用non-member函数

- 如果需要为某个函数的所有参数(包括被this指针所指的那个隐喻参数)进行类型转换，那么这个函数必须是个non-member

25. 考虑写出一个不拋异常的swap

- 当std::swap对自定义类型效率不高时，提供一个swap成员函数，并确定这个函数不抛出异常
- 如果提供一个`member swap`，那么也该提供一个`non-member swap`来调用前者，对于class，也该特化std::swap
- 调用swap时应针对std::swap使用using声明，然后调用swap时不带任何**命名空间资格修饰**
- 为**用户定义类型**进行std template全特化是好的，但千万不要尝试在std内加入某些对std而言全新的东西

# 第五章 实现

26. 尽可能延后变量定义式的出现时间

- 尽可能延后变量定义式的出现，这样做可增加程序的清晰程度并改善程序效率
- 不只应该延后变量的定义，直到非得使用该变量为止，甚至应该尝试延后这份定义直到能够给他初值实参为止，避免无意义的default构造函数
- 对于循环内使用的变量，除非明确得知`赋值成本`低于`构造+析构成本`，或代码效率高度敏感，可以将变量定义于循环之外，否则将变量定义于循环之内

27. 尽量少做转型动作

- 尽量避免转型，特别是在注重效率的代码中避免`dynamic_cast`。如果有个设计需要转型动作，试着发展无需转型的替代设计
- 如果转型是必要的，试着将它隐藏于某个函数背后。客户随后可以调用该函数，而不需将转型放进他们自己的代码内
- 宁可使用C++式转型，不要使用旧式转型。前者很容易辨认出来，而且也比较容易分门别类

28. 避免返回handles指向对象内部成分

- 避免返回handles(包括references、指针、迭代器)指向对象内部，可以增加封装性，帮助const成员的行为像个const，并将发生`dangling handles`的可能性降至最低

29. 为`异常安全`而努力是值得的

- 异常安全函数即使发生异常也不会泄漏资源或允许败坏数据结构。这样的函数区分为三种可能的保证：基本型、强烈型、不抛异常型
- `强烈保证`往往能够以`copy-and-swap`实现，但`强烈保证`并非对所有函数都可实现和uo具备现实意义
- 函数提供的`异常安全保证`通常最高只等于其所调用之各个函数的`异常安全保证`中的最弱者

30. 透彻了解inlining的里里外外

- 将大多数inlining限制在小型、被频繁调用的函数身上，这可使得日后在调试的过程中而二进制升级更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化
- 不要只因为`function template`出现在头文件，将将他们声明为`inline`

31. 将文件间的编译依存关系降至最低

- 支持`编译依存性最小化`的一般构想是：相依于声明式，而不要相依于定义式，基于此构想的两个手段是`handle classes`和`Interface classes`
- 程序库头文件应该以`完全且仅有声明式`的形式存在，这种做法不论是否涉及templates都适用

# 第六章 继承与面向对象设计

32. 确定public继承塑模出`is-a`关系

- public继承意味着`is-a`，适用于`base class`身上的每一件事一定也适用于`derived class`身上，因为每一个`derived class`对象也都是一个`base class`对象
- 适用于`base class`的某一件事情如果不适用于`derived class`，则应该考虑设计是否有错误，如将矩形的只更改宽度操作加于正方形身上是不可取的(对于正方形，需要同时更改)

33. 避免遮掩继承而来的名称

- `derived class`内的名称会遮掩`base class`内的名称，在`public`继承下是不可取的
- 为了让被遮掩的名称得见，可使用`using声明`或`forwarding functions`

34. 区分接口继承和实现继承

- 接口继承和实现继承不同，在public继承下`derived class`总是继承`base class`的接口
- `pure virtual`函数只具体指定接口继承
- `impure virtual`函数具体指定接口继承及缺省实现继承
- `non-virtual`函数具体指定接口继承以及强制性实现继承

35. 考虑virtual函数以外的其他选择

- virtual函数的替代方案包括NVI手法以及Stratege设计模式的多种形式。NVI手法本身是一个特殊的Template Method设计模式
- 将技能从成员函数移到class外部函数，带来的一个缺点是，非成员函数无法访问class的non-public成员
- function对象的行为就像一般函数指针，这样的对象可接纳`与给定之目标签名式兼容`的所有可调用物

36. 绝不重新定义继承而来的non-virtual函数

- 如果重新定义了，则表示有特化的情况，那么其不该为non-virtual函数

37. 绝不重新定义继承而来的缺省参数值

- 绝对不要重新定义一个继承而来的缺省参数值，因为缺省参数值都是静态绑定的，而virtual函数——唯一应该覆写的东西——是动态绑定的

38. 通过复合塑模出has-a或`根据某物实现出`

- 复合(composition)的意义和public继承完全不同
- 在应用域，复合意味着`has-a`。在实现域，复合意味着`is-implemented-in-terms-of`(根据某物实现出)

39. 明智而审慎地使用private继承

- private继承意味着`is-implememted-in-terms-of`。他通常比复合的级别低，但当`derived class`需要访问`protected base class`的成员，或需要重新定义继承而来的virtual函数时，这么设计是可以的
- 和复合不同，private继承可以造成empty class最优化

40. 明智而审慎地使用多重继承

- 多重继承比单一继承复杂。他可能导致新的歧义性，以及对virtual继承的需要
- virtual继承会增加大小、速度、初始化复杂度等等成本。如果virtual base class不带任何数据，则是最具实用价值的情况
- 多重继承的确有正当用途。其中一个情节涉及`public继承某个interface class`和`private继承某个协助实现的class`的两相组合

# 第七章 模板与泛型编程

41. 了解隐式接口和编译期多态

- class和template都支持接口和多态
- 对class而言，接口是显式的，以函数签名为中心。多态是通过virtual函数发生于运行期
- 对template参数而言，接口是隐式的，奠基于有效表达式。多态则是通过template具现化和函数重载解析发生于编译期

42. 了解typename的双重意义

- 声明template参数时，前缀关键字class和typename可互换
- 使用关键字typename标识嵌套从属类型名称，但不得在base class lists(基类列)或member initialization list(成员初值列)内一他作为base class修饰符

43. 学习处理模板化基类的名称

- 可在`derived class template`内通过`this->`指涉`base class tmplate`内的成员名称，或藉由一个明白写出的`base class 资格修饰符`完成

44. 将与参数无关的代码抽离template

- template生成多个classes和多个函数，所以任何template代码都不该与某个造成膨胀的template参数产生相依关系
- 因非类型模板参数而造成的代码膨胀，往往可以消除，做法是以函数参数或class成员变量替换template参数
- 因类型模板参数而造成的代码膨胀，往往可降低，做法是让带有完全相同二进制表述的局限类型共享实现码，比如在成员函数中使用T\*作为参数，则可以使用void\*代替该指针，然后由后者完成实际工作

45. 运用成员函数模板接受所有兼容类型

- 使用`member function template`生成`可接受所有兼容类型`的函数
- 如果声明了`member template`用于泛化copy构造或泛化assignmnet操作，还是需要声明正常的copy构造函数和copy assignment操作符


46. 需要类型转换时为模板定义非成员函数

- 当编写一`class template`，而他所提供之`与此template相关的`函数支持`所有参数之隐式类型转换`时，将那些函数定义为`class template内部的friend函数`

47. 使用traits class表现类型信息

- `traits classes`使得类型相关信息在编译期可用，他们以template和template特化完成实现
- 整合重载技术后，traits class有可能在编译期对类型执行测试

48. 认识template元编程

- `Template MetaProgramming`(TMP, 模板元编程)可将工作由运行期移往编译期，因而得以实现早期错误侦测和更高的执行效率
- TMP可被用来生成`基于政策选择组合`(based on combinations of policy choices`)的客户定制代码，也可用来避免生成对某些特殊类型并不适合的代码

# 第八章 定制new和delete

49. 了解new-handler行为

- set_new_handler允许客户指定一个函数，在内存分配无法获得满足时被调用
- `Nothrow new`是一个颇为局限的工具，因为他只适用于内存分配;后继的构造函数调用还是可能抛出异常

50. 了解new和delete的合理替换时机

- 有许多理由需要写个自定的new和delete，包括改善性能、对heap运用错误进行调试、收集heap使用信息

51. 编写new和delete时需固守常规

- `operator new`应该内含一个无穷循环，并在其中尝试分配内存，如果他无法满足内存需求，就该调用new-handler，他也应该有能力处理0 bytes申请。class专属版本则还应该处理`比正确大小更大的错误申请`
- `operator delete`应该在收到null指针时不做任何事，class专属版本则还应该处理`比正确大小更大的错误申请`
- 所谓的处理`比正确大小更大的错误申请`，一般交由`global new或delete`处理

52. 写了placement new也要写placement delete

- 当写了一个`placement operator new`，也要确定写了相应的`placement operator delete`，否则可能会发生隐微而时断时续的内存泄漏
- 当声明`placement new`和`placement delete`时，不要无意识地遮掩了他们的正常版本

# 第九章 杂项讨论

53. 不要轻忽编译器的警告

- 严肃对待编译器发出的警告信息。努力在编译器的最高警告级别下争取`无任何警告`
- 不要过度依赖编译器的报警能力，因为不同的编译器对待事情的态度并不相同。一旦移植到另一个编译器上，原本的警告信息有可能消失

54. 让自己熟悉标准库程序

55. 让自己熟悉Boost

