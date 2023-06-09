#### 重学Python面向对象
1. 注意: 
    + 类名称首字母大写,编写规范 
    + py3 默认类都继承object类
2. 对象和 self 
    + `__init__` 初始化方法, 在实例化对象时会自动执行
    + self 本质上相当于实例化对象在内存中的地址,外界实例化对象调用类方法的时候会在类中寻找方法并将self 传递过去
    + 而对象 就是基于类实例化出来的一块地址,这块地址有个名字 就是实例化对象的名字. 只有经过`__init__`初始化方法后,会设置一些属性
3. 常见成员
    + 实例变量,属于对象. 只能通过对象来调用
        + 在实例变量取值时,实例变量没有会向上找. 而对实例变量设置值时,直接向这块内存地址设置值不会牵扯到类.
        + 多继承的读与写 也是按照继承顺序来查找
    + 类变量, 属于类,可以被所有对象共享,类似于给所有类的实例化对象增加一个 全局变量 这个变量就叫做 类变量
    + 
    + **绑定方法 默认有一个self**,由对象调用. 直接写def 方法名,属于类. 通过对象调用 也可以用过类直接调用 但**主要都是对象调用**
        + 第一种执行方法  实例化对象.方法名(参数) 调用
        + 第二种 **类名.方法名(实例化对象, 参数)** 来调用   很少这样写
        + 当一个方法需要用到 self 的时候 就用绑定方法
    + **类方法 @classmethod修饰 以及 默认有一个cls** 类或对象都可以调用. cls 相当于当前这个类  **主要都是 类直接调用**
        + 当一个方法需要用到当前类的时候 句用类方法
    + **静态方法 @staticmethod修饰 默认没有参数**   对象与类都可以调用
    + 
    + 属性: 由 绑定方法 + 特殊装饰器 @property 组合创造出来的,方便调用的时候 不需要加括号
        + 本质还是一个方法,只是加了装饰器之后 可以不需要加括号来调用.可以直接当属性来使用
    + 一般不可出现属性 和 实例属性 一致的情况 
4. 成员修饰符
    + 公有 任何地方都可以调用这个成员
    + 私有 只有类的内部方法 才可以调用这个成员  一般以 两个下划线开头表示私有成员 实例属性 以及方法 都是一样写法
    + 但！！ 父类的私有成员 子类无法继承到
    + 若 强制性使用类的私有成员 可以 类的实例化对象._类名__私有成员名  这样来使用
5. 三大特性之封装
    + 将同一类方法封装到一个类中,提升代码利用率
    + 将属性都封装到对象中,在实例化一个对象时,通过`__init__` 来构造实例化对象
6. 三大特性之继承
    + 父类 - 子类 |  基类 - 派生类
    + 即拥有父类的一些属性和方法,又可以修改继承到父类的方法以及拥有自己的独特的方法.
    + 多继承
        + 优先级-寻找方法或属性的顺序: 先左边后右边 广度优先 先找到先执行  新式类 Python3 默认都是新式类
        + 明白self 具体是哪个对象的实例 最关键！
7. 三大特性之多态
    + 鸭子类型 多态的一种风格 由于python中 类型可以随意变化,所以不需要向Java 先写接口 在写实现 最后完成多态的功能.
8. 面向对象之间的嵌套关系, 某个类的实例属性 为 另一个类的
   + 可以表示层级关系,顶级的实例化对象作为 次顶级初始化时某个实例属性的参数,这样就实现了层级关系 包含关系 等等
