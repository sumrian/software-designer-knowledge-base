# Java 方法重载与编译期类型

## 定义
方法重载（overloading）是同一作用域内存在多个同名方法，但参数列表不同。编译器根据调用点可见的方法以及实参的编译期类型等信息选择最合适的重载。

## 例子

    class Printer {
        void print(Object value) { System.out.print("Object"); }
        void print(String value) { System.out.print("String"); }
    }

    Object value = "hello";
    new Printer().print(value);

输出为 Object。

原因：虽然变量实际引用的是 String 对象，但变量 value 的编译期类型为 Object，所以调用 print(Object)。

## 易错点
- 重载：依据编译期可用签名和参数类型选择方法。
- 重写：子类提供继承实例方法的新实现；调用重写实例方法时，会根据运行时对象类型进行动态分派。
- 不要只看对象实际类型而忽略变量的静态类型；也不要认为同名方法必然造成编译错误。

## 原创75题记录（2026-10-20）
- 第42题用户选择 B，正确 B，1/1；输出 Object。
- 本题为原创模拟记录，不代表程序设计语言模块已完成全部教学或合规真题校准。
