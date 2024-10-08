## C++知识点

### C语言与C++的区别
C语言没有bool类型 C++有 C++可以重载和c没有<br />
这是因为C++的编译器在编译的时候会带着函数参数的类型 C语言不会
C++是面向对象编程 C是面向过程<br />
C++ 变量检测增强  例如 在全局 定义一个变量 为 int a； 然后还在全局对他初始化 int a=4；这时会报错 但C不会<br />
默认返回值不同 在c++中若函数没有返回值 则必须指定为void  c没有的话默认为int<br />
C++有缺省参数 c没有<br />
C语言中作用域只有两个：局部，全局。C++中则是有：局部作用域，类作用域，名字空间作用域三种。
### 菱形继承
类A,类B，类C都继承A，然后类D继承自类和类C，
#### 会产生数据冗余
类A有一些数据成员，类B和类C继承于A，类B和类C都会有一份自类A的数据副本，当类D继承自类B和类C时，类D实际会有两份来自类A的数据副本，造成数据冗余。<br />
#### 二义性
当类 D 中调用一个在类 A 中定义的方法时，由于类 D 有两条通过类 B 和类 C 到达类 A 的继承路径，编译器无法确定应该使用哪一条路径上的方法，从而导致二义性问题。<br />
可以通过虚继承来解决菱形继承
用类B和类C以虚继承的方式继承类A时，类D只会有一份来自类A的数据副本。<br />
### malloc是如何分配内存的
1.通过brk系统调用从堆分配内存<br />
2.通过mmap系统调用在文件映射区域分配内存<br />
malloc分配的是虚拟内存<br />
他会预分配更大的空间作为内存池<br />
malloc通过brk方式申请的内存，free释放的时候，并不会把内存归还给操作系统，而是缓存在malloc的内存池中<br />
malloc通过mmap方式申请的内存，free释放内存的时候，会把内存归还给操作系统，内存得到真正的释放。<br />
### volatile关键字
提醒编译器他后面所定义的变量随时都有可能被改变，因此编译后的程序每次需要存储或读取这个变量的时候，都会直接从变量地址中读取数据。如果没有volatile关键字，则编译器可能优化读取和存储，可能暂时使用寄存器中的值<br />
### C++将引用作为返回值的好处和应该遵守的规则
- 减少内存开销 
- 提高效率
- 支持链式操作
- 注意不能返回局部变量的引用
### union和struct的区别
#### struct
各成员拥有自己的内存，各自使用互不干涉，同时存在遵循内存对齐原则，一个struct变量总长度等于所有成员的长度之和
#### union
各成员共用一块内存空间，并且同时只有一个成员可以得到这块内存的使用权，各变量共用一个内存首地址，联合体比结构体更节约内存，一个union变量的总长度至少能容纳最大的成员变量，而且要满足是所有成员变量类型大小的整数倍。
在赋值的时候，对于union的不同成员赋值，将会对其他成员进行重写，原来成员的值就不存在了，而对于struct的不同成员赋值是互不影响的。
### 四种强制类型转换
#### static_cast
用于将一种数据类型强制转换为另一种数据类型

#### const_cast
用于强制去掉不能被修改的常数特性，但需要特别主义的是const_cast不是用于去除变量的常量性，而是去除指向常数对象的指针或引用的常量性，其去除常量性的对象必须为指针或引用。
#### reinterpret_cast
改变指针或引用的类型，指针或引用转换为一个足够长度的整形，将整形转换为指针或引用类型
#### dynamic_cast
将基类的指针或引用安全地转换成派生类的指针或引用，并用派生类的指针或引用调用非虚函数。如果是基类指针或引用调用的是虚函数无需转换就能在运行时调用派生类的虚函数
#### dynamic_cast 
的实现原理涉及到一个名为 "vtable" 的虚函数表和一个名为 "type_info" 的运行时类型信息 (RTTI) 系统。
### 迭代器失效
迭代器：是一个遍历各种容器内元素的访问。
它的底层是一个指针 类模板
迭代器失效：就是迭代器底层对应指针所指向的空间被销毁了，而使用一块已经被释放的空间，造成的后果是程序崩溃。
可能会引起的扩容操作都有可能导致迭代器失效，push_back什么的
### this指针
this指针存在于类成员函数中，指向类对象的指针。this是一个关键字，同时也是一个指针常量。
成员函数调用时，传递了一个隐含的参数指向函数所在类对象的地址。<br />
this在成员在成员函数的开始执行前构造，在成员的执行结束后清除。<br />
悬空指针是指向被释放内存的指针，而野指针是不确定其具体指向的指针
### 用户态与内核态
内核态控制的是内核空间的资源管理，用户态访问的是用户空间内的资源
指令的划分 特权不同
用户态--->内核态：唯一途径是通过中断、异常、陷入机制（访管指令）
内核态--->用户态：设置程序状态字PSW
- 于用户态执行时，进程所能访问的内存空间和对象受到限制，其所处于占有的处理器是可被抢占的
- 处于内核态执行时，则能访问所有的内存空间和对象，且所占有的处理器是不允许被抢占的。
- 线程切换只能在内核态完成，如果当前用户处于用户态，则必然引起用户态与内核态的切换，线程的调度是在内核态运行的，而线程中的代码是在用户态运行。