9. 面向对象之 特殊成员
    + `__init__` 初始化方法
    + `__new__` 构造方法
        + 在 初始化方法之前 触发, 会调用 `object.__new__(cls)` 然后返回 先创建空对象返回 然后再进入初始化方法
    + `__call__ `
        + 类名() 会执行new 和init 方法
        + 类实例化对象() 会自动执行 call 方法
    + `__str__` 返回字符串
        + 当对实例化对象进行强制转字符串的时候,就会调用这个方法.  str(实例化对象)
        + 直接打印 实例化对象时 也会默认执行这个方法
    + `__dict__`  会将实例化对象 中实例属性 来构造一个dict 
        + 实例化对象.__dict__
    + `__getitem__`、`__setitem__`、`__delitem__`
        + 相当于给 实例化对象 增加了 类似字典的功能
        + `实例化对象['key1'] = value1` 会自动触发 `__setitem__` 方法
        + `实例化对象['key1']` 会自动触发 `__getitem__` 方法
        + `del 实例化对象['key1']` 会自动触发 `__delitem__` 方法
    + `__enter__`、`__exit__`  使 实例化对象 拥有 上下文管理器. 一般 两者成对出现
        + 如 with 实例化对象 as f  此时的f 就是 enter 返回的对象
        + 当with 执行完毕 会进入exit 方法
    + `__add__` 两个实例化对象 相加 默认会调用这个方法 默认有个参数 other 类似于 重载运算符
    + `__iter__` 关键 重要  延伸出 迭代器 生成器  以及 可迭代对象 
        + 迭代器：
            + 当类中 定义了 `__iter__` 以及 `__next__` 两个方法
            + `__iter__` 需要返回对象本身 self
            + `__next__` 返回一个数据,若没有数据 就抛出一个 StopIteration异常.
            + for 循环先执行 iter 然后在执行 next 直到没有数据抛出异常就停止循环
        + 生成器 是一种特殊的迭代器
            + 先创造一个生成器函数, 函数返回用yield 来表示  
            + 在创建一个生成器对象 对象 =  函数() 
        + 可迭代对象
            + 一个类 实现了 iter 和next 那就是迭代器类  如果实例化一个对象那就是 迭代器对象
            + 一个类 仅实现了 iter  并返回一个迭代器对象  乘 这个类实例化对象为 可迭代对象
            + 循环 可迭代对象时 内部先执行 对象.__iter__ 来获取 迭代器对象 然后不断的调用next 方法来执行
            + 如 range 就是一个典型的 可迭代对象 
            + 而 生成器形成可迭代对象 只需在第二步 仅仅实现一个iter 并每次yield 出去一个值 就类似于完成了可迭代对象这一操作
10. mro 和C3 算法
    + 可通过 类名.mro() 获取当前类的继承关系 成员寻找顺序
    + C3算法
        + 先获取直接继承的类,
        + 若有多个类直接继承于同一个基类,最靠右边的派生类查找的时候才会查找这个共同的基类.
        + 从左到右 然后 深度优先 遇到菱形继承 最后顶端
11. 内置函数
    + callable  是否可以在后面加括号 来执行
        + 函数 类名  类中实现了__call__ 的方法的实例化对象  
    + super 按照mro的继承顺序来向上找成员
        + 派生类 调用 基类的某个方法时，出现了super().方法名  具体super 指向的是谁 取决于方法名 self 指向的是哪个对象,就从这个对象的mro继承关系继续往后找
        + super(派生类类名,self).__init__(父类初始化参数) 初始化父类 
    + isinsatance
12. 反射 提供了一种更加灵活的方式实现对象中操作成员 
    + getattr(实例化对象,“实例变量名 或 方法名”)
    + setatter(实例化对象,key，value)
    + hasatter(实例化对象,“实例变量名 或 方法名”)
    + delatter(实例化对象,“实例变量名 或 方法名”)
13. 一切皆对象
    + 



   
#### 面向对象 

1. 用 @classmethod 修饰的函数  类函数  函数第一个参数 cls
    + 类函数最常用的功能是实现不同的 init 构造函数
2. 什么都不修饰的函数  成员函数   函数第一个参数为 self  
    + 成员函数则是我们最正常的类的函数，它不需要任何装饰器声明，
    + 第一个参数 self 代表当前对象的引用，可以通过此函数，来实现想要的查询 / 修改类的属性等功能
3. 用 @staticmethod 修饰的函数 静态函数

类函数 和 成员函数 可以访问和修改对象属性
每个类都有构造函数，继承类在生成对象的时候，是不会自动调用父类的构造函数的 ，
因此你必须在 init() 函数中显式调用父类的构造函数。它们的执行顺序是 子类的构造函数 -> 父类的构造函数。

最后需要注意到 print_title() 函数，这个函数定义在父类中，但是子类的对象可以毫无阻力地使用它来打印 title，
这也就体现了继承的优势：减少重复的代码，降低系统的熵值（即复杂度）

4. 抽象类 抽象函数
    + 抽象类是一种特殊的类，它生下来就是作为父类存在的，一旦对象化就会报错。同样，抽象函数定义在抽象类之中，子类必须重写该函数才能使用。相应的抽象函数，则是使用装饰器 @abstractmethod 来表示
5. 面向对象编程四要素是类、属性、函数（方法）、对象（实例）， 它们关系可以总结为：类是一群具有相同属性和函数的对象的集合。
6. 思考题  多重继承的查找顺序