### 拷贝构造为什么参数必须传引用
原文链接：https://blog.csdn.net/xiao23597/article/details/131041511
### 初始化参数列表有什么特点
只能在构造函数中使用<br />
初始化参数列表的初始化顺序和成员变量的顺序一致<br />
常量和引用在初始化参数列表中初始化<br />
初始化参数列表可以调用成员对象的构造函数<br />
当父类没有默认构造函数时，可以利用初始化参数列表调用父类的构造函数<br />
### 什么时候调用拷贝构造
用已经存在的对象初始化新的对象的时候<br />
当对象以值的形式作为函数的参数或返回值时<br />
delete为什么先调用析构函数在调用free<br />
因为先调用free的话，会导致内存泄漏，free直接释放对象的内存了，并没有释放对象所指向的内存<br />
### 指针与引用
原文链接：https://blog.csdn.net/weixin_45805339/article/details/128205810<br />
引用与所引用的变量共用同一块内存空间
### 函数指针
int（*p）（int,int）他可以接收add（int  a，int b） p=add 函数的名字就是地址
{ return a+b；}
### C++空类大小为什么为1
为了实现每个实例在内存中都有一个独一无二的地址，编译器往往会给一个空类隐含的加一个字节，这样空类在实例化后在内存得到了独一无二的地址，所以空类所占的内存大小是1个字节。
### strcpy，sprinty，memcpy的区别
#### 1.操作对象不同
strcpy的操作对象均为字符串
memcpy的两个对象是两个可以任意可操作的内存地址，不限于数据类型。
#### 效率不同
memcpy最快，strcpy慢
#### 实现功能不同
strcpy主要实现字符串变量间的拷贝
sprintf主要实现其他数据类型格式到字符串的转化
memcpy主要是内存块间的拷贝
### 拷贝构造函数调用时机
用已经存在的对象去初始化新对象<br />
作为函数参数和函数返回值<br />
如何只在堆区申请对象<br />
将析构函数私有化，编译器会检查析构函数是否可以被调用 栈区申请内存用alloca()<br />
在栈区的话将new和delete重载为私有化<br />
### const
const修饰变量时表示变量不可以修改，但是可以通过指针来修改<br />
const修饰函数时，表示为常函数，常函数内的this指针是const * const this 所以常函数不能修改成员变量，也不能调用非常函数，这些是因为他们的this指针不匹配<br />
const他作为函数的返回值时，可以防止被修改。<br />
非常函数只能调用常函数，常函数可以调用非常函数也可以调用常函数<br />
常函数和非常函数是重载关系<br />
### C++类对象的初始化顺序
基类初始化，成员类对象初始化，自身构造函数初始化
### 常量指针
const int *p 声明一个指向常量整数的指针，常量指针，指针p可以指向不同的内存地址，但所指向的整数不能通过p来修改。<br />
     const int a = 10;<br />
     const int b = 20;<br />
     const int* p = &a;<br />
     // *p = 15; // 错误，不能通过指针修改所指向的常量整数<br />
     p = &b; // 合法，可以改变指针指向的地址<br />
指针常量
### int * const p
指针常量是指针本身是常量，即指针一旦初始化指向一个地址后，就不能再指向其他地址，但可以通过该指针修改所指向对象的值。<br />
   int c = 30;<br />
   int d = 40;<br />
   int * const p = &c;<br />
   *p = 35; // 合法，可以通过指针修改所指向的值<br />
   // p = &d; // 错误，不能改变指针<br />
### Static
static修饰全局变量
如果想在头文件中定义全局变量，需要定义为静态全局变量，会防止重定义问题<br />
static修饰局部变量
只作用在局部作用域，只会被初始化一次，不会随着函数的结束而被释放。<br />
static修饰成员变量
静态成员变量在编译期间初始化<br />
公有的静态成员变量可以通过类名：：和对象名直接访问
存放在静态区<br />
静态成员变量在继承关系中父类和子类共享
必须在类外初始化，因为静态成员变量是属于类的，不是属于类的任何特定对象，这意味着无论创建了多少个类的实例，静态成员变量都只有一个副本，因此，他需要在类的外部初始化，以确保这个变量的唯一性。<br />
static修饰成员函数时
静态成员函数没有this指针，因此静态成员函数不能访问非静态成员变量
也不能调用非静态成员函数。<br />
### 抽象类为什么不能创建对象
这是因为纯虚函数在虚函数表里存放的地址为0
### 友元函数和友元类
通过友元，一个普通函数或者另一个类中的成员函数可以访问类中的私有成员和保护成员。友元正确的使用能提高程序的运行效率，但同时也破坏了类的封装性和数据的隐藏性。
### 如何用代码判断大小端存储？
大端 字数据的高字节存储在低地址中<br />
小端 字数据的低字节存储在低地址中<br />
强制类型转换 int转为char 只会留下低地址的地方<br />
巧用联合体<br />
### 如何在类外访问私有成员
通过公有方法访问，使用友元函数或友元类，使用指针强制转换reinterpret_cast <br />
### 面向过程语言
优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源;比如单片机、嵌入式开发、 Linux/Unix等一般采用面向过程开发，性能是最重要的因素。<br />
缺点：没有面向对象易维护、易复用、易扩展<br />
### 面向对象语言
优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护
在C++的面试中，面向对象编程（OOP）的概念和应用是经常被提及的话题。为了成功应对这类面试，你需要对OOP的基本原理、特性以及它们在C++中的实现方式有深入的理解。以下是一些常见的C++面向对象面试问题及其回答策略：
回答：面向对象编程是一种编程范式，它将现实世界中的事物抽象为对象，并使用类来定义这些对象的属性和行为。OOP的主要特性包括封装、继承和多态。通过OOP，我们可以创建模块化、可重用和可维护的代码。<br />
- 封装：封装是隐藏对象的属性和实现细节，仅对外提供公共接口的过程。这有助于保护数据的完整性和安全性，同时也提高了代码的可维护性。
- 继承：继承允许我们创建一个新的类（子类或派生类），它继承了一个或多个已存在的类（父类或基类）的属性和方法。这使得代码重用成为可能，并有助于建立类之间的层次结构。
- 多态：多态是面向对象编程的一个重要特性，它允许我们使用父类类型的引用或指针来调用子类的方法。这使得我们可以在运行时动态地确定要调用的方法，从而实现更灵活和可扩展的代码。
### 虚函数
多态
多态分静态多态和动态多态 <br />
静态多态是在编译期间产生的多态 <br />
而动态多态是在运行期间确定的多态 <br />
静态多态包含： 函数重载 运算符重载 函数模板<br />
动态多态包括： 父类指针或引用指向子类对象 并通过指针或引用调用重写函数<br />
动态多态调用过程
首先会通过父类指针或引用访问到子类对象中的虚表指针，然后通过虚表指针找到虚函数表 通过虚函数表存放的虚函数地址去调用虚函数<br />
#### 虚函数表
一个类只有一个 在编译阶段被构造 所有对象共享同一个虚函数表<br />
#### 虚函数表指针
当类中存在虚函数时，编译器会给类增加一个指针类型的变量 放到虚函数表中 创建对象时就会创建一个虚函数表指针，在构造函数被赋值，<br />
因为是在构造函数中被复赋值的 所以构造函数不可以为虚函数 <br />
#### 析构函数可以是虚函数么？
可以 如果产生多态的话  一定要设置虚函数否则在编译时，由于编译器会把父类和子类的构造函数统一命名，那么此时析构函数为函数隐藏，如果父类中的析构函数为虚函数，那么将在子类的析构函数中去调用父类的析构函数，以此来避免内存泄漏<br />

### 内联函数
提高效率 它可以将函数体直接嵌入到调用处，避免了常规函数 调用时的压栈，跳转等操作，如果程序的执行小于开辟栈帧等操作的时间有必要设置为内联函数<br />
不能过于复杂 递归或代码太长<br />
并非总是有效<br />

### 堆和栈

从内存角度上看 堆需要手动申请释放内存 在C++中一般用 new delete c中用 malloc free 栈区的对象不需要回收 由操作系统进行回收 栈的申请速度和释放比堆快的多 因为他是由操作系统来操作的 <br />
栈的地址是高地址向低地址增长，堆的地址是低地址向高地址增长的<br />
从数据结构上看 堆分为 最大堆和最小堆 <br />
 栈  先进后出<br />
### new malloc
new是在堆区申请内存的 如果给类和结构体申请内存的话 会先调用malloc在调用构造函数
利用new创建的数据 返回数据对应的类型的指针<br />
释放new申请的内存需要时候delete 如果是数组delete[]  当这个类的析构函数没有作用时
也可以使用free释放new申请的堆区空间<br />
### new和malloc的区别
new 会先执行malloc 在执行构造函数给成员变量赋值 <br />
 delete 先会执行析构函数 在执行free<br />
 new 返回值不需要强转 malloc 返回值需要强转<br />
 new 是运算符 malloc是c语言库函数<br />
 new 不需要传入具体的字节个数 malloc 需要传具体字节个数<br />
 new 会先执行malloc 在执行 构造函数给成员变量赋值 malloc 只分配堆区<br />
 new 申请失败会抛出异常 malloc会返回空<br />
 new可以重载因为是运算符
### final关键字
修饰虚函数，可以阻止子类重写父类这个函数。修饰类的话，表示不允许被继承
### 移动语义
移动语义可以通过浅拷贝从一个对象转移到另一个对象这样就能减少不必要的临时对象的创建，拷贝以及销毁，大幅度提高性能，正常用对象初始化对象时会调用拷贝构造，如果这个对象占的堆区内存很大，就可以用右值引用进行性能优化。
### 深拷贝浅拷贝
#### 浅拷贝（Shallow Copy）
是指在拷贝对象时，只是复制了对象中的成员变量的值的引用或指针。浅拷贝后的对象和原对象共享一份数据，修改一个对象可能会影响另一个对象。
#### 深拷贝（Deep Copy）
是指在拷贝对象时，会创建一个新的独立的对象，并复制原对象中的所有成员变量的值。深拷贝后的对象和原对象是完全独立的，修改一个对象不会影响另一个对象。
### auto
#### auto
让编译器通过初始值来进行类型推演，从而获得定义变量的类型，所以说auto定义的变量必须由初始值。
#### decltype
它的作用是选择并返回操作数的数据类型，在此过程中，编译器只是分析表达式并得到它的类型，去不进行实际的计算表达式的值。
### Lambda
相当于一个内嵌的匿名函数，用于替换独立函数或者函数对象。<br />
返回值，编译器会根据return语句自动推导返回值类型，但是需要主义的是lambda表达式不能通过列表初始化自动推导返回值类型，像vector，list这种。<br />
sizeof捕获列表：如果lambda捕获了一些外部变量，它的大小将包含这些捕获变量的大小，每个捕获的变量都会在lambda的内部定义一个数据成员<br />
状态：如果lambda没有捕获任何变量，则其大小通常是固定的，1字节。因为编译器需要给每个lambda表达式生成一个独特的类。<br />
### 智能指针
智能指针的底层实现原理主要依赖于引用计数和RAII（‌资源获取即初始化）‌原则。<br />
RALL：是C++语言的一种管理资源、避免资源泄漏的惯用法，利用栈对象自动销毁的特点来实现，这一概念最早由Bjarne Stroustrup提出。因此，我们可以通过构造函数获取资源，通过析构函数释放资源。‌<br />
智能指针的主要目的是解决原始指针使用中的内存管理问题，‌如内存泄漏和悬挂指针等。‌智能指针的实现通常涉及到引用计数的机制，‌当智能指针指向的对象不再被使用时，‌引用计数会减少，‌当引用计数达到零时，‌智能指针会自动释放所指向的对象，‌从而避免内存泄漏。‌此外，‌智能指针还可以提供对对象生命周期的更精细控制，‌例如在需要时延迟对象的销毁或共享对象的所有权等
头文件是<memory><br />
#### shared_ptr
使用引用计数的方式来管理内存。多个shared_ptr可以指向同一个对象，当最后一个shared_ptr被销毁时，对象才会被释放。适用于多个对象需要共享同一个资源的场景，比如在多个函数之间传递一个对象，并且需要保证在所有使用该对象的地方都不再需要时才释放资源。
#### unique_ptr
直接防止拷贝的方式解决智能指针的拷贝问题，简单而又粗暴，防止智能指针对象拷贝，保证资源不会被多次释放。
可以通过move函数转移给其他的unique_ptr，
unique_ptr是独占式的智能指针，它保证在任何时刻只有一个unique_ptr指向一个对象，当unique_ptr被销毁时，它所指向的对象也会被自动释放。适用于需要独占资源的场景，比如在函数内部创建一个对象并返回给调用者时，可以使用unique_ptr来确保资源的正确释放。
#### weak_ptr
expired() 它是来判断观测的资源是否被释放
lock 获取管理所监测资源的share_ptr对象
reset 置零 其他减一
std::weak_ptr是一种弱引用的智能指针，它不会增加对象的引用计数，主要用于解决std::shared_ptr可能出现的循环引用问题。当需要观察一个由std::shared_ptr管理的对象，但又不想影响对象的生命周期时，可以使用std::weak_ptr。<br />
